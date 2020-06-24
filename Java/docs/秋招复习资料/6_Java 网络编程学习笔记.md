# Java 网络编程学习笔记

## 前置概念

### **Java IO 模型**

| IO 模型              | 对应的 Java 版本 |
| -------------------- | ---------------- |
| BIO（同步阻塞 IO）   | 1.4 之前         |
| NIO（同步非阻塞 IO） | 1.4              |
| AIO（异步非阻塞 IO） | 1.7              |

---

### Linux 内核 IO 模型

- **阻塞 IO**

  最传统的一种 IO 模型，在读写数据过程中会发生阻塞。

  当用户线程发出 IO 请求后，内核会去查看数据是否就绪，如果没有就绪就会等待数据就绪，而用户线程就会处于阻塞状态，用户线程交出CPU。当数据就绪之后，内核会将数据拷贝到用户线程，并返回 IO 执行结果给用户线程，用户线程解除阻塞状态并开始处理数据。

  对应于 Java 中的 BIO。

- **非阻塞 IO**

  当用户线程发起 IO 请求后并不需要等待，即使内核数据还没有准备好也会马上得到内核返回的一个结果，用户线程可以之后再次询问内核。一旦内核中的数据准备好了，并且又再次收到了用户线程的请求，那么内核就将数据拷贝到用户线程并通知用户线程。

  在非阻塞 IO 中，用户线程需要不断询问内核数据是否准备就绪，在数据未就绪时可以处理其他任务。

- **多路复用 IO**

  在多路复用 IO 模型中会有一个 Selector 线程不断轮询多个 Socket 的状态，只有当 Socket 真正有读写事件时才通知用户线程进行实际的 IO 读写操作。阻塞 IO 和 非阻塞 IO 模型需要为每个 Socket 建立一个单独的线程处理数据，而多路复用 IO 只需要一个线程管理多个 Socket，并且只在真正有读写事件时才会使用操作系统的 IO 资源，大大节约了系统资源。

  在非阻塞 IO 中不断询问 Socket 状态是通过用户线程进行的，而在多路复用 IO 中轮询每个 Socket 状态是内核在进行的，效率要比用户线程高。

  对于多路复用 IO 模型来说在事件响应体很大时，Selector 线程会成为性能瓶颈，导致后续事件无法及时处理，影响下一轮事件轮询，因此实际应用中方法体内不做复杂逻辑处理，只做数据的接收和转发，而将具体业务操作转发给业务线程处理。

  对应于 Java 中的 NIO，在 NIO 中通过 `selector.select()` 去查询每个 Channel 是否有事件到达，如果没有事件到达用户线程会一直阻塞，因此 NIO 也会导致用户线程的阻塞。

- **信号驱动 IO**

  当用户线程发起一个 IO 请求操作，会给对应的 Socket 注册一个信号函数，然后用户线程会继续执行，当内核数据就绪时会发送一个信号给用户线程，用户线程接收到信号之后，便在信号函数中调用 IO 读写操作来进行实际的 IO 请求操作。

  一般用于 UDP 中。

- **异步 IO**

  当用户线程发起异步 read 操作后，立刻就可以开始去做其它事。另一方面，从内核的角度，当它收到一个异步 read 之后会立刻返回一个状态，说明请求是否成功发起，用户线程不会任何阻塞。然后内核会等待数据准备完成并将数据拷贝到用户线程，完成后内核会给用户线程发送一个信号通知 read 操作已完成。

  用户线程完全不需要关心实际的整个 IO 操作是如何进行的，只需要先发起一个请求，当接收内核返回的成功信号时表示 IO 操作已经完成，就可以直接去使用数据了。

  在异步IO模型中，IO操作的两个阶段都不会阻塞用户线程，这两个阶段都是由内核自动完成，然后发送一个信号告知用户线程操作已完成，用户线程**不需要再次调用 IO 函数进行具体的读写**。在信号驱动模型中，当用户线程接收到信号表示数据已经就绪，然后需要用户线程调用IO函数进行实际的读写操作。

  对应于 Java 中的 AIO。	

<img src="https://img2018.cnblogs.com/blog/1748170/201909/1748170-20190906210650873-272348693.png" style="zoom:80%;" />

---

### URL 解析与构造

当在浏览器输入： `http://www.google.com ` 时,

可能发出的实际请求是： `http://www.google.com:80/search?q=test&safe=strict`

其中 `http` 表示应用协议，`www.google.com` 表示域名，`80` 表示端口（可以明确指定要进行数据交换的进程），`search` 表示路径（请求提供的服务），`q=test&safe=strict` 表示请求参数。

其中域名通过 DNS 域名解析系统将域名解析为主机的 IP 地址，解析时是从右向左解析的，`www.google.com` 实际上是`www.google.com.root`，`.root` 是根域名，一般会省略。

域名的层级如下：

| 域名层级 | 实例                             |
| -------- | -------------------------------- |
| 根域名   | `.root`                          |
| 顶级域名 | `.com`、  `.edu` 、  `.org`      |
| 次级域名 | `.google`  、 `.baidu` 、  `.qq` |
| 主机名   | `www `                           |

DNS 本质是一个分布式数据库，域名对应 IP 地址的映射关系存储在不同层级的域名服务器上，查询方式分为递归查询和迭代查询。

**递归查询：**浏览器将域名发给 DNS 客户端，DNS 客户端将查询请求发送给根域名服务器，根域名服务器如果知道对应 IP 地址将返回结果，否则发送给其已知的顶级域名服务器查询，顶级域名如果知道对应的 IP 地址就返回结果，否则就发给其已知的二级域名服务器查询，二级域名服务器也进行类似操作，最终将结果一路返回给浏览器。

**迭代查询：**DNS 客户端将请求发送给根域名服务器，如果根域名服务器不知道对应 IP 地址不会去请求顶级域名服务器，而是把已知的顶级域名服务器返回给 DNS 客户端（不会帮 DNS 客户端去查询），DNS 客户端得到顶级域名服务器地址后就再去请求顶级域名服务器发送查询请求，以此类推。

不管使用哪种方式，一旦查询成功，都会将结果缓存在查询经过的域名服务器、DNS 客户端或浏览器上。根域名服务器很少，一般其地址都内置在 DNS 客户端中。

---

### 网络分层

| 网络分层   | 数据格式          | 协议                | 作用                                                         |
| ---------- | ----------------- | ------------------- | ------------------------------------------------------------ |
| 应用层     | 报文              | HTTP、FTP、SMTP     | 通过应用进程之间的交互来完成特定网络应用。                   |
| 运输层     | 报文段/用户数据报 | TCP、UDP            | 向两台主机进程之间的通信提供的数据传输服务。                 |
| 网络层     | 分组/包           | IP、ICMP、IGMP、ARP | 为分组交换网上的不同主机提供通信服务。                       |
| 数据链路层 | 帧                | PPP、CSMA/CD        | 将网络层 IP 数据报组装成帧，在两个相邻结点之间的链路上传输。 |
| 物理层     | 0、1电信号        | FDDI                | 屏蔽掉传输媒体和通信手段的差异。                             |

---

### java.io

网络编程的本质是进程间的通信，通信的基础是 IO 模型

IO 流分类：

<img src="http://assets.processon.com/chart_image/5edb0f207d9c0866361b02d2.png" style="zoom:40%;" align='left'/>

java.io 包中用到了装饰器模式：

例如创建 BufferedInputStream 对象时必须传入一个 InputStream 对象（如 FileInputStream 对象）作为参数，可以利用缓冲区在内存读写数据提高效率。

---

### Socket 概述

Socket 也是一种数据源，是网络通信的端点，可以把 Socket 理解为一个唯一的 IP 地址和端口号，例如 `127.0.0.1:8080`。

发送数据：

应用进程创建 Socket 并绑定到网卡的驱动程序，通过 Socket 发送数据，网卡驱动程序从 Socket 读取数据并发送。

接收数据：

应用进程创建 Socket 并绑定到网卡的驱动程序，网卡驱动程序从网络中接收到数据并传输给 Socket，Socket 将数据再传输给应用进程。

---

### 同步/异步/阻塞/非阻塞

同步和异步是通信机制，阻塞和非阻塞是调用状态。

- 同步 IO 是用户线程发起 I/O 请求后需要等待或者轮询内核 I/O 操作完成后才能继续执行。

- 异步 IO 是用户线程发起 I/O 请求后仍可以继续执行，当内核 I/O 操作完成后会通知用户线程，或者调用用户线程注册的回调函数。

- 阻塞 IO 是指 I/O 操作需要彻底完成后才能返回用户空间 。
- 非阻塞 IO 是指 I/O 操作被调用后立即返回一个状态值，无需等 I/O 操作彻底完成。

---

### 线程池

线程池可以通过复用线程，减小线程创建和销毁的开销，提高程序效率。Executors 类提供了几种静态方法直接创建线程池：

- newSingleThreadExecutor 

  线程池中只有一个线程，不断复用

- newFixedThreadPool

  线程池中的线程数量是固定的

- newCachedThreadPool

  提交的任务如果有空闲线程则处理，否则创建一个新线程来处理任务

- newScheduledThreadPool

  提交的任务可以在指定的时间执行

---

## BIO 同步阻塞

BIO 即同步阻塞式 IO，每一个客户端请求都对应一个线程来处理。

服务器端通常由一个独立的 Acceptor 线程负责监听客户端的连接。一般通过在 `while(true)` 循环中调用 `accept()` 方法接收客户端连接的方式监听连接请求，请求一旦接收到一个连接请求，就可以建立通信套接字在这个通信套接字上进行读写操作，此时不能再接收其他客户端连接请求，只能等待同当前连接的客户端的操作执行完成， 可以通过多线程来支持多个客户端的连接。

### 模拟 TCP 通信

Socket类 是客户端的 Socket 连接，使用步骤：

- 创建对象时要提供服务器的主机和端口参数。
- 数据通信结束后通过 close 方法关闭连接。

ServerSocket 类是服务器端的 Socket 连接，使用步骤：

- 创建对象时要提供端口参数进行绑定，服务器会监听该端口，任何发送到该端口的信息都会被服务器接收并处理。

- 通过 accept 方法获取客户端连接，通过该连接与客户端进行数据通信，是一种**阻塞式**调用。

---

服务器的代码：

```java
public class Server {

    public static void main(String[] args) {
        // 退出条件
        final String QUIT = "quit";
        // 服务器端口
        final int PORT = 8080;
        // 服务器的 socket 对象
        ServerSocket serverSocket = null;

        try {
            // 绑定监听端口
            serverSocket = new ServerSocket(PORT);
            System.out.println("启动服务器，监听端口：" + PORT);

            while (true){
                // 等待客户端连接
                Socket socket = serverSocket.accept();
                System.out.println("客户端[" + socket.getPort() + "]已连接");
                // 获取和客户端通信的字符输入流和字符输出流
                BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

                // 读取客户端发送的消息
                String message;
                while ((message = br.readLine()) != null){
                    System.out.println("客户端[" + socket.getPort() + "]发送消息：" + message);

                    // 回复客户端
                    bw.write("服务器已经收到消息：" + message + "\n");
                    bw.flush();

                    // 查看客户端是否关闭
                    if (QUIT.equals(message)){
                        System.out.println("客户端[" + socket.getPort() + "]已断开连接");
                        break;
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 释放资源
            if(serverSocket != null){
                try {
                    serverSocket.close();
                    System.out.println("关闭 serverSocket");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

---

客户端的代码：

```java
public class Client {

    public static void main(String[] args) {
        // 退出条件
        final String QUIT = "quit";
        // 服务器主机和端口
        final String HOST = "127.0.0.1";
        final int PORT = 8080;
        // 客户端的 socket 对象
        Socket socket = null;
        BufferedWriter bw = null;

        try {
            // 创建 socket
            socket = new Socket(HOST, PORT);

            // 获取和服务器通信的字符输入流和字符输出流
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

            // 等待用户输入信息
            while (true) {
                System.out.println("请输入要发送的消息：");
                BufferedReader consoleReader = new BufferedReader(new InputStreamReader(System.in));
                String input = consoleReader.readLine();

                // 发送消息给服务器
                bw.write(input + "\n");
                bw.flush();

                // 读取服务器返回的消息
                String message= br.readLine();
                if (message != null) {
                    System.out.println("服务器发送消息：" + message);
                }

                // 查看用户是否退出
                if(QUIT.equals(input)){
                    break;
                }
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            // 释放资源
            try {
                if(bw != null) {
                    bw.close();
                    System.out.println("关闭 socket");
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

---

### 基于 BIO 的多人聊天室

- 基于 BIO 模型
- 支持多人同时在线
- 每个用户的发言都会被转发给其他在线用户

一共由四个类组成：

- 服务器类：ChatServer

  ```java
  public class ChatServer {
  
      /** 服务器端口 */
      private final int PORT = 8080;
  
      /** 结束条件 */
      private final String QUIT = "quit";
  
       /** 服务器端的 socket 连接 */
      private ServerSocket serverSocket;
  
       /** 存储在线用户 */
      private final Map<Integer, Writer> clients;
  
      public ChatServer(){
          clients = new HashMap<>();
      }
  
  
      /**
       * 添加在线用户
       * @param socket 上线的客户端 socket 连接对象
       * @throws IOException 可能产生的异常
       */
      public synchronized void addClient(Socket socket) throws IOException {
          if(socket != null) {
              int port = socket.getPort();
              BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
              clients.put(port, writer);
              System.out.println("客户端[" + port + "]已连接到服务器");
          }
      }
  
      /**
       * 移除离线用户
       * @param socket 下线的客户端 socket 连接对象
       * @throws IOException 可能产生的异常
       */
      public synchronized void removeClient(Socket socket) throws IOException {
          if(socket != null){
              int port = socket.getPort();
              // 如果当前用户仍在在线用户集合中则移除
              if(clients.containsKey(port)){
                  clients.get(port).close();
              }
              clients.remove(port);
              System.out.println("客户端[" + port + "]已断开连接");
          }
      }
  
      /**
       * 转发消息给其他用户
       * @param socket 要发送消息的客户端的 socket 连接对象
       * @param message 要发送的消息
       * @throws IOException 可能产生的异常
       */
      public synchronized void forwardMessage(Socket socket, String message) throws IOException {
          for (Integer port : clients.keySet()){
              // 遍历在线用户，通过比较端口号判断当前用户是否是发送消息的用户，如果不是则转发消息
              if(!port.equals(socket.getPort())){
                  Writer writer = clients.get(port);
                  writer.write(message);
                  writer.flush();
              }
          }
      }
  
      /**
       * 查看用户是否准备退出
       * @param message 用户发送的消息
       * @return true 表示准备退出，反之
       */
      public boolean checkQuit (String message) {
          return QUIT.equals(message);
      }
  
      /**
       * 释放资源
       */
      public synchronized void close() {
          if(serverSocket != null){
              try {
                  serverSocket.close();
                  System.out.println("关闭 serverSocket");
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  
      /**
       * 启动服务器端开始监听
       */
      public void start() {
          try {
              // 绑定监听端口
              serverSocket = new ServerSocket(PORT);
              System.out.println("服务器已经启动，监听端口：" + PORT);
              while (true) {
                  // 等待获取客户端连接
                  Socket socket = serverSocket.accept();
                  // 创建线程处理客户端连接
                  new Thread(new ChatHandler(this, socket)).start();
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              //释放资源
              close();
          }
      }
  
      /**
       * 用 main 线程模拟 Acceptor 线程
       * @param args main方法参数
       */
      public static void main(String[] args) {
          ChatServer chatServer = new ChatServer();
          // 启动服务器
          chatServer.start();
      }
  }
  ```

- 服务器处理客户端连接类：ChatHandler

  ```java
  public class ChatHandler implements Runnable{
  
      /** 服务端 */
      private ChatServer chatServer;
  
      /** 客户端连接 */
      private Socket socket;
  
      public ChatHandler(ChatServer chatServer, Socket socket) {
          this.chatServer = chatServer;
          this.socket = socket;
      }
  
      /**
       * 处理客户端连接
       */
      @Override
      public void run() {
          try {
              // 添加新上线用户
              chatServer.addClient(socket);
              // 读取用户发送的信息
              BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
              String message;
              while ((message = reader.readLine()) != null) {
                  String forwardMessage = "客户端[" + socket.getPort() + "]发送了一条消息：" + message + "\n";
                  System.out.println(forwardMessage);
                  // 转发消息给其他在线用户
                  chatServer.forwardMessage(socket, forwardMessage);
                  // 查看用户是否准备退出
                  if(chatServer.checkQuit(message)){
                      break;
                  }
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              // 移除离线用户
              try {
                  chatServer.removeClient(socket);
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ```

- 客户端类：ChatClient

  ```java
  public class ChatClient {
  
      /** 服务器主机 */
      private final String HOST = "127.0.0.1";
  
      /** 服务器端口 */
      private final int PORT = 8080;
  
      /** 结束条件 */
      private final String QUIT = "quit";
  
      /** 和服务器通信的 socket */
      private Socket socket;
  
      /** 和服务器通信的输入流 */
      private BufferedReader reader;
  
      /** 和服务器通信的输出流 */
      private BufferedWriter writer;
  
      /**
       * 发送信息给服务器
       * @param message 要发送的信息
       * @throws IOException 可能抛出的异常
       */
      public void send(String message) throws IOException {
          // 确保输出流是开放状态
          if(!socket.isOutputShutdown()){
              writer.write(message + "\n");
              writer.flush();
          }
      }
  
      /**
       * 从服务器端接收消息
       * @return 返回接受的消息
       * @throws IOException 可能抛出的异常
       */
      public String receive() throws IOException {
          String message = null;
          // 确保输入流是开放状态
          if(!socket.isInputShutdown()){
              message = reader.readLine();
          }
          return message;
      }
  
      /**
       * 查看用户是否准备退出
       * @param message 用户发送的消息
       * @return true 表示准备退出，反之
       */
      public boolean checkQuit (String message) {
          return QUIT.equals(message);
      }
  
      /**
       * 释放资源
       */
      public synchronized void close() {
          if(writer != null){
              try {
                  writer.close();
                  System.out.println("关闭 socket");
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  
      /**
       * 启动客户端
       */
      public void start() {
          try {
              // 创建 socket
              socket = new Socket(HOST, PORT);
              // 创建 IO 流
              reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
              writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
              // 创建新线程处理用户的输入
              new Thread(new UserInputHandler(this)).start();
              // 读取服务器转发的消息
              String message;
              while ((message = receive()) != null){
                  System.out.println(message);
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              //释放资源
              close();
          }
      }
  
      public static void main(String[] args) {
          ChatClient chatClient = new ChatClient();
          // 启动客户端
          chatClient.start();
      }
  }
  ```

- 客户端处理用户输入类：UserInputHandler

  ```java
  public class UserInputHandler implements Runnable{
  
      /** 客户端 */
      private ChatClient chatClient;
  
      public UserInputHandler(ChatClient chatClient) {
          this.chatClient = chatClient;
      }
  
      /**
       * 处理用户输入信息
       */
      @Override
      public void run() {
          try {
              // 等待用户输入信息
              BufferedReader consoleReader = new BufferedReader(new InputStreamReader(System.in));
              while (true) {
                  String message = consoleReader.readLine();
                  // 向服务器发送消息
                  chatClient.send(message);
                  // 检查用户是否准备退出
                  if(chatClient.checkQuit(message)){
                      break;
                  }
              }
          }catch (IOException e){
              e.printStackTrace();
          }
      }
  }
  ```

---

### 伪异步 IO 改进多人聊天室

在基于 BIO 的多人聊天室中，一个客户端请求对应着一个线程，当用户数量很大时线程的开销也会很大。我们可以通过线程池来实现伪异步 IO 来进行优化，限制线程的总数量，实现线程的复用。

在 ChatServer 中添加属性并修改构造器：

```java
/** 线程池 */
private ExecutorService executorService;

public ChatServer(){
    // 限制服务器处理请求的线程数为 10
    executorService = Executors.newFixedThreadPool(10);
    clients = new HashMap<>();
}
```

同时修改 ChatServer 的 start 方法中创建线程部分的代码，改为使用线程池：

```java
while (true) {
    // 等待获取客户端连接
    Socket socket = serverSocket.accept();
    // 将处理请求任务提交给线程池执行
    executorService.execute(new ChatHandler(this, socket));
}
```

---

## NIO 同步非阻塞

BIO 中的阻塞：

- ServerSocket 的 accept()
- InputStream 和 OutputStream 的读和写
- 无法在同一个线程中处理多个 Stream IO

可以使用非阻塞的 IO 即 NIO，在 NIO 中：

- 使用 Channel 代替 Stream，Channel 是双向的，既可以读数据也可以写数据，既可以阻塞读写也可以非阻塞读写
- 使用 Selector 监控多条 Channel
- 可以在一个线程里处理多个 Channel IO

---

**NIO 的核心组件：**

- **Selector**

  选择器或多路复用器，主要作用是轮询检查多个 Channel 的状态，判断 Channel 注册的事件是否发生，即判断 Channel 是否处于可读或可写状态。

  在使用之前需要将 Channel 注册到 Selector 上，注册之后会得到一个 SelectionKey。

  SelectionKey 的相关方法：

  - interestOps() ： 查看 Channel 对象注册的状态集合，例如 Accept（接收到一个客户端请求）、Read、Wirte 状态。
  - readyOps() ：查看 Channel 对象可操作的状态集合
  - channel() ：获取 Channel 对象
  - selector()：获取 Selector 对象
  - attachment()：附加一个对象或更多信息

  使用 Selector 选择 Channel：

  - select()：获取处于就绪状态的 Channel 对象数量

- **Channel**

  双向通道，替换了 IO 中的 Stream，不能直接访问数据，要通过 Buffer 来读写数据，也可以和其他 Channel 交互。

  分类：

  - FileChannel（处理文件）

  - DatagramChannel（处理 UDP 数据）

  - SocketChannel（处理 TCP 数据，用作客户端）

  - ServerSocketChannel（处理 TCP 数据，用作服务器端）。

- **Buffer**

  缓冲区，本质是一块可读写数据的内存，这块内存被包装成 NIO 的 Buffer 对象，用来简化数据的读写。Buffer 的三个重要属性：position 表示下一次读写数据的位置，limit 表示本次读写的极限位置，capacity 表示最大容量。

  -  `flip()` 将写转为读，底层实现原理是把 position 置 0，并把 limit 设为当前的 position 值。
  - 通过`clear()` 将读转为写模式（用于读完全部数据的情况，把 position 置 0，limit 设为 capacity）。
  - 通过`compact()` 将读转为写模式（用于没有读完全部数据，存在未读数据的情况，让 position 指向未读数据的下一个）。
  - <font color ="red">**通道的方向和 Buffer 的方向是相反的，读取数据相当于向 Buffer 写入，写出数据相当于从 Buffer 读取。**</font>

  使用步骤：

  - 向 Buffer 写入数据
  - 调用 flip 方法将 Buffer 从写模式切换为读模式
  - 从 Buffer 中读取数据
  - 调用 clear 或 compact 方法来清空 Buffer

---

### 本地文件拷贝

使用 4 种方法进行本地文件的拷贝，分别是不使用/使用缓冲区的 BIO 方式，使用 Buffer/直接使用 Channel 的 NIO 方式。

其中不使用缓冲区的 BIO 效率最低，其余效率差不多，直接使用 Channel 的 NIO 方式效率较高。

```java
/** 拷贝文件的接口 */
public interface FileCopy {

    /**
     * 拷贝文件
     * @param source 拷贝文件源
     * @param target 拷贝文件目的地
     */
    void copyFile(File source, File target);
}

class FileCopyDemo {
    
    private static void close(Closeable closeable) {
        if (closeable != null) {
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {

        // BIO 不使用缓冲区
        FileCopy noBufferStreamCopy = (source, target) -> {
            InputStream is = null;
            OutputStream os = null;
            try {
                is = new FileInputStream(source);
                os = new FileOutputStream(target);
                int read;
                while ((read = is.read()) != -1) {
                    os.write(read);
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                close(is);
                close(os);
            }
        };

        // BIO 使用缓冲区
        FileCopy bufferStreamCopy = (source, target) -> {
            InputStream is = null;
            OutputStream os = null;
            try {
                is = new BufferedInputStream(new FileInputStream(source));
                os = new BufferedOutputStream(new FileOutputStream(target));
                int read;
                byte[] buffer = new byte[1024];
                while ((read = is.read(buffer)) != -1) {
                    os.write(buffer, 0 ,read);
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                close(is);
                close(os);
            }
        };

        // NIO 使用 Buffer 交互
        FileCopy nioBufferCopy = (source, target) -> {
            FileChannel in = null;
            FileChannel out = null;
            try {
                in = new FileInputStream(source).getChannel();
                out = new FileOutputStream(target).getChannel();
                // 分配一个大小为 1024KB 的缓冲区
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                while ((in.read(buffer)) != -1) {
                    // 将 Buffer 的写模式转为读模式（读取数据 = 向 Buffer 写）
                    buffer.flip();
                    // 只要还有数据就全部写出
                    while (buffer.hasRemaining()) {
                        out.write(buffer);
                    }
                    // 将 Buffer 的读模式转为写模式（写出数据 = 从 Buffer 读）
                    buffer.clear();
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                close(in);
                close(out);
            }
        };

        //NIO 直接使用 Channel 交互
        FileCopy nioTransferCopy = (source, target) -> {
            FileChannel in = null;
            FileChannel out = null;
            try {
                in = new FileInputStream(source).getChannel();
                out = new FileOutputStream(target).getChannel();
                long transferred = 0;
                long size = in.size();
                while (transferred != size) {
                    transferred += in.transferTo(0, size, out);
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                close(in);
                close(out);
            }
        };
    }
}
```

---

### 使用 NIO 改写多人聊天室

服务器端代码：

```java
public class ChatServer {

    /** 服务器端口 */
    private static final int DEFAULT_PORT = 8080;

    /** 结束条件 */
    private static final String QUIT = "quit";

    /** 缓冲区大小 */
    private static final int BUFFER_SIZE = 1024;

    /** 服务端 Channel */
    private ServerSocketChannel server;

    /** Selector 选择器 */
    private Selector selector;

    /** 读缓冲 */
    private ByteBuffer readBuffer = ByteBuffer.allocate(BUFFER_SIZE);

    /** 写缓冲 */
    private ByteBuffer writeBuffer = ByteBuffer.allocate(BUFFER_SIZE);

    /** 使用的编码 */
    private Charset charset = StandardCharsets.UTF_8;

    /** 可以自定义服务器端口 */
    private int port;

    public ChatServer() {
        this(DEFAULT_PORT);
    }
    public ChatServer(int port){
        this.port = port;
    }

    /**
     * 查看用户是否准备退出
     * @param message 用户发送的消息
     * @return true 表示准备退出，反之
     */
    private boolean checkQuit (String message) {
        return QUIT.equals(message);
    }

    /** 释放资源 */
    private void close(Closeable closeable) {
        if(closeable != null){
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    private String getClientName(SocketChannel client) {
        return "客户端[" + client.socket().getPort() + "]";
    }

    private void forwardMessage(SocketChannel client, String message) throws IOException {
        // 遍历所有注册的 channel
        Set<SelectionKey> selectionKeys = selector.keys();
        for(SelectionKey selectionKey : selectionKeys) {
            Channel channel = selectionKey.channel();
            // 是服务端的 channel 则跳过
            if (channel instanceof  ServerSocketChannel) {
                continue;
            }
            // channel 有效且不是发送消息的客户端 channel
            if (selectionKey.isValid() && !client.equals(channel)) {
                // 向 Buffer 写入数据，准备发送
                writeBuffer.clear();
                writeBuffer.put(charset.encode(getClientName(client) + ":" + message));
                writeBuffer.flip();
                // 从 Buffer 读取数据并发送
                while (writeBuffer.hasRemaining()) {
                    ((SocketChannel)channel).write(writeBuffer);
                }
            }
        }
    }

    // 读取消息
    private String receive(SocketChannel client) throws IOException {
        // 清空残留信息，将读模式转为写模式
        readBuffer.clear();
        // 接收消息，相等于向 Buffer 写数据
        while (client.read(readBuffer) > 0);
        // 写完后将写模式转为读模式
        readBuffer.flip();
        return String.valueOf(charset.decode(readBuffer));
    }

    /** 处理事件 */
    private void handles(SelectionKey selectionKey) throws IOException{
        // 如果触发了 ACCEPT 事件 —— 和客户端建立了连接
        if (selectionKey.isAcceptable()) {
            ServerSocketChannel server = (ServerSocketChannel) selectionKey.channel();
            // 获取客户端的 channel
            SocketChannel client = server.accept();
            // 设为非阻塞调用模式
            client.configureBlocking(false);
            // 注册客户端的 READ 事件到 selector
            client.register(selector, SelectionKey.OP_READ);
            System.out.println(getClientName(client)  + "已连接服务器");
        }else if (selectionKey.isReadable()){
            // 如果触发了 READ 事件 —— 客户端发送了数据
            SocketChannel client = (SocketChannel) selectionKey.channel();
            // 读取数据
            String message = receive(client);
            if (message.isEmpty()) {
                // 消息为空，客户端异常，取消监听
                selectionKey.cancel();
                // 刷新
                selector.wakeup();
            } else {
                // 转发消息
                forwardMessage(client, message);
                // 查看用户是否准备退出
                if(checkQuit(message)) {
                    selectionKey.cancel();
                    selector.wakeup();
                    System.out.println(getClientName(client) + "已断开连接");
                }
            }
        }
    }

    /** 启动服务器端开始监听 */
    private void start() {
        try {
            server = ServerSocketChannel.open();
            // 设置非阻塞调用
            server.configureBlocking(false);
            // 绑定监听端口
            ServerSocket serverSocket = server.socket();
            serverSocket.bind(new InetSocketAddress(port));
            // 创建 selector
            selector = Selector.open();
            // 注册服务器 Channel 的 ACCEPT（接收客户端请求） 事件到 selector
            server.register(selector, SelectionKey.OP_ACCEPT);
            System.out.println("启动服务器，监听端口：" + port + "...");
            // 不断监听请求
            while (true) {
                // select 是阻塞式调用，如果成功调用说明已有 Channel 就绪
                selector.select();
                // select 监听到的所有事件
                Set<SelectionKey> selectionKeys = selector.selectedKeys();
                for (SelectionKey selectionKey : selectionKeys) {
                    // 处理被触发的事件
                    handles(selectionKey);
                }
                // 处理后清空
                selectionKeys.clear();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(selector);
        }
    }

    /**
     * @param args main方法参数
     */
    public static void main(String[] args) {
        ChatServer chatServer = new ChatServer();
        chatServer.start();
    }
}
```

客户端代码：

```java
public class ChatClient {

    /** 服务器默认主机 */
    private static final String DEFAULT_HOST = "127.0.0.1";

    private String host;

    /** 服务器默认端口 */
    private static final int DEFAULT_PORT = 8080;

    private int port;

    /** 结束条件 */
    private static final String QUIT = "quit";

    /** 和服务器通信的 socket */
    private SocketChannel client;

    /** 缓冲区大小 */
    private static final int BUFFER_SIZE = 1024;

    /** 读取缓冲 */
    private ByteBuffer readBuffer = ByteBuffer.allocate(BUFFER_SIZE);

    /** 写出缓冲 */
    private ByteBuffer writeBuffer = ByteBuffer.allocate(BUFFER_SIZE);

    /** Selector 选择器 */
    private Selector selector;

    /** 使用的编码 */
    private Charset charset = StandardCharsets.UTF_8;

    public ChatClient() {
        this(DEFAULT_HOST, DEFAULT_PORT);
    }

    public ChatClient(String host, int port) {
        this.host = host;
        this.port = port;
    }


    /**
     * 向服务器发送消息
     * @param message 要发送的消息
     */
    public void send(String message) throws IOException {
        if (message.isEmpty()) {
            return;
        }
        // 读模式转写模式
        writeBuffer.clear();
        writeBuffer.put(charset.encode(message));
        // 写模式转读模式
        writeBuffer.flip();
        while (writeBuffer.hasRemaining()) {
            client.write(writeBuffer);
        }
        // 判断用户是否准备退出
        if (checkQuit(message)) {
            close(selector);
        }
    }

    /**
     * 查看用户是否准备退出
     * @param message 用户发送的消息
     * @return true 表示准备退出，反之
     */
    public boolean checkQuit (String message) {
        return QUIT.equals(message);
    }

    /** 释放资源 */
    private void close(Closeable closeable) {
        if(closeable != null){
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /** 启动客户端 */
    public void start() {
        try {
            // 创建 channel
            client = SocketChannel.open();
            // 设置非阻塞调用
            client.configureBlocking(false);
            // 创建 selector
            selector = Selector.open();
            // 注册客户端 channel 的 CONNECT 事件到 selector
            client.register(selector, SelectionKey.OP_CONNECT);
            // 客户端向服务器发起连接请求
            client.connect(new InetSocketAddress(host, port));
            while (true) {
                // select 是阻塞式调用，如果成功调用说明已有 Channel 就绪
                selector.select();
                // select 监听到的所有事件
                Set<SelectionKey> selectionKeys = selector.selectedKeys();
                for (SelectionKey selectionKey : selectionKeys) {
                    // 处理被触发的事件
                    handles(selectionKey);
                }
                // 处理后清空
                selectionKeys.clear();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClosedSelectorException e) {
            // 用户正常退出
        } finally {
            close(selector);
        }
    }

    private void handles(SelectionKey selectionKey) throws IOException {
        // 如果触发了 CONNECT 事件 —— 连接就绪事件
        if (selectionKey.isConnectable()) {
            SocketChannel client = (SocketChannel) selectionKey.channel();
            // 如果连接已经建立了
            if (client.isConnectionPending()) {
                client.finishConnect();
                // 处理用户输入
                new Thread(new UserInputHandler(this)).start();
            }
            // 注册客户端的 READ 事件到 selector
            client.register(selector, SelectionKey.OP_READ);
        }else if (selectionKey.isReadable()) {
            // 如果触发了 READ 事件 —— 服务器转发了数据
            SocketChannel client = (SocketChannel) selectionKey.channel();
            // 读取数据
            String message = receive(client);
            if (message.isEmpty()) {
                // 消息为空，服务器异常，取消监听
                close(selector);
            } else {
                System.out.println(message);
            }
        }
    }

    private String receive(SocketChannel client) throws IOException {
        readBuffer.clear();
        while (client.read(readBuffer) > 0);
        readBuffer.flip();
        return String.valueOf(charset.decode(readBuffer));
    }

    public static void main(String[] args) {
        ChatClient chatClient = new ChatClient();
        // 启动客户端
        chatClient.start();
    }
}
```

处理用户输入的类 UserInputHandler 代码不变。

---

## AIO 异步非阻塞

服务器端通道AsynchronousServerSocketChannel，通过 accept 获取客户端连接

客户端通道 AsynchronousSocketChannel，通过 connect 连接服务器

实现方式：

- 通过 Future 的 get 阻塞式调用
- 通过实现 CompletionHandler 接口，重写回调方法 completed 和 failed

---

### 回音聊天室

实现功能：可以将用户发送给服务器的消息再返回给用户，类似“回音”效果。

服务器代码：

```Java
public class Server {

    final String LOCALHOST = "127.0.0.1";
    final int DEFAULT_PORT = 8080;

    /** 异步的服务器端 channel */
    AsynchronousServerSocketChannel serverChannel;

    public static void main(String[] args) {
        new Server().start();
    }

    public void start() {
        try {
            // 绑定监听的端口
            serverChannel = AsynchronousServerSocketChannel.open();
            serverChannel.bind(new InetSocketAddress(LOCALHOST, DEFAULT_PORT));
            System.out.println("启动服务器，监听端口：" + DEFAULT_PORT);
            while (true) {
                // 异步调用，接收客户端请求
                serverChannel.accept(null, new AcceptHandler());
                // 控制调用频率
                System.in.read();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(serverChannel);
        }
    }

    /**
     * 释放资源
     * @param closeable 要释放的资源
     */
    private void close(Closeable closeable) {
        if (closeable != null) {
            try {
                closeable.close();
                System.out.println("关闭" + closeable);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 处理异步 ACCEPT 事件的方法，第一个泛型参数是成功回调的 result 类型，第二个泛型参数是 attachment 类型
     */
    private class AcceptHandler implements CompletionHandler<AsynchronousSocketChannel, Object> {

        /**
         * 成功的回调方法
         * @param result 和服务器建立连接的客户端 channel
         * @param attachment 额外数据
         */
        @Override
        public void completed(AsynchronousSocketChannel result, Object attachment) {
            // 让服务器继续监听其他客户端
            if (serverChannel.isOpen()) {
                serverChannel.accept(null, this);
            }
            AsynchronousSocketChannel clientChannel = result;
            if (clientChannel != null && clientChannel.isOpen()) {
                // 处理异步 read
                ClientHandler clientHandler = new ClientHandler(clientChannel);
                // 读取客户端数据
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                // 附加信息，一个 map 集合
                Map<String, Object> info = new HashMap<>();
                info.put("type", "read");
                info.put("buffer", buffer);
                // 异步调用 read，相当于向 buffer 写数据，buffer 是写模式
                clientChannel.read(buffer, info, clientHandler);
            }
        }

        /**
         * 失败的回调方法
         * @param exc 异常
         * @param attachment 额外数据
         */
        @Override
        public void failed(Throwable exc, Object attachment) {
            // 处理错误
        }
    }

    /**
     * 处理读写事件，第一个参数表示 Buffer 写入的字节
     */
    private class ClientHandler implements CompletionHandler<Integer, Object> {

        private AsynchronousSocketChannel clientChannel;

        ClientHandler(AsynchronousSocketChannel clientChannel) {
            this.clientChannel = clientChannel;
        }

        @Override
        public void completed(Integer result, Object attachment) {
            Map<String, Object> info = (Map<String, Object>) attachment;
            // 判断操作类型是 read 还是 write
            String type = (String) info.get("type");
            // 读完客户端数据后写出
            if ("read".equals(type)) {
                ByteBuffer buffer = (ByteBuffer) info.get("buffer");
                // 将 buffer 从写模式转为读模式
                buffer.flip();
                info.put("type", "write");
                // 写出数据，相当于从 buffer 读
                clientChannel.write(buffer, info, this);
                // 将 buffer 从读模式转为写模式
                buffer.clear();
            } else if ("write".equals(type)) {
                // 写出后继续读取客户端数据
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                info.put("type", "read");
                info.put("buffer", buffer);
                // 异步调用 read
                clientChannel.read(buffer, info, this);
            }
        }

        @Override
        public void failed(Throwable exc, Object attachment) {
            //处理错误
        }
    }
}
```

客户端代码：

```java
public class Client {

    final String LOCALHOST = "127.0.0.1";
    final int DEFAULT_PORT = 8080;

    /** 异步的客户端 channel */
    AsynchronousSocketChannel clientChannel;

    public static void main(String[] args) {
        new Client().start();
    }

    public void start() {
        try {
            // 创建 channel
            clientChannel = AsynchronousSocketChannel.open();
            Future<Void> future = clientChannel.connect(new InetSocketAddress(LOCALHOST, DEFAULT_PORT));
            // get 是阻塞式调用，在客户端和服务器建立连接前会阻塞
            future.get();
            // 等待用户输入
            BufferedReader consoleReader = new BufferedReader(new InputStreamReader(System.in));
            while (true) {
                String input = consoleReader.readLine();
                // 将用户消息发送给服务器
                byte[] inputBytes = input.getBytes();
                ByteBuffer buffer = ByteBuffer.wrap(inputBytes);
                Future<Integer> writeResult = clientChannel.write(buffer);
                // 阻塞直到消息成功发送
                writeResult.get();
                // 从服务器读取响应消息
                // 写出数据相当于从 Buffer 读，所以现在将读转为写模式
                buffer.clear();
                Future<Integer> readResult = clientChannel.read(buffer);
                // 阻塞直到成功读取消息
                readResult.get();
                String echo = new String(buffer.array());
                // 读取数据相当于向 Buffer 写，所以现在将写转为读模式
                buffer.flip();
                System.out.println(echo);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            close(clientChannel);
        }
    }

    /**
     * 释放资源
     * @param closeable 要释放的资源
     */
    private void close(Closeable closeable) {
        if (closeable != null) {
            try {
                closeable.close();
                System.out.println("关闭" + closeable);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

---

### 使用 AIO 改写多人聊天室

服务器端：

```java
public class ChatServer {

    /** 服务器主机 */
    private static final String LOCALHOST = "localhost";

    /** 服务器端口 */
    private static final int DEFAULT_PORT = 8080;

    /** 结束条件 */
    private static final String QUIT = "quit";

    /** 缓冲区大小 */
    private static final int BUFFER_SIZE = 1024;

    /** 线程池大小 */
    private static final int THREAD_POOL_SIZE = 8;

    /** 自定义 ChannelGroup */
    private AsynchronousChannelGroup channelGroup;

    /** 服务端 Channel */
    private AsynchronousServerSocketChannel serverChannel;

    /** 使用的编码 */
    private Charset charset = StandardCharsets.UTF_8;

    /** 可以自定义服务器端口 */
    private int port;

    /** 存储用户在线列表 */
    private List<ClientHandler> connectedClients;

    public ChatServer() {
        this(DEFAULT_PORT);
    }

    public ChatServer(int port){
        this.port = port;
        this.connectedClients = new ArrayList<>();
    }

    /**
     * 程序入口
     * @param args 参数
     */
    public static void main(String[] args) {
        new ChatServer().start();
    }

    /**
     * 启动服务器
     */
    private void start() {
        try {
            // 创建自定义 channel group
            ExecutorService executorService = Executors.newFixedThreadPool(THREAD_POOL_SIZE);
            channelGroup = AsynchronousChannelGroup.withThreadPool(executorService);
            // 创建 channel 并绑定 group 和端口
            serverChannel = AsynchronousServerSocketChannel.open(channelGroup);
            serverChannel.bind(new InetSocketAddress(LOCALHOST, port));
            System.out.println("启动服务器，监听端口：" + port);
            while (true) {
                // 接收客户端请求并处理
                serverChannel.accept(null, new AcceptHandler());
                System.in.read();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(serverChannel);
        }
    }

    /**
     * 查看用户是否准备退出
     * @param message 用户发送的消息
     * @return true 表示准备退出，反之
     */
    private boolean checkQuit (String message) {
        return QUIT.equals(message);
    }

    /**
     * 获取客户端名
     * @param clientChannel 客户端 channel
     * @return 返回客户端名
     */
    private String getClientName(AsynchronousSocketChannel clientChannel) {
        int clientPort = -1;
        try {
            InetSocketAddress inetSocketAddress = (InetSocketAddress) clientChannel.getRemoteAddress();
            clientPort = inetSocketAddress.getPort();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "客户端[" + clientPort + "]";
    }

    /**
     * 服务器转发消息给其他客户端
     * @param clientChannel 发送消息的客户端
     * @param message 发送的具体消息
     */
    private synchronized void forwardMessage(AsynchronousSocketChannel clientChannel, String message) {
        for (ClientHandler handler : connectedClients){
            // 该信息不用再转发到发送信息的客户端
            if (!handler.clientChannel.equals(clientChannel)){
                try {
                    // 将要转发的信息写入到 buffer 中
                    ByteBuffer buffer = charset.encode(getClientName(handler.clientChannel) + ":" + message);
                    // 写事件不添加附加信息
                    handler.clientChannel.write(buffer,null, handler);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 服务器接收客户端消息
     * @param buffer buffer 缓冲区
     * @return 返回接收的消息
     */
    private synchronized String receive(ByteBuffer buffer) {
        CharBuffer charBuffer = charset.decode(buffer);
        return String.valueOf(charBuffer);
    }

    /**
     * 添加一个新的客户端进到在线列表
     * @param handler 客户端对应的 handler
     */
    private synchronized void addClient(ClientHandler handler) {
        connectedClients.add(handler);
        System.out.println(getClientName(handler.clientChannel) + "已经连接到服务器");
    }

    /**
     * 将下线用户从在线列表中删除
     * @param clientHandler 客户端对应的 handler
     */
    private synchronized void removeClient(ClientHandler clientHandler) {
        connectedClients.remove(clientHandler);
        System.out.println(getClientName(clientHandler.clientChannel) + "已断开连接");
        // 关闭该客户对应流
        close(clientHandler.clientChannel);
    }

    /**
     * 释放资源
     */
    private void close(Closeable closeable) {
        if(closeable != null){
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 处理服务器的 ACCEPT 事件，第一个泛型参数是异步调用方法应该返回的类型，ACCEPT 应该返回一个客户端的 channel
     */
    private class AcceptHandler implements CompletionHandler<AsynchronousSocketChannel, Object> {


        @Override
        public void completed(AsynchronousSocketChannel clientChannel, Object attachment) {
            // 让服务器继续监听其他客户端
            if (serverChannel.isOpen()) {
                serverChannel.accept(null, this);
            }
            // 客户端处于有效状态
            if (clientChannel != null && clientChannel.isOpen()) {
                // 处理异步 read
                ClientHandler clientHandler = new ClientHandler(clientChannel);
                // 读取客户端数据
                ByteBuffer buffer = ByteBuffer.allocate(BUFFER_SIZE);
                // 将新用户添加到在线用户列表
                addClient(clientHandler);
                // 异步调用 read，向 buffer 写数据，因此此时 buffer 是写模式。
                // 第二个参数表示将 buffer 作为附加信息，第三个参数表示处理异步读的 handler
                clientChannel.read(buffer, buffer, clientHandler);
            }
        }

        @Override
        public void failed(Throwable exc, Object attachment) {
            System.out.println("连接失败：" + exc);
        }

    }

    /**
     * 处理客户端的读写事件
     */
    private class ClientHandler implements CompletionHandler<Integer, Object> {


        private AsynchronousSocketChannel clientChannel;

        public ClientHandler(AsynchronousSocketChannel clientChannel) {
            this.clientChannel = clientChannel;
        }

        @Override
        public void completed(Integer result, Object attachment) {
            ByteBuffer buffer = (ByteBuffer) attachment;
            // buffer 附加信息不为空，说明处理的是异步读
            if (buffer != null) {
                if (result <= 0) {
                    // 客户端异常
                    removeClient(this);
                } else {
                    // 将 buffer 从写模式转为读模式，准备写出数据
                    buffer.flip();
                    String message = receive(buffer);
                    System.out.println(getClientName(clientChannel) + ":" + message);
                    forwardMessage(clientChannel, message);
                    // 将 buffer 转回写模式
                    buffer.clear();
                    // 判断用户是否准备下线
                    if (checkQuit(message)){
                        // 将用户从在线客户列表中去除
                        removeClient(this);
                    }else {
                        // 如果不是则继续等待读取用户输入的信息
                        clientChannel.read(buffer, buffer,this);
                    }
                }
            }
        }

        @Override
        public void failed(Throwable exc, Object attachment) {
            System.out.println("读写失败:"+exc);
        }
    }
}
```

客户端：

```java
public class ChatClient {

    private static final String LOCALHOST = "localhost";
    private static final int DEFAULT_PORT = 8080;
    private static final String QUIT = "quit";
    private static final int BUFFER_SIZE = 1024;

    private AsynchronousSocketChannel clientChannel;
    private Charset charset = StandardCharsets.UTF_8;
    private String host;
    private int port;

    public ChatClient(){
        this(LOCALHOST , DEFAULT_PORT);
    }

    public ChatClient(String host, int port) {
        this.host = host;
        this.port = port;
    }

    /**
     * 程序入口
     * @param args 参数
     */
    public static void main(String[] args) {
        ChatClient client = new ChatClient();
        client.start();
    }

    /**
     * 判断用户是否准备下线
     * @param msg 用户输入的信息
     * @return 用户是否准备下线
     */
    public boolean checkQuit(String msg){
        boolean flag = QUIT.equalsIgnoreCase(msg);
        if(flag){
            close(clientChannel);
        }
        return flag;
    }

    /**
     * 客户端发送数据给服务器
     * @param message 要发送的数据
     */
    public void send(String message) {
        if (message.isEmpty()) {
            return;
        }
        // 将要发送的消息编码并写入缓冲区
        ByteBuffer writeBuffer = charset.encode(message);
        // 发送消息
        Future<Integer> writeResult = clientChannel.write(writeBuffer);
        try {
            // 阻塞直到发送结束
            writeResult.get();
        } catch (InterruptedException | ExecutionException e) {
            System.out.println("发送消息失败");
            e.printStackTrace();
        }
    }

    /**
     * 释放资源
     * @param closeable 要释放的资源
     */
    private void close(Closeable closeable){
        if(closeable != null){
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }


    /**
     * 启动客户端
     */
    private void start(){
        try {
            // 打开管道
            clientChannel = AsynchronousSocketChannel.open();
            // 异步调用connect连接服务器
            Future<Void> future = clientChannel.connect(new InetSocketAddress(host, port));
            // 阻塞直到客户端和服务器建立连接
            future.get();
            new Thread(new UserInputHandler(this)).start();
            // 读取服务器转发的消息
            ByteBuffer readBuffer = ByteBuffer.allocate(BUFFER_SIZE);
            while (true) {
                Future<Integer> readResult = clientChannel.read(readBuffer);
                // 阻塞直到读取完毕
                int result = readResult.get();
                // 假如无法读到数据
                if (result <= 0) {
                    System.out.println("服务器断开连接");
                    close(clientChannel);
                    System.exit(1);
                } else {
                    // 成功读取到了数据，打印在控制台
                    // 先把缓冲区转为读模式
                    readBuffer.flip();
                    String message = String.valueOf(charset.decode(readBuffer));
                    System.out.println(message);
                    // 读完将缓冲区转为写模式
                    readBuffer.clear();
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            close(clientChannel);
        }
    }
}
```

处理用户输入的类 UserInputHandler 代码不变。

---

## 简易 Web 服务器的实现

在客户端向服务器请求资源时，请求的资源可以分为：

- 静态资源：不因请求的次数或顺序变化，例如 HTML、CSS、GIF、PNG 等。
- 动态资源：随着请求发起方/发起时间/请求内容等因素变化，例如商品库存量等。客户端发起请求，服务器将请求交给容器，容器再交给 Servlet 处理，处理完后再返回给容器、服务器及客户端。

Tomcat 的结构：

- Server：Tomocat 服务器最顶层的组件，负责运行 Tomcat 服务器、加载服务器资源和环境变量。

- Service：集合了 Connector 和 Engine 的抽象组件，一个 Service 可以包含多个 Service，多个 Connector 和 一个 Engine。
- Connector：提供基于不同特定协议的实现，接收解析请求并返回响应。具体包括负责提供给客户端可以和服务器创建连接的端点、接收连接请求和建立连接后的资源请求、将服务器的响应返回给客户等。Connector 本身不会处理请求，会将请求交给 Processor 派遣至 Engine 进行处理。
- 容器是 Tomcat 用来处理请求的组件，内部的组件按照层级排列，其中 Engine 是容器的顶级组件。Engine 通过解析请求内容将请求交发送给对应的 Host，Host 代表一个虚拟主机，一个 Engine 可以支持对多个虚拟主机的请求。Host 将请求交给 Context 组件，Context 代表一个 Web Application，是 Tomcat 最复杂的组件之一，负责应用资源管理、应用类加载、Servlet 管理、安全管理等。Context 下一层是 Wrapper 组件，Wrapper 是容器最底层的组件，包裹 Servlet 实例，负责管理 Servlet 的生命周期。

---

### 处理静态资源请求

#### HTTPStatus 状态码枚举类

```java
public enum HTTPStatus {

    OK(200, "OK"),
    NOT_FOUND(404, "File Not Found");

    private int statusCode;
    private String reason;

    HTTPStatus(int statusCode, String reason) {
        this.statusCode = statusCode;
        this.reason = reason;
    }

    public int getStatusCode() {
        return statusCode;
    }

    public String getReason() {
        return  reason;
    }
}
```

---

#### ConnectorUtils 工具类

```java
public class ConnectorUtils {

    /** 存放静态资源的根路径 根据自己的情况设置 */
    public static final String WEB_ROOT = "C:\\Users\\Administrator\\IdeaProjects\\java_study\\test" + File.separator + "webroot";

    public static final String PROTOCOL = "HTTP/1.1";

    public static final String CARRIAGE = "\r";

    public static final String NEWLINE = "\n";

    public static final String SPACE = " ";

    public static String renderStatus(HTTPStatus status) {
        // 协议 + 状态 + 状态码
        return PROTOCOL + SPACE + status.getStatusCode() + SPACE + status.getReason() + CARRIAGE + NEWLINE + CARRIAGE + NEWLINE;
    }
}
```

---

#### Request 处理请求

```java
public class Request {

    /**
     * 默认缓冲区大小
     */
    private static final int BUFFER_SIZE = 1024;

    /**
     * 客户端和服务器的 socket 的输入流，通过该流获取请求内容
     */
    private InputStream inputStream;

    /**
     * 请求的资源标识符
     */
    private String URI;

    public Request(InputStream inputStream) {
        this.inputStream = inputStream;
    }

    /**
     * 获取请求 URI
     * @return 返回 URI 字符串
     */
    public String getURI() {
        return URI;
    }

    /**
     * 解析客户端请求
     */
    public void parse() {
        int len = 0;
        byte[] buffer = new byte[BUFFER_SIZE];
        try {
            len = inputStream.read(buffer);
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 将读取到的字节数组转为字符串
        String requestURL = new String(buffer, 0, len);
        URI = parseURI(requestURL);
    }

    /**
     * 解析静态请求的 URI
     * @param requestStr 请求文本
     * @return 返回 URI
     * 例如 GET /index.html HTTP1.1，先找到第一个空格的位置，再找到第二个空格的位置，截取两个位置中间作为结果返回。
     */
    private String parseURI(String requestStr) {
        int index1, index2;
        index1 = requestStr.indexOf(' ');
        if(index1 != -1) {
            index2 = requestStr.indexOf(' ', index1 + 1);
            if (index2 != -1) {
                return requestStr.substring(index1 + 1, index2);
            }
        }
        // 无法解析出 URI
        return "";
    }

}
```

---

#### Response 处理响应

```java
public class Response {

    /**
     * 默认缓冲区大小
     */
    private static final int BUFFER_SIZE = 1024;

    /**
     * 客户端请求
     */
    private Request request;

    /**
     * 客户端和服务器的 socket 的输出流，通过该流响应客户端
     */
    private OutputStream outputStream;

    public Response(OutputStream outputStream) {
        this.outputStream =outputStream;
    }

    /**
     * 设置客户端请求
     * @param request 客户端请求
     */
    public void setRequest(Request request) {
        this.request = request;
    }

    /**
     * 响应静态资源的请求
     */
    public void sendStaticResource() throws IOException {
        File file = new File(ConnectorUtils.WEB_ROOT, request.getURI());
        try {
            // 成功处理
            write(file, HTTPStatus.OK);
        } catch (IOException e) {
            // 失败处理
            write(new File(ConnectorUtils.WEB_ROOT, "404.html"), HTTPStatus.NOT_FOUND);
        }
    }

    private void write(File resource, HTTPStatus status) throws IOException {
        try (FileInputStream inputStream = new FileInputStream(resource)){
            // 响应状态信息
            outputStream.write(ConnectorUtils.renderStatus(status).getBytes());
            // 响应资源
            byte[] buffer = new byte[BUFFER_SIZE];
            int length;
            while ((length = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, length);
            }
        }
    }

}
```

---

#### StaticProcessor 处理静态资源

```java
/**
 * 处理静态资源的 processor
 */
public class StaticProcessor {

    public void process(Request request, Response response) {
        try {
            response.sendStaticResource();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

#### Connector 处理服务器连接、请求、响应

```java
public class Connector implements Runnable{

    private static final int DEFAULT_PORT = 8080;

    private ServerSocket serverSocket;

    private int port;

    public Connector() {
        this(DEFAULT_PORT);
    }

    public Connector(int port) {
        this.port = port;
    }

    public void start() {
        Thread thread = new Thread(this);
        thread.start();
    }

    @Override
    public void run() {
        try {
            // 绑定端口
            serverSocket = new ServerSocket(port);
            System.out.println("服务器已启动，监听端口：" + port);
            while (true) {
                // 等待客户端连接
                Socket socket = serverSocket.accept();
                // 客户端输入流
                InputStream inputStream = socket.getInputStream();
                // 客户端输出流
                OutputStream outputStream = socket.getOutputStream();
                // 创建 Request 实例并解析 URI
                Request request = new Request(inputStream);
                request.parse();
                // 创建 Response 实例并设置 request
                Response response = new Response(outputStream);
                response.setRequest(request);
                // 处理静态资源
                StaticProcessor staticProcessor = new StaticProcessor();
                staticProcessor.process(request, response);
                // 处理完后断开连接
                close(socket);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(serverSocket);
        }
    }

    /**
     * 释放资源
     * @param closeable 要关闭的资源
     */
    private void close(Closeable closeable) {
        if (closeable != null) {
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

---

#### BootStrap 服务器启动类

```java
public final class BootStrap {

    public static void main(String[] args) {
        // 启动服务器
        new Connector().start();
    }
}
```

---

#### ClientTest 客户端测试类

也可以直接在浏览器输入 `localhost:8080/index.html` 进行测试。

```java
public class ClientTest {

    public static void main(String[] args) throws IOException {
        // 建立客户端 socket，连接服务器
        Socket socket = new Socket("localhost", 8080);
        // 获取客户端输出流，发送请求
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("GET /index.html HTTP/1.1".getBytes());
        socket.shutdownOutput();
        // 获取客户端输入流，接收响应
        InputStream inputStream = socket.getInputStream();
        byte[] buffer = new byte[2048];
        int length;
        while ((length = inputStream.read(buffer)) != -1) {
            String receive = new String(buffer, 0 ,length);
            System.out.print(receive);
        }
        socket.shutdownInput();
        // 关闭连接
        socket.close();
    }
}
```

---

### 处理动态资源请求

#### 修改 Request 和 Response

让 Request 和 Response 分别实现 ServletRequest 和 ServletResponse 接口，并使用默认的重写方法，只需要修改 Response 中的 getWriter 方法：

```java
@Override
public PrintWriter getWriter() throws IOException {
    return new PrintWriter(outputStream);
}
```

---

#### ServletProcessor 处理动态资源

```java
/**
 * 处理动态资源的 processor
 */
public class ServletProcessor {

    public void process(Request request, Response response) {
        URLClassLoader servletLoader = null;
        try {
            servletLoader = getServletLoader();
            Servlet servlet = getServlet(servletLoader, request);
            servlet.service(request, response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    URLClassLoader getServletLoader() throws MalformedURLException {
        File webroot = new File(ConnectorUtils.WEB_ROOT);
        URL webrootURL = webroot.toURI().toURL();
        return URLClassLoader.newInstance(new URL[]{webrootURL});
    }

    Servlet getServlet(URLClassLoader loader, Request request) throws Exception {
        // 请求 servlet 时，例如请求 XxxServlet 实际请求 servlet/XxxServlet
        String URI = request.getURI();
        String servletName = URI.substring(URI.lastIndexOf("/") + 1);
        // 通过反射创建 servlet 的实例
        Class servletClass = loader.loadClass(servletName);
        Servlet servlet = (Servlet) servletClass.getConstructor().newInstance();
        return servlet;
    }
}
```

---

#### TimeServlet 动态资源

添加在 webroot下：

```java
/**
 * 动态资源 servlet
 */
public class TimeServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        // 得到响应的输出流
        PrintWriter writer = servletResponse.getWriter();
        // 写出响应头
        writer.println(ConnectorUtils.renderStatus(HTTPStatus.OK));
        // 写出当前的时间
        writer.println("当前时间：");
        writer.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
        // 不添加可能空数据
        writer.flush();
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

---

#### 修改 Connector 的 run 方法

```java
// 根据 URI 的开头判断资源类型
if (request.getURI().startsWith("/servlet")) {
    // 处理动态资源
    ServletProcessor servletProcessor = new ServletProcessor();
    servletProcessor.process(request, response);
} else {
    // 处理静态资源
    StaticProcessor staticProcessor = new StaticProcessor();
    staticProcessor.process(request, response);
}
```

---

#### ClientTest 客户端测试类

也可以直接在浏览器输入 `localhost:8080/servlet/TimeServlet` 进行测试。

```java
public class ClientTest {

    public static void main(String[] args) throws IOException {
        ...
        outputStream.write("GET /servlet/TimeServlet HTTP/1.1".getBytes());
		...
    }
}
```

---

### 使用 NIO 改写 Connector

```java
public class Connector implements Runnable{

    private static final int DEFAULT_PORT = 8080;

    private ServerSocketChannel server;

    private Selector selector;

    private int port;

    public Connector() {
        this(DEFAULT_PORT);
    }

    public Connector(int port) {
        this.port = port;
    }

    public void start() {
        Thread thread = new Thread(this);
        thread.start();
    }

    @Override
    public void run() {
        try {
            // 打开 serverSocket的 Channel
            server = ServerSocketChannel.open();
            // 设置非阻塞调用
            server.configureBlocking(false);
            // 绑定监听端口
            ServerSocket serverSocket = server.socket();
            serverSocket.bind(new InetSocketAddress(port));
            // 创建 selector
            selector = Selector.open();
            // 注册服务器 Channel 的 ACCEPT（接收客户端请求） 事件到 selector
            server.register(selector, SelectionKey.OP_ACCEPT);
            System.out.println("启动服务器，监听端口：" + port + "...");
            // 不断监听请求
            while (true) {
                // select 是阻塞式调用，如果成功调用说明已有 Channel 就绪
                selector.select();
                // select 监听到的所有事件
                Set<SelectionKey> selectionKeys = selector.selectedKeys();
                for (SelectionKey selectionKey : selectionKeys) {
                    // 处理被触发的事件
                    handles(selectionKey);
                }
                // 处理后清空
                selectionKeys.clear();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(selector);
        }
    }

    /** 处理事件 */
    private void handles(SelectionKey selectionKey) throws IOException{
        // 如果触发了 ACCEPT 事件 —— 和客户端建立了连接
        if (selectionKey.isAcceptable()) {
            // 获取客户端的 channel
            SocketChannel client = ((ServerSocketChannel)(selectionKey.channel())).accept();
            // 设为非阻塞调用模式
            client.configureBlocking(false);
            // 注册客户端的 READ 事件到 selector
            client.register(selector, SelectionKey.OP_READ);
        } else {
            // 触发的是 READ 事件
            // 获取客户端的 channel
            SocketChannel client = (SocketChannel) selectionKey.channel();
            // 避免设为 BIO 模式报错，取消注册
            selectionKey.cancel();
            // 由于要使用 BIO，重设为阻塞模式
            client.configureBlocking(true);
            Socket socket = client.socket();
            // 客户端输入流
            InputStream inputStream = socket.getInputStream();
            // 客户端输出流
            OutputStream outputStream = socket.getOutputStream();
            // 创建 Request 实例并解析 URI
            Request request = new Request(inputStream);
            request.parse();
            // 创建 Response 实例并设置 request
            Response response = new Response(outputStream);
            response.setRequest(request);
            // 根据 URI 的开头判断资源类型
            if (request.getURI().startsWith("/servlet")) {
                ServletProcessor servletProcessor = new ServletProcessor();
                servletProcessor.process(request, response);
            } else {
                StaticProcessor staticProcessor = new StaticProcessor();
                staticProcessor.process(request, response);
            }
            // 处理完后断开连接
            close(socket);
        }

    }

    /**
     * 释放资源
     * @param closeable 要关闭的资源
     */
    private void close(Closeable closeable) {
        if (closeable != null) {
            try {
                closeable.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

---

## 总结

### BIO

- ServerSocket 服务器端通信套接字
  - bind()： 绑定服务器信息。
  - accept()： 获取客户端连接请求，阻塞调用。
  - 通过 InputStream / OutputStream 进行读写操作，阻塞调用。
  - close()：释放资源。
- Socket 客户端通信套接字
  - connect()：发送连接请求，和服务器建立连接。
  - 通过 InputStream / OutputStream 进行读写操作，阻塞调用。
  - close()：释放资源。
- 特点
  - 同步阻塞 IO，每个连接分配一个线程。
  - 适用场景：连接数目少、服务器资源多、开发难度低。

### NIO

- Channel 通道
  - 可双向操作，既可读也可写。
  - 两种模式，既支持阻塞也支持非阻塞。
  - ServerSocketChannel  服务器端通道，通过静态方法 `open()` 获取实例。
  - SocketChannel 客户端通道，通过静态方法 `open()` 获取实例。
- Buffer 缓冲区
  - flip() 写模式转为读模式，可以准备从 buffer 获取数据。
  - clear() / compact() 读模式转为写模式，可以准备向 buffer 写入数据。
- Selector 多路复用器
  - 轮询监控多条 Channel。
  - select()：查询每个 Channel 是否有事件到达。
- 特点
  - 同步非阻塞 IO，只需要一个线程管理多个连接。
  - 适应场景：连接数目多、连接时间短、开发难度高。

### AIO

- AsynchronousServerSocketChannel 异步服务器端通道，通过静态方法 `open()` 获取实例。
- AsynchronousSocketChannel 异步客户端通道，通过静态方法 `open()` 获取实例。
- AsynchronousChannelGroup 异步通道分组管理器，它可以实现资源共享。创建时需要传入一个ExecutorService，也就是绑定一个线程池，该线程池负责两个任务：处理 IO 事件和触发 CompletionHandler 回调接口。
- 两种异步调用机制：Future 和 CompletionHandler。
- 适用场景：连接数目多、连接时间长、开发难度高。

