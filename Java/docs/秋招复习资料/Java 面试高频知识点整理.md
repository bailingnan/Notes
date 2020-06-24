Java 面试高频知识点整理

---

# Java 面试的 96 个高频知识点 

## 排序算法 9

### P1：排序算法的分类

排序算法可以分为内部排序和外部排序，在内存中进行的排序称为内部排序，当要排序的数据量很大时无法全部拷贝到内存，需要使用外存进行排序，这种排序称为外部排序。

内部排序包括比较排序和非比较排序，比较排序包括插入排序、选择排序、交换排序和归并排序，非比较排序包括计数排序、基数排序和桶排序。其中插入排序又包括直接插入排序和希尔排序，选择排序包括直接选择排序和堆排序，交换排序包括冒泡排序和快速排序。

---

### P2：直接插入排序

直接插入排序属于插入排序，是一种稳定的排序，平均时间复杂度和最差时间复杂度均为 O(n²)，当元素基本有序时的最好时间复杂度为O(n)，空间复杂度为 O(1)。

基本原理是每一趟将一个待排序的记录，按其关键字的大小插入到已经排好序的一组记录的适当位置上，直到所有待排序记录全部插入为止。适用于待排序记录较少或基本有序的情况。

```java
public void insertionSort(int[] nums) {
    for (int i = 1; i < nums.length; i++) {
        int insertNum = nums[i];
        int insertIndex;
        for (insertIndex = i - 1; insertIndex >= 0 && nums[insertIndex] > insertNum; insertIndex--) {
            nums[insertIndex + 1] = nums[insertIndex];
        }
        nums[insertIndex + 1] = insertNum;
    }
}
```

**优化：**直接插入并没有利用到要插入的序列已有序的特点，插入第 i 个元素时可以通过二分查找找到要插入的位置，再把第 i 个元素前 1位与插入位置之间的所有元素后移，把第 i 个元素放在目标位置上。

```java
public void binaryInsertionSort(int[] nums) {
    for (int index = 1; index < nums.length; index++) {
        int insertNum = nums[index];
        int insertIndex = -1;
        int start = 0;
        int end = index - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (insertNum > nums[mid])
                start = mid + 1;
            else if (insertNum < nums[mid])
                end = mid - 1;
            else {
                insertIndex = mid + 1;
                break;
            }
        }
        if (insertIndex == -1)
            insertIndex = start;
        if (index - insertIndex >= 0)
            System.arraycopy(nums, insertIndex, nums, insertIndex + 1, index - insertIndex);
        nums[insertIndex] = insertNum;
    }
}
```

---

### P3：希尔排序

希尔排序属于插入排序，又称缩小增量排序，是对直接插入排序的一种改进，并且是一种不稳定的排序，平均时间复杂度为O(n^1.3^)，最差时间复杂度为 O(n²)，最好时间复杂度为 O(n)，空间复杂度为 O(1)。

基本原理是把记录按下标的一定增量分组，对每组进行直接插入排序，每次排序后减小增量，当增量减至 1 时，排序完毕。适用于中等规模的数据量，对规模非常大的数据量不是最佳选择。

```java
public void shellSort(int[] nums) {
    for (int d = nums.length / 2; d > 0 ; d /= 2) {
        for (int i = d; i < nums.length; i++) {
            int insertNum = nums[i];
            int insertIndex;
            for (insertIndex = i - d; insertIndex >= 0 && nums[insertIndex] > insertNum; insertIndex -= d) {
                nums[insertIndex + d] = nums[insertIndex];
            }
            nums[insertIndex + d] = insertNum;
        }
    }
}
```

---

### **P4：直接选择排序**

直接选择排序属于选择排序，是一种不稳定的排序，任何情况下时间复杂度都是 O(n²)，空间复杂度为 O(1)。基本原理是每次在未排序序列中找到最小元素，和未排序序列的第一个元素交换位置，再在剩余的未排序序列重复该操作直到所有元素排序完毕。适用于数据量较小的情况，比直接插入排序稍快。

```java
public void selectSort(int[] nums) {
    int minIndex;
    for (int index = 0; index < nums.length - 1; index++){
        minIndex = index;
        for (int i = index + 1;i < nums.length; i++){
            if(nums[i] < nums[minIndex]) 
                minIndex = i;
        }
        if (index != minIndex){
            swap(nums, index, minIndex);
        }
    }
}
```

---

### <font color = 'red'>P5：堆排序</font>

堆排序属于选择排序，是对直接选择排序的改进，并且是一种不稳定的排序，任何情况时间复杂度都为 O(nlogn)，空间复杂度为 O(1)。

基本原理是将待排序记录看作完全二叉树，可以建立大根堆或小根堆，大根堆中每个节点的值都不小于它的子节点值，小根堆中每个节点的值都不大于它的子节点值。适用于数据量较大的情况。

以大根堆为例，在建堆时首先将最后一个节点作为当前节点，如果当前结点存在父节点且值大于父节点，就将当前节点和父节点交换。在移除时首先暂存根节点的值，然后用最后一个节点代替根节点并作为当前节点，如果当前节点存在子节点且值小于子节点，就将其与值较大的子节点进行交换，调整完堆后返回暂存的值。

```java
public void add(int[] nums, int i, int num){
    nums[i] = num;
    int curIndex = i;
    while (curIndex > 0) {
        int parentIndex = (curIndex - 1) / 2;
        if (nums[parentIndex] < nums[curIndex]) 
            swap(nums, parentIndex, curIndex);
        else break;
        curIndex =parentIndex;
    }
}

public int remove(int[] nums, int size){
    int result = nums[0];
    nums[0] = nums[size - 1];
    int curIndex = 0;
    while (true) {
        int leftIndex = curIndex * 2 + 1;
        int rightIndex = curIndex * 2 + 2;
        if (leftIndex >= size) break;
        int maxIndex = leftIndex;
        if (rightIndex < size && nums[maxIndex] < nums[rightIndex])
            maxIndex = rightIndex;
        if (nums[curIndex] < nums[maxIndex])
            swap(nums, curIndex, maxIndex);
        else break;
        curIndex = maxIndex;
    }
    return result;
}
```

---

### P6：冒泡排序

冒泡排序属于交换排序，是一种稳定的排序，平均时间复杂度和最坏时间复杂度均为 O(n²)，当元素基本有序时的最好时间复杂度为O(n)，空间复杂度为 O(1)。

基本原理是比较相邻的元素，如果第一个比第二个大就进行交换，对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对，每一轮排序后末尾元素都是有序的，针对 n 个元素重复以上步骤 n -1 次排序完毕。

```java
public void bubbleSort(int[] nums) {
    for (int i = 0; i < nums.length - 1; i++) {
        for (int index = 0; index < nums.length - 1 - i; index++) {
            if (nums[index] > nums[index + 1]) 
                swap(nums, index, index + 1)
        }
    }
}
```

**优化：**当序列已经有序时仍会进行不必要的比较，可以设置一个标志位记录是否有元素交换，如果没有直接结束比较。

```java
public void betterBubbleSort(int[] nums) {
    boolean swap;
    for (int i = 0; i < nums.length - 1; i++) {
        swap = true;
        for (int index = 0; index < nums.length - 1 - i; index++) {
            if (nums[index] > nums[index + 1]) {
                swap(nums, index ,index + 1);
                swap = false;
            }
        }
        if (swap) break;
    }
}
```

---

### <font color = 'red'>P7：快速排序</font>

快速排序属于交换排序，是对冒泡排序的一种改进，并且是一种不稳定的排序，平均时间复杂度和最好时间复杂度均为 O(nlogn)，当元素基本有序时的最坏时间复杂度为O(n²)，空间复杂度为 O(logn)。

基本原理是首先选择一个基准元素，然后通过一趟排序将要排序的数据分割成独立的两部分，一部分全部小于等于基准元素，一部分全部大于等于基准元素，然后再按此方法递归对这两部分数据分别进行快速排序。适用于数据量较大且元素基本无序的情况。

快速排序的一次划分从两头交替搜索，直到 low 和 high 指针重合，因此时间复杂度是 O(n)，而整个算法的时间复杂度与划分趟数有关。最好情况是每次划分选择的中间数恰好将当前序列几乎等分，经过 logn 趟划分便可得到长度为 1 的子表，这样算法的时间复杂度为O(nlogn)。最坏的情况是每次所选中间数是当前序列中的最大或最小元素，这使每次划分所得的子表其中一个为空表，另一个子表的长度为原表的长度 - 1。这样长度为 n 的数据表的需要经过 n 趟划分，整个排序算法的时间复杂度为O(n²)。

从空间上看尽管快速排序只需要一个元素的辅助空间，但快速排序需要一个栈空间来实现递归。最好的情况下，即快速排序的每一趟排序都将元素序列均匀地分割成长度相近的两个子表，所需栈的最大深度为 log(n+1)，最坏情况下栈的最大深度为 n。

```java
public void quickSort(int[] nums, int start, int end) {
    if (start < end) {
        int pivotIndex = getPivotIndex(nums, start, end);
        quickSort(nums, start, pivotIndex - 1);
        quickSort(nums, pivotIndex + 1, end);
    }
}

public int getPivotIndex(int[] nums, int start, int end) {
    int pivot = nums[start];
    int low = start;
    int high = end;
    while (low < high) {
        while (low <= high && nums[low] <= pivot) 
            low++;
        while (low <= high && nums[high] > pivot) 
            high--;
        if (low < high) 
            swap(nums, low, high);
    }
    swap(nums, start, high);
    return high;
}
```

**优化：**当规模足够小时，例如 `end - start < 10` 时，采用直接插入排序。

----

### P8：归并排序

归并排序是基于归并操作的排序算法，是一种稳定的排序算法，任何情况时间复杂度都为 O(nlogn)，空间复杂度为 O(n)。

基本原理是应用分治法将待排序序列分成两部分，然后对两部分分别递归排序，最后进行合并，使用一个辅助空间并设定两个指针分别指向两个有序序列的起始元素，将指针对应的较小元素添加到辅助空间，重复该步骤到某一序列到达末尾，然后将另一序列剩余元素合并到辅助空间末尾。适用于数据量大且对稳定性有要求的情况。

```java
int[] help;

public void mergeSort(int[] arr) {
    int[] help = new int[arr.length];
    sort(arr, 0, arr.length - 1);
}

public void sort(int[] arr, int start, int end) {
    if (start == end) return;
    int mid = start + (end - start) / 2;
    sort(arr, start, mid);
    sort(arr, mid + 1, end);
    merge(arr, start, mid, end);
}

public void merge(int[] arr, int start, int mid, int end) {
    if (end + 1 - start >= 0) System.arraycopy(arr, start, help, start, end + 1 - start);
    int p = start;
    int q = mid + 1;
    int index = start;
    while (p <= mid && q <= end) {
        if (help[p] < help[q]) 
            arr[index++] = help[p++];
        else 
            arr[index++] = help[q++];
    }
    while (p <= mid) arr[index++] = help[p++];
    while (q <= end) arr[index++] = help[q++];
}
```

---

### P9：排序算法的选择原则

当数据量规模较小时，可以考虑直接插入排序或直接选择排序，当元素分布有序时直接插入排序将大大减少比较次数和移动记录的次数，如果不要求稳定性，可以使用直接选择排序，效率略高于直接插入排序。

当数据量规模中等时，可以选择希尔排序。

当数据量规模较大时，可以考虑堆排序、快速排序和归并排序。如果对稳定性有要求可以采用归并排序，如果元素分布随机可以采用快速排序，如果元素分布接近正序或逆序可以采用堆排序。

一般不使用冒泡排序。

---

## 设计模式 7

### P1：设计模式的原则

**开闭原则：**面向对象设计中最基础的设计原则，指一个软件实体（类、模块、方法等）应该对扩展开放，对修改关闭。它强调用抽象构建框架，用实现扩展细节，提高代码的可复用性和可维护性。例如在版本更新时尽量不修改源代码，但可以增加新功能。

**单一职责原则：**一个类、接口或方法只负责一个职责，可以提高代码可读性和可维护性，降低代码复杂度以及变更引起的风险。

**依赖倒置原则：**程序应该依赖于抽象类或接口，而不是具体的实现类。可以降低代码的耦合度，提高系统的稳定性。

**接口隔离原则：**将不同功能定义在不同接口中实现接口隔离，避免了类依赖它不需要的接口，减少了接口之间依赖的冗余性和复杂性。

**里氏替换原则：**对开闭原则的补充，规定了任何父类可以出现的地方子类都一定可以出现，可以约束继承泛滥，加键程序健壮性。

**迪米特原则：**也叫最少知道原则，每个模块对其他模块都要尽可能少的了解和依赖，可以降低代码耦合度。

**合成/聚合原则：**尽量使用组合(has a)或聚合(contains a)而不是继承关系达到软件复用的目的，可以使系统更加灵活，降低耦合度。

---

### P2：设计模式的分类

**创建型模式：**提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象，这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。包括：工厂模式、抽象工厂模式、单例模式、建造者模式、原型模式。

**结构型模式：**通过类和接口之间的继承和引用实现创建复杂结构对象的功能。包括：适配器模式、桥接模式、过滤器模式、组合模式、装饰器模式、外观模式、享元模式、代理模式。

**行为型模式：**通过类之间不同的通信方式实现不同的行为方式。包括：责任链模式、命名模式、解释器模式、迭代器模式、中介者模式、备忘录模式、观察者模式、状态模式、策略模式、模板模式、访问者模式。

---

### P3：工厂模式

工厂模式属于创建型模式，分为简单工厂模式，工厂方法模式和抽象工厂模式。

简单工厂模式指由一个工厂对象来创建实例，客户端不需要关注创建的逻辑，只需要提供传入工厂对象的参数。

工厂方法模式指定义一个创建对象的接口，让接口的实现类来决定创建哪一种对象，工厂方法模式让类的实例化推迟到子类中进行。工厂方法模式中客户端只需关心对应的工厂而无需关心创建细节，主要解决了产品扩展的问题，在简单工厂模式中如果产品种类变多，工厂的职责会越来越多，不便于维护。

抽象工厂模式指提供一个创建一系列相关或相互依赖对象的接口，无需指定它们的具体类。客户端不依赖于产品类实例如何被创建和实现的细节，主要用于系统的产品有多于一个的产品族，而系统只消费其中某一个产品族产品的情况。

**总结：**

**简单工厂：**一个工厂，一种抽象产品。例如一个麦当劳店，可以生产多种汉堡。

```java
public class MacDonaldFactory {
    public Hamburger eatHamburger(String name) {
        if ("beef".equals(name))
            return new BeefHamburger();
        else if ("pig".equals(name))
            return new PigHamburger();
        return null;
    }
}

interface Hamburger {
    void eat();
}

class BeefHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃牛肉汉堡");
    }
}

class PigHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃猪肉汉堡");
    }
}
```

**工厂方法：**多个工厂，一种抽象产品。例如一个麦当劳店，可以生产多种汉堡，一个肯德基店，也可以生产多种汉堡。

```java
public interface HamburgerFactory {
    Hamburger build();
}

class MCFactory implements HamburgerFactory {
    @Override
    public Hamburger build() {
        return new MCHamburger();
    }
}

class KFCFactory implements HamburgerFactory {
    @Override
    public Hamburger build() {
        return new KFCHamburger();
    }
}

interface Hamburger {
    void eat();
}

class MCHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃麦当劳汉堡");
    }
}

class KFCHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃肯德基汉堡");
    }
}
```

**抽象工厂：**多个工厂，多种抽象产品。例如一个麦当劳店和一个肯德基店都可以生产多种汉堡和可乐。

```java
public interface FoodFactory {
    Hamburger buildHamburger();
    Drink buildDrink();
}

class MCFactory implements FoodFactory {
    @Override
    public Hamburger buildHamburger() {
        return new MCHamburger();
    }

    @Override
    public Drink buildDrink() {
        return new MCDrink();
    }
}

class KFCFactory implements FoodFactory {
    @Override
    public Hamburger buildHamburger() {
        return new KFCHamburger();
    }

    @Override
    public Drink buildDrink() {
        return new KFCDrink();
    }
}

interface Hamburger {
    void eat();
}

class MCHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃麦当劳汉堡");
    }
}

class KFCHamburger implements Hamburger {
    @Override
    public void eat() {
        System.out.println("吃肯德基汉堡");
    }
}

interface Drink {
    void drink();
}

class MCDrink implements Drink {
    @Override
    public void drink() {
        System.out.println("喝麦当劳饮料");
    }
}

class KFCDrink implements Drink {
    @Override
    public void drink() {
        System.out.println("喝肯德基饮料");
    }
}
```

---

### <font color="red">P4：单例模式</font>

单例模式属于创建型模式，是指一个单例类在任何情况下都只存在一个实例，构造器必须是私有的并由自己创建一个静态实例对象，并对外提供一个静态公有的获取实例方法。优点是在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例的情况下，并且可以避免对资源的多重占用。缺点是没有抽象层，难以扩展，与单一职责原则冲突。

**饿汉式：**在类加载时就初始化创建单例对象，是线程安全的，但不管是否使用都会创建对象，可能会浪费内存。

```java
public class HungrySingleton {
    private HungrySingleton(){}
    
    private static HungrySingleton instance = new HungrySingleton();
    
    public static HungrySingleton getInstance() {
        return instance;
    }
}
```

**懒汉式：**在外部调用时才会加载，是线程不安全的，可以加锁保证线程安全但效率低。

```java
public class LazySingleton {
    private LazySingleton(){}
    
    private static LazySingleton instance;
    
    public static LazySingleton getInstance() {
        if(instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

**双重检查锁：**使用 volatile 以及两次检查来减小 synchronized 锁范围，提升效率。

```java
public class DoubleCheckSingleton {
    private DoubleCheckSingleton(){}
    
    private volatile static DoubleCheckSingleton instance;
    
    public static DoubleCheckSingleton getInstance() {
        if(instance == null) {
            synchronized (DoubleCheckSingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckSingleton();
                }
            }
        }
        return instance;
    }
}
```

**静态内部类：**可以同时解决饿汉式的内存浪费问题和懒汉式的线程安全问题。

```java
public class StaticSingleton {
    private StaticSingleton(){}
    
    public static StaticSingleton getInstance() {
        return StaticClass.instance;
    }
    
    private static class StaticClass {
        private static final StaticSingleton instance = new StaticSingleton();
    }
}
```

**枚举：**这种方式是 Effective Java 作者提倡的方式，它不仅能避免多线程同步问题，还能防止反序列化重新创建新的对象，绝对防止多次实例化，也能防止反射破解单例的问题。

```java
public enum EnumSingleton {
    INSTANCE;
}
```

---

### <font color="red">P5：代理模式</font>

代理模式属于结构型模式，为其他对象提供一种代理以控制对这个对象的访问，可以增强目标对象的功能。优点是可以增强目标对象的功能，一定程度降低代码耦合度，扩展性好。缺点是在客户端和目标对象之间增加代理对象会导致请求处理速度变慢，同时也会增加系统复杂度。

**静态代理：**代理对象持有真实对象的引用，调用代理对象方法时也会调用真实对象的方法，但是会在真实对象方法的前后增加一些其他逻辑。需要手动完成代理操作，在程序运行前就已经存在代理类的字节码文件，代理类和被代理类的关系在运行前就已经确定了。 缺点是一个代理类只能为一个目标类服务，如果要服务多种类型就会增加很大的工作量。

```java
public interface Company {
    void findWorker();
}

public class Hr implements Company {
    @Override
    public void findWorker() {
        System.out.println("我需要找招聘一个员工");
    }
}

public class Proxy implements Company {
    private Hr hr;

    public Proxy(){
        this.hr = new Hr();
    }

    @Override
    public void findWorker() {
        hr.findWorker();
        System.out.println("找到了员工");
    }

}
```

**动态代理：**动态代理在程序运行时才通过反射创建具体的代理类，代理类和被代理类的关系在运行前是不确定的。动态代理的适用性更强，主要分为 JDK 动态代理和 CGLib 动态代理。

- **JDK 动态代理：**通过 `Proxy `类的 `newInstance` 方法获取一个动态代理对象，需要传入三个参数，被代理对象的类加载器、被代理对象实现的接口，以及一个 `InvocationHandler` 调用处理器实例来指明具体的逻辑，相比静态代理最大的优势是接口中声明的所有方法都被转移到 `InvocationHandler` 中的 `invoke` 方法集中处理。

  ```java
  public static void main(String[] args) {
      Hr hr = new Hr();
      Company proxyHr = (Company) Proxy.newProxyInstance(hr.getClass().getClassLoader(), hr.getClass().getInterfaces(), (proxy, method, args1) -> {
          System.out.println("接收代理请求");
          Object obj = method.invoke(hr, args1);
          System.out.println("找到了员工，完成请求");
          return obj;
      });
      proxyHr.findWorker();
  }
  ```

- **CGLib 动态代理：**与 JDK 动态代理不同的是，JDK 动态代理要求实现被代理对象的接口，而 CGLib 要求继承被代理对象，如果一个类是 final 类则不能使用 CGLib 动态代理。两种代理都是在运行期生成字节码，JDK 动态代理直接写字节码，而 CGLib 动态代理使用 ASM 框架写字节码，ASM 作用于已编译好的 Class 文件，其目的是生成、转换和分析以字节数组表示的已编译 Java 类。 JDK 动态代理调用代理方法是通过反射机制实现的，而 GCLib 动态代理是通过 FastClass 机制直接调用方法的，为代理类和被代理类各生成一个类，该类为代理类和被代理类的方法会分配一个 int 类型的参数，调用方法时可以直接定位而省去反射，因此调用方法的效率更高。

---

### P6：装饰器模式

指在不改变原有对象的基础上，将功能附加到对象上，相比继承可以更加灵活地扩展原有对象的功能，属于结构型模式。这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下提供了额外的功能。装饰器模式适合的场景：在不想增加很多子类的前提下扩展一个类的功能或给一个类添加附加职责、动态地给一个类添加功能，这些功能可以再动态地撤销。

**和动态代理的区别：**装饰器模式的关注点在于给对象动态添加方法，而动态代理更注重对象的访问控制。动态代理通常会在代理类中创建被代理对象的实例，而装饰器模式会将装饰者作为构造器的参数。

---

### P7：适配器模式

适配器模式属于结构型模式，它作为两个不兼容的接口之间的桥梁，结合了两个独立接口的功能，将一个类的接口转换成另外一个接口，这种模式涉及到一个单一的类，该类负责加入独立的或不兼容的接口功能。优点是使得原本由于接口不兼容而不能一起工作的类可以一起工作。 缺点是过多使用适配器会让系统非常零乱，不易整体进行把握。

**和装饰器模式的区别：**适配器模式的是要将一个接口转变成另一个接口，目的是通过改变接口来解决接口不兼容的问题。而装饰器模式不是要改变被装饰对象的接口，而是要增强原有对象的功能。例如 java.io 包中，适配器模式是将 InputStream 字节输入流通过适配器 InputStreamReader 转换为 Reader 字符输入流，而装饰器模式是将 InputStream 通过装饰器 BufferedInputStream 增强为缓冲字节输入流。

---

## Java 基础 20

### P1：Java 语言的基本概念

**优点：**

- 具有平台无关性，摆脱了硬件平台的束缚，实现了“一次编写，到处运行”的理想。

- 提供了一种相对安全的内存管理和访问机制，避免了绝大部分内存泄漏和指针越界问题。

- 实现了热点代码检测和运行时编译及优化，使得 Java 程序随运行时间增长可以获得更高的性能。

- 有一套完善的应用程序接口，还支持很多第三方类库。

**Java 平台无关性原理：**

主要是通过 JVM 和 Java 语言规范实现。

- 编译器生成一个体系结构中立的目标文件格式，这是一种编译后的代码，只要有 Java 运行时系统，这些编译后的代码可以在很多处理器上运行。Java 编译器通过生成与特定计算机体系结构无关的字节码指令来实现这一特性，字节码文件不仅可以很容易地在任何机器上解释执行，还可以动态地转换成本地机器代码，转换是由 JVM 实现的，JVM 是平台相关的，屏蔽了不同操作系统的差异。
- Java 中基本数据类型的大小以及有关运算的行为都有明确的说明，例如 Java 中的 int 类型永远为 32 位的整数，而在 C/C++ 中 int 可能是 16 位整数、32 位整数，也可能是编译器开发商指定的其他任何大小。在 Java 中数值类型有固定的字节数，二进制数据以固定的格式进行存储和传输，字符串则采用标准的 Unicode 格式存储。

**专业术语：**

- JDK：Java Development Kit，Java 开发工具包。它提供了编译、运行 Java 程序所需的各种工具和资源，包括 Java 编译器、JRE 以及常用的 Java 基础类库等，是 JAVA 的核心。JDK 是编写 Java 程序的程序员使用的软件。
- JRE：Java Runtime Environment，Java 运行时环境，是运行基于 Java 语言编写的程序所不可缺少的运行环境。JRE 是运行 Java 程序的用户使用的软件。
- SE：Standard Edition，标准版，用于桌面或简单服务器应用的 Java 平台。
- EE：Enterprise Edition，企业版，用于复杂服务器应用的 Java 平台。
- ME：Micro Edition，微型版，用于小型设备的 Java 平台。

---

### P2：Java 基本数据类型

| 数据类型 | 占用内存大小                                                 | 取值范围                     |
| -------- | ------------------------------------------------------------ | ---------------------------- |
| byte     | 1 字节                                                       | -2^7^ ~ 2^7^-1               |
| short    | 2 字节                                                       | -2^15^ ~ 2^15^-1             |
| int      | 4 字节                                                       | -2^31^ ~ 2^31^-1             |
| long     | 8 字节                                                       | -2^63^ ~ 2^63^-1             |
| float    | 4 字节                                                       | ±3.4E+38F（有效位数 6~7 位） |
| double   | 8 字节                                                       | ±1.7E+308（有效位数 15 位）  |
| char     | 英文在 UTF-8 和 GBK 中均占 1 字节，中文在 UTF-8 占 3 字节，GBK 占 2 字节。 | /                            |
| boolean  | 单个变量用 int 代替，占 4 字节，而数组会编码成 byte 数组，占 1 字节。 | true、false                  |

每个基本数据类型都对应一个自己的包装类，除了 int 和 char 对应 Integer 和 Character 之外，其余基本数据类型的包装类都是首字母大写即可。自动装箱指的是将基本数据类型包装为一个包装类对象，例如向一个泛型为 Integer 类型的集合添加 int 类型的元素。自动拆箱指的是将一个包装类对象转换为一个基本数据类型，例如将一个包装类对象赋值给一个基本数据类型的变量。要比较两个包装类的数值需要使用 `equals` 方法，而不能使用 `==` 比较运算符。

---

### P3：String

**不可变性：**String 是不可变类，并且存储数据的 value 字符数组也是 final 修饰的不可变数组，因此当修改一个 String 变量的值时，并没有真正修改它引用的 String 对象的字符数组中的值，而是重新创建了一个 String 对象赋值给了 String 变量进行引用。

**字符串拼接：**直接使用 + 进行字符串拼接，如果是字面量会自动拼接为一个新的常量。要提升拼接效率可以使用 StringBuilder 或 StringBuffer 可变字符串，区别是 StringBuffer 使用了 synchronized 保证线程安全性，但一般字符串拼接都是单线程操作，所以使用 StringBuilder 较多。常量和常量的拼接，结果也在常量池中，且不存在两个相同的常量。只要参与拼接的字符串里有变量，结果就在堆中。

**创建：** 如果是通过字符串常量赋值的形式，例如 String s = "s"，字符串常量内容存于常量池，变量存于栈中并直接引用常量池中的字符串。如果是通过new 的形式，例如 `String s = new String("s")`，会先在堆中创建实例对象，然后再去常量池寻找需要的字符串常量，如果找到了则直接使用，没找到则开辟新的空间并存储内容，最后栈中变量引用堆中对象，对象再引用常量池中的字符串。

---

### P4：值调用和引用调用

按值调用指的是方法接收的是调用者提供的值，而按引用调用指的是方法接收的是调用者提供的变量地址。Java 总是采用按值调用，也就是说方法得到的是所有参数值的一个副本，当传递对象时实际上方法接收的是这个对象引用的副本。方法不能修改基本数据类型的参数，可以改变对象参数的状态，但不能让对象参数引用一个新的对象。

举例来说，如果传递了一个 int 类型的值 ，改变该值不会影响实参，因为改变的是该值的一个副本。如果传递了一个 int[] 类型的数组，改变数组的内容会影响实参，而如果改变这个参数的引用，并不会让实参引用新的数组对象。

---

### <font color="red">P5：面向对象</font>

**概念：**面向对象是一种程序设计思想，相对于面向过程而言更适合解决规模较大的问题。采用面向对象的开发方式可以对现实的事物进行抽象，把现实的事物映射为开发对象，接近人的思维。并且可以通过继承或组合的方式实现代码的重用，因此开发效率高。并且面向对象的开发方式提高了代码的可读性，使代码结构更加清晰，方便代码的维护。

**特性：**

- **封装：**也称数据隐藏，从形式上看就是将数据和行为组合在一个包中，并对对象的使用者隐藏具体的实现方式。
- **继承：**可以通过继承来扩展一个类，扩展的子类可以继承父类的属性和方法，并可以添加自己独有的属性和方法。Java 中类只可以单继承，接口之间是可以多继承的。继承是一种"is-a"的关系，可以提高代码的复用性。
- **多态：**父类的变量可以引用一个子类的对象，在运行时通过动态绑定来决定调用的方法。
  - **重载：**是指同一个类中具有多个方法名相同而方法参数列表不同的方法，重载方法的返回值类型不做要求，但方法的参数列表必须不同。重载属于一种编译时多态。
  - **重写：**是指子类具有和父类方法名和方法参数列表都相同的方法，要求返回值不大于父类方法的返回值，抛出的异常类型不大于父类方法抛出的异常类型，访问修饰符可见性不小于父类方法的访问修饰符可见性。重写属于一种运行时多态。

---

### P6：方法修饰符

| 访问修饰符 | 本类可见性 | 本包可见性 | 子类可见性 | 不同包可见性 |
| ---------- | ---------- | ---------- | ---------- | ------------ |
| public     | √          | √          | √          | √            |
| protected  | √          | √          | √          | ×            |
| 默认       | √          | √          | ×          | ×            |
| private    | √          | ×          | ×          | ×            |

---

### P7：接口和抽象类

**成员变量：**接口中的成员变量默认是 public static final 修饰的常量，抽象类中的成员变量无特殊要求。

**构造器：**接口和抽象类都不能直接实例化，但接口没有构造器，抽象类是有构造器的。

**方法：**接口中的方法默认是 public 修饰的，Java 8 开始支持默认方法和静态方法，Java 9 开始支持私有方法。抽象类中的方法不做要求，抽象类可以不含抽象方法，但含有抽象方法的类一定是抽象类。

**继承：**接口可以多继承和多实现，而抽象类只能单继承。

**选择原则：**如果知道某个类应该成为基类，那么第一选择应该是让它成为一个接口，只有在必须要有方法定义和成员变量的时候，才应该选择抽象类。在接口和抽象类的选择上，必须遵守这样一个原则：行为模型应该总是通过接口而不是抽象类定义。通过抽象类建立行为模型会出现的问题：如果有一个产品类 A，有两个子类 B 和 C 分别有自己的功能，如果出现一个既有 B 产品功能又有 C 产品功能的新产品需求，由于 Java 不允许多继承就出现了问题，而如果是接口的话只需要同时实现两个接口即可。

----

### P8：Object 类

Object 的类是所有类的父类，Object 类的方法：

- **equals：**用于检测一个对象是否等于另一个对象，默认使用 `==` 比较两个对象的引用，可以重写 equals 方法自定义比较规则。equals 方法需要满足以下规范：自反性、对称性、传递性、一致性并对于任何非空引用 x，`x.equals(null)`  返回 false。
- **hashCode：**散列码是由对象导出的一个整型值，是没有规律的，每个对象都有一个默认的散列码，值由对象的存储地址得出。字符串可能有相同的散列码，因为字符串的散列码是由内容导出的。为了在集合中正确使用对象，一般需要同时重写 equals 和 hashCode 方法，要求是 equals 相同是 hashCode 必须相同，但 hashCode 相同时 equals 未必相同，因此 hashCode 是两个对象相同的必要不充分条件。
- **toString**：打印对象时默认会调用它的 toString 方法，如果没有重写该方法默认打印的是表示对象值的一个字符串，一般需要重写该方法。打印数组时可以使用 `Arrays.toString()` 方法。
- **clone：**clone 方法声明为 protected，类只能通过该方法克隆它自己的对象，如果希望其他类也能调用该方法必须定义该方法为 public。如果一个对象的类没有实现 Cloneable 接口，该对象调用 clone 方法会抛出一个 CloneNotSupport 异常。默认的 clone 方法是浅拷贝，一般重写 clone 方法需要实现 Cloneable 接口并指定访问修饰符为 public。
  - **浅拷贝：**如果对象包含子对象的引用，拷贝字段就会得到相同子对象的另一个引用，如果共享的子对象是不可变的则是安全的，通常子对象都是可变的，因此浅拷贝是不安全的，拷贝对象的更改会影响原对象。
  - **深拷贝：**会完全拷贝基本数据类型和引用数据类型，深拷贝是安全的。
- **finalize：**要确定一个对象死亡至少要经过两次标记，如果对象在进行可达性分析后发现没有与GC Roots 连接的引用链，会被第一次标记，随后进行一次筛选，筛选的条件是此对象是否有必要执行 finalize 方法。假如对象没有重写 finalize 方法或者该方法已经被虚拟机调用过，那么这两种情况视为没有必要执行。如果这个对象被判断为有必要执行，就会被放置在一个叫做 F-Queue 的队列中，由一条虚拟机自动建立的低调度优先级的 Finalizer 线程去执行它们的 finalize 方法。虚拟机会触发该方法但不保证它会运行结束，这是为了防止某个对象的 finalize 方法执行缓慢或发生死循环。只要对象在 finalize 方法中重新与引用链上的任何一个对象建立关联关系就会在第二次标记时被移出即将回收集合，否则就会被真正回收。由于运行代价高昂，不确定性大，无法保证各个对象的调用顺序，如今已被官方明确声明为不推荐使用的语法，因此它并不适合释放资源，释放资源完全可以使用 try-finally 代码块。
- **getClass：**返回包含对象信息的类对象。
- **wait / notify / notifyAll：**阻塞或唤醒持有该对象锁的线程。

---

### P9：内部类

使用内部类主要有两个原因：内部类可以对同一个包中的其他类隐藏。内部类方法可以访问定义这个内部类的作用域中的数据，包括原本私有的数据。内部类是一个编译器现象，与虚拟机无关。编译器会把内部类转换成常规的类文件，用美元符号 $ 分隔外部类名与内部类名，而虚拟机对此一无所知。

**静态内部类：**由static修饰，属于外部类本身，只加载一次。类可以定义的成分静态内部类都可以定义，可以访问外部类的静态变量和方法，通过 `new 外部类.内部类构造器` 来创建对象。只要内部类不需要访问外部类对象，就应该使用静态内部类。

**成员内部类：**属于外部类的每个对象，随对象一起加载。不可以定义静态成员和方法，可以访问外部类的所有内容，通过 `new 外部类构造器.new 内部类构造器` 来创建对象。

**局部内部类：**定义在方法、构造器、代码块、循环中。不能声明访问修饰符，只能定义实例成员变量和实例方法，作用范围仅在声明这个局部类的代码块中。

**匿名内部类：**没有名字的局部内部类，可以简化代码，匿名内部类会立即创建一个匿名内部类的对象返回，对象类型相当于当前 new 的类的子类类型。匿名内部类一般用于实现事件监听器和其他回调。

```java
class OuterClass{

    static class StaticInnerClass {}

    class NormalInnerClass {}

    public void test() {
        
        class LocalClass {}

        // 静态内部类创建对象
        new OuterClass.StaticInnerClass();
        
        // 成员内部类创建对象
        new OuterClass().new NormalInnerClass();
        
        // 局部内部类创建对象
        new LocalClass();
        
        // 匿名内部类创建对象
        Runnable runnable = () -> {};
    }
}
```

----

### P10：static 关键字

static 关键字主要有两个作用：（1）为某特定数据类型或对象分配单一的存储空间，而与创建对象的个数无关。（2）让某个属性或方法与类而不是对象关联在一起，可以在不创建对象的情况下通过类名来访问。

**作用范围：**

static 修饰的变量称为静态变量，也叫做类变量，可以直接通过类名来访问，静态变量存储在 JVM 的方法区中。

static 修饰的方法称为静态方法，也叫做类方法，可以直接通过类名来访问，静态方法只能访问静态变量或静态方法。

static 修饰的代码块称为静态代码块，只能定义在类下，会在类加载时执行，只会执行一次。

static 修饰的类称为静态内部类，可以访问外部类的静态变量和方法。

static 也可以用来导入包下的静态变量。

**类初始化的顺序：**

（1）父类静态代码块和静态变量
（2）子类静态代码块和静态变量
（3）父类普通代码块和普通变量
（4）父类构造器
（5）子类普通代码块和普通变量
（6）子类构造器

其中代码块和变量的初始化顺序按照类中声明的顺序执行。

---

### P11：序列化和反序列化

Java 对象在 JVM 运行时被创建，当 JVM 退出时存活对象都会销毁，如果需要将对象及其状态持久化，就需要通过序列化来实现，将对象及其状态信息保存在字节数组中，在需要时再将这些字节数组反序列化为对象。对象序列化保存的是对象的状态，因此类中的静态变量不会被序列化，因为静态变量是类属性。

要实现序列化功能需要实现 `java.io.Serializabale` 标记接口，序列化和反序列化必须保持序列化 ID 的一致，一般使用 `private static final long serialVersionUID` 定义序列化 ID，如果需要序列化父类的状态，父类也需要实现该接口。

有许多序列化框架，例如 fastjson、thrift等，也可以使用 JDK 自带的 ObjectOutputStream 类的 writeObject 方法实现序列化，将对象以流的方式写入磁盘中，ObjectInputStream 类的 readObject 方法实现反序列化，以流的方式从磁盘读取。

除了静态变量外，transient 修饰的变量也不会被序列化。transient 的作用就是把这字段的生命周期仅限于内存中而不会写到磁盘里持久化，被 transient 修饰的变量会被设为对应数据类型的默认初始值。

除了实现 Serializabale 接口外，另一种方法是实现 Exteranlizable 接口。 需要重写 writeExternal 和 readExternal 方法，它的效率比Serializable 高一些，并且可以决定哪些属性需要序列化（即使是 transient 修饰的变量），但是对大量对象或者重复对象则效率低。

---

### P12：反射

在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为Java的反射机制。优点是运行时动态获取类的全部信息，缺点是破坏了类的封装性，泛型的约束性。反射是框架的核心灵魂，动态代理设计模式采用了反射机制，还有 Spring、Hibernate 等框架也大量使用到了反射机制。

在程序运行期间，Java 运行时系统始终为所有对象维护一个运行时类型标识，这个信息会跟踪每个对象所属的类，虚拟机利用运行时类型信息选择要执行的正确方法，保存这些信息的类名为 Class。

获取 Class 实例的方法有三种：（1）直接通过 `类名.class` 。②通过对象的 `getClass()`方法。③通过 `Class.forName(类的全限定名)`。Class 类中的 getFields、getMethods 和 getConstructors 方法分别返回这个类支持的公共字段、方法和构造器的数组，其中包括父类的公共成员。Class 类中的 getDeclaredFields、getDeclaredMethods 和 getDeclaredConstructors 方法分别返回这个类声明的全部字段、方法和构造器的数组，其中包括私有成员、包成员和受保护成员，但不包括父类的成员。

Field、Method、Constructor 分别用于描述类的字段、方法和构造器。这三个类都有一个 getName 方法返回字段、方法或构造器的名称。Field 类有一个 getType 方法用来返回描述字段类型的一个对象，这个对象的类型也是 Class。Method 和 Constructor 类有报告参数类型的方法，Method 类还有一个报告返回类型的方法。这三个类都有一个 getModifiers 方法，它返回一个整数，用不同的 0/1 位描述所使用的修饰符。

---

### P13：注解

注解是一种标记，可以使类或接口附加额外的信息，是帮助编译器和 JVM 完成一些特定功能的，例如常用注解 `@Override` 标识一个方法是重写方法。

元注解就是自定义注解的注解，包括：

- `@Target`：用来约束注解作用的位置，值是 ElementType 枚举类实例，包括 METHOD 方法、VARIABLE 变量、TYPE 类/接口、PARAMETER 方法参数、CONSTRUCTORS 构造器和 LOACL_VARIABLE 局部变量等。

- `@Rentention`：用来约束注解的生命周期，值是 RetentionPolicy 枚举类实例，包括：SOURCE 源码、CLASS 字节码和 RUNTIME 运行时。

- `@Documented`：表明这个注解应该被 javadoc 工具记录。

- `@Inherited`：表面某个被标注的类型是被继承的。

---

### P14：异常

所有的异常都派生于 Throwable 类的一个类实例，在下一层分为 Error 和 Exception。

Error 类描述了 Java 运行时系统的内部错误和资源耗尽错误，如果出现了这种错误，一般无能为力。

Exception 类又分为 RuntimeException 和其他异常，一般规则是由编程错误导致的异常属于 RuntimeException，如果程序本身没有问题，但由于像 IO 错误这类问题导致的异常属于其他异常。派生于 Error 和 RuntimeException 的异常属于非检查型异常，其余异常都属于检查型异常。

常见的 RuntimeException 异常：

- ClassCastException，错误的强制类型转换。
- ArrayIndexOutOfBoundsException，数组访问越界。
- NullPointerException，空指针异常。

常见的检查型异常：

- FileNotFoundException，试图打开不存在的文件。
- ClassNotFoundException，试图根据指定字符串查找 Class 对象，而这个类并不存在。
- IOException，试图超越文件末尾继续读取数据。

**异常处理：**

**抛出异常：**遇到异常不进行具体处理，而是将异常抛出给调用者，由调用者根据情况处理。抛出异常有2种形式，一种是 throws 关键字声明抛出的异常，作用在方法上，一种是使用throw 语句直接抛出异常，作用在方法内。

**捕获异常：**使用 try/catch 进行异常的捕获，try 中发生的异常会被 catch 代码块捕获，根据情况进行处理，如果有 finally 代码块无论是否发生异常都会执行，一般用于释放资源，Java 7 开始可以将资源定义在 try 代码块中自动释放资源。

---

### P15：泛型

泛型的本质是参数化类型，泛型提供了编译时类型的安全检测机制，该机制允许程序在编译时检测非法的类型。

**类型擦除：**

虚拟机没有泛型类型对象，所有对象都属于普通类。无论何时定义一个泛型类型，都会自动提供一个相应的原始类型，原始类型的名字就是去掉类型参数后的泛型类型名。类型变量会被擦除，如果没有限定类型就会替换为 Object，如果有限定类型就会替换为第一个限定类型，例如 `<T extends A & B>` 会使用 A 类型替换 T。

泛型主要用于编译阶段，在编译后生成的 Java 字节代码文件中不包含泛型中的类型信息。

**泛型规范：**

| 泛型标记     | 说明                                   |
| ------------ | -------------------------------------- |
| E（Element） | 在集合中使用，表示在集合中存放的元素。 |
| T（Type）    | 表示类，包括基本的类以及自定义类。     |
| K（Key）     | 表示键，例如 Map 集合中的 Key。        |
| V（Value）   | 表示值，例如 Map 集合中的 Value。      |
| N（Number）  | 表示数值类型。                         |
| ？           | 表示不确定的类型。                     |

**泛型限定：**

对泛型上限的限定使用<? extends T>，它表示该通配符所代表的类型是 T 类的子类型或 T 接口的子接口。

对泛型下限的限定使用<? super T>，它表示该通配符所代表的类型是 T 类的父类型或 T 接口的父接口。

---

### <font color="red">P16：Java 8 新特性</font>

**lambda 表达式：**lambda 表达式允许把函数作为一个方法的参数传递到方法中，主要用来简化匿名内部类的代码。

**函数式接口：**使用 `@FunctionalInterface` 注解标识，有且仅有一个抽象方法，可以被隐式转换为 lambda 表达式。

**方法引用：**可以直接引用已有类或对象的方法或构造器，进一步简化 lambda 表达式。方法引用有四种形式：引用构造方法、引用类的静态方法、引用特定类的任意对象方法、引用某个对象的方法。

**接口中的方法：**接口中可以定义 `default ` 修饰的默认方法，降低了接口升级的复杂性，还可以定义静态方法。

**注解：**Java 8 引入了重复注解机制，相同的注解在同一个地方可以声明多次。注解的作用范围也进行了扩展，可以作用于局部变量、泛型、方法异常等。

**类型推测：**加强了类型推测机制，可以使代码更加简洁，例如在定义泛型集合时可以省略对象中的泛型参数。

**Optional 类：**用来处理空指针异常，提高代码可读性。

**Stream 类：**把函数式编程风格引入 Java 语言，提供了很多功能，可以使代码更加简洁。方法包括`forEach()` 遍历、`count()` 统计个数、`filter()` 按条件过滤、`limit()` 取前 n 个元素、`skip()` 跳过前 n 个元素、`map()` 映射加工、`concat()` 合并stream流等。

**日期：**增强了日期和时间的 API，新的 java.time 主要包含了处理日期、时间、日期/时间、时区、时刻和时钟等操作。

**JavaScript：**Java 8 提供了一个新的 Nashorn JavaScript 引擎，它允许我们在 JVM上运行特定的 JavaScript 应用。

---

### P17：Java IO

| IO 模型              | 对应的 Java 版本 |
| -------------------- | ---------------- |
| BIO（同步阻塞 IO）   | 1.4 之前         |
| NIO（同步非阻塞 IO） | 1.4              |
| AIO（异步非阻塞 IO） | 1.7              |

**同步和异步是通信机制，阻塞和非阻塞是调用状态。**

- 同步 IO 是用户线程发起 I/O 请求后需要等待或者轮询内核 I/O 操作完成后才能继续执行。

- 异步 IO 是用户线程发起 I/O 请求后仍可以继续执行，当内核 I/O 操作完成后会通知用户线程，或者调用用户线程注册的回调函数。

- 阻塞 IO 是指 I/O 操作需要彻底完成后才能返回用户空间 。
- 非阻塞 IO 是指 I/O 操作被调用后立即返回一个状态值，无需等 I/O 操作彻底完成。

**BIO：**

同步阻塞式 IO，服务器实现模式为一个连接请求对应一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销。可以通过线程池机制改善，这种 IO 称为伪异步 IO。

主要分为字符流和字节流，字符流包括字符输入流 Reader 和字符输出流 Writer，字节流包括字节输入流 InputStream 和 字节输出流 OutputStream，字节流和字符流都有对应的缓冲流和过滤流，也可以将字节流包装为字符流。

**适用场景：**连接数目少、服务器资源多、开发难度低。

---

**NIO：**

同步非阻塞 IO，服务器实现模式为多个连接请求对应一个线程，客户端发送的连接请求都会注册到一个多路复用器 Selector 上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理，有数据才会开启线程处理，性能比较好。

**同步是指线程还是要不断接收客户端连接并处理数据，非阻塞是指如果一个管道没有数据，不需要等待，可以轮询下一个管道。**

有三个核心组件：

- **Selector**

  选择器或多路复用器，主要作用是轮询检查多个 Channel 的状态，判断 Channel 注册的事件是否发生，即判断 Channel 是否处于可读或可写状态。在使用之前需要将 Channel 注册到 Selector 上，注册之后会得到一个 SelectionKey，通过 SelectionKey 可以获取 Channel 和 Selector 的相关信息。

- **Channel**

  双向通道，替换了 IO 中的 Stream，不能直接访问数据，要通过 Buffer 来读写数据，也可以和其他 Channel 交互。

  **分类：**FileChannel 处理文件、DatagramChannel 处理 UDP 数据、SocketChannel 处理 TCP 数据，用作客户端、ServerSocketChannel 处理 TCP 数据，用作服务器端。

- **Buffer**

  缓冲区，本质是一块可读写数据的内存，这块内存被包装成 NIO 的 Buffer 对象，用来简化数据的读写。Buffer 的三个重要属性：position 表示下一次读写数据的位置，limit 表示本次读写的极限位置，capacity 表示最大容量。

  -  `flip()` 将写转为读，底层实现原理是把 position 置 0，并把 limit 设为当前的 position 值。
  -  通过 `clear()` 将读转为写模式（用于读完全部数据的情况，把 position 置 0，limit 设为 capacity）。
  -  通过 `compact()` 将读转为写模式（用于没有读完全部数据，存在未读数据的情况，让 position 指向未读数据的下一个）。
  -  <font color ="red">**通道的方向和 Buffer 的方向是相反的，读取数据相当于向 Buffer 写入，写出数据相当于从 Buffer 读取。**</font>

  **使用步骤：**向 Buffer 写入数据，调用 flip 方法将 Buffer 从写模式切换为读模式，从 Buffer 中读取数据，调用 clear 或 compact 方法来清空 Buffer。

**适应场景：**连接数目多、连接时间短、开发难度高。

---

**AIO：**

异步非阻塞 IO，服务器实现模式为一个有效请求对应一个线程,客户端的 I/O 请求都是由操作系统先完成 IO 操作后再通知服务器应用来启动线程直接使用数据。

**异步是指服务端线程接收到客户端管道后就交给底层处理IO通信，自己可以做其他事情，非阻塞是指客户端有数据才会处理，处理好再通知服务器。**

AsynchronousServerSocketChannel 异步服务器端通道，通过静态方法 `open()` 获取实例，通过 accept 方法获取客户端连接通道。

AsynchronousSocketChannel 异步客户端通道，通过静态方法 `open()` 获取实例，过 connect 方法连接服务器通道。

AsynchronousChannelGroup 异步通道分组管理器，它可以实现资源共享。创建时需要传入一个ExecutorService，也就是绑定一个线程池，该线程池负责两个任务：处理 IO 事件和触发 CompletionHandler 回调接口。

**实现方式：**

通过 Future 的 get 方法进行阻塞式调用。

通过实现 CompletionHandler 接口，重写请求成功的回调方法 completed() 和 请求失败回调方法 failed()。

**适用场景：**连接数目多、连接时间长、开发难度高。

----

### P18：List 

List 是一种线性列表结构，元素是有序、可重复的。

**ArrayList **

底层由数组实现，随机访问效率高，读快写慢，由于写操作涉及元素的移动，因此写操作效率低。

ArrayList 实现了 RandomAcess 标记接口，如果一个类实现了该接口，那么表示这个类使用索引遍历比迭代器更快。

三个重要的成员变量：

```java
transient Object[] elementData;
```

elementData 是 ArrayList 的数据域，transient 表示它不会被序列化，不使用 elementData 直接序列化是因为这是一个缓存数组，出于性能考虑通常会预留一些容量，当容量不足时会扩充容量，因此可能会有大量空间没有存储元素，采用这样的方式可以保证只序列化实际有值的那些元素而不需要序列化整个数组。

```java
private int size;
```

size 表示当前 List 的实际大小，elementData 的大小是大于等于 size 的。

```java
protected transient int modCount = 0;
```

该成员变量继承自 AbstractList，记录了ArrayList 结构性变化的次数。所有涉及结构变化的方法都会增加该值，包括add()、remove()、addAll()、removeRange() 及clear() 等。

在使用迭代器遍历 ArrayList 时不能修改元素，modCount 统计 ArrayList 修改次数，expectedModCount 则是在迭代器初始化时记录的modCount 值，每次访问新元素时都会检查 modCount 和 expectedModCount是否相等，如果不相等就会抛出异常。

**LinkedList**

底层由链表实现，与 ArrayList 相反，需要顺序访问元素，即使有索引也需要从头遍历，因此写快读慢。

LinkedList 实现了 Deque 接口，具有队列的属性，可在尾部增加元素，在头部获取元素，也能操作头尾之间任意元素。

所有成员变量都被 transient 修饰，序列化原理和ArrayList类似。

**Vector 和 Stack**

Vector 的实现和 ArrayList 基本一致，底层使用的也是数组，它和 ArrayList 的区别主要在于：（1）Vector 的所有公有方法都使用了 synchronized 修饰保证线程安全性。（2）增长策略不同，Vector 多了一个成员变量 capacityIncrement 用于标明扩容的增量。

Stack 是 Vector 的子类，实现和 Vector基本一致，与之相比多提供了一些方法表达栈的含义。

---

### **P19：HashSet**

HashSet 中的元素是无序、不重复的，最多只能有一个 null 值。

HashSet 的底层是通过 HashMap 实现的，HashMap 的 key 值即 HashSet 存储的元素，所有 key 都使用相同的 value ，一个static final 修饰的变量名为 PRESENT 的 Object 类型的对象。

由于 HashSet 的底层是 HashMap 实现的，HashMap 是线程不安全的，因此 HashSet 也是线程不安全的。

**去重：**
对于基本类型的包装类，直接按值进行比较。对于引用数据类型，会先比较 hashCode() 返回值是否相同，如果不同则代表不是同一个对象，如果相同则继续比较equals()方法返回值是否相同，都相同说明是同一个对象。

---

### <font color="red">P20：HashMap</font>

**JDK 8 之前**

底层实现是数组 + 链表，主要成员变量包括：存储数据的 table 数组、键值对数量 size、加载因子 loadFactor。

table 数组用于记录 HashMap 的所有数据，它的每一个下标都对应一条链表，所有哈希冲突的数据都会被存放到同一条链表中，Entry 是链表的节点元素，包含四个成员变量：键 key、值 value、指向下一个节点的指针 next 和 元素的散列值 hash。

在 HashMap 中数据都是以键值对的形式存在的，键对应的 hash 值将会作为其在数组里的下标，如果两个元素 key 的 hash 值一样，就会发送哈希冲突，被放到同一个下标中的链表上，为了使 HashMap 的查询效率尽可能高，应该使键的 hash 值尽可能分散。

HashMap 默认初始化容量为 16，扩容容量必须是 2 的幂次方、最大容量为 1<< 30 、默认加载因子为 0.75。

**1.put 方法：添加元素**

① 如果 key 为 null 值，直接存入 table[0]。② 如果 key 不为 null 值，先计算 key 对应的散列值。③ 调用 indexFor 方法根据 key 的散列值和数组的长度计算元素存放的下标 i。④ 遍历 table[i] 对应的链表，如果 key 已经存在，就更新其 value 值然后返回旧的 value 值。⑤ 如果 key 不存在，就将 modCount 的值加 1，使用 addEntry 方法增加一个节点，并返回 null 值。

**2.hash 方法：计算元素 key 对应的散列值**

① 处理 String 类型的数据时，直接调用对应方法来获取最终的hash值。② 处理其他类型数据时，提供一个相对于 HashMap 实例唯一不变的随机值 hashSeed 作为计算的初始量。③ 执行异或和无符号右移操作使 hash 值更加离散，减小哈希冲突的概率。

**3.indexFor 方法：计算元素下标**

直接将 hash 值和数组长度 - 1 进行与操作并返回，保证计算后的结果不会超过 table 数组的长度范围。

**4.resize 方法：根据newCapacity 来确定新的扩容阈值 threshold**

① 如果当前容量已经达到了最大容量，就将阈值设置为 Integer 的最大值，之后扩容就不会再触发。② 创建一个新的容量为 newCapacity 的 Entry 数组，并调用 transfer 方法将旧数组的元素转移到新数组.③ 将阈值设为（newCapacity 和加载因子 loadFactor 的积）和（最大容量 + 1 ）的较小值。

**5.transfer：转移旧数组到新数组**

① 遍历旧数组的所有元素，调用 rehash 方法判断是否需要哈希重构，如果需要就重新计算元素 key 的散列值。② 调用 indexFor 方法根据 key 的散列值和数组的长度计算元素存放的下标 i，利用头插法将旧数组的元素转移到新的数组。

**6.get 方法：根据 key 获取元素的 value 值**

① 如果 key 为 null 值，调用 getForNullKey 方法，如果 size 为 0 表示链表为空，返回 null 值。如果 size 不为 0，说明存在链表，遍历 table[0] 的链表，如果找到了 key 为 null 的节点则返回其 value 值，否则返回 null 值。② 调用 getEntry 方法，如果 size 为 0 表示链表为空，返回 null 值。如果 size 不为 0，首先计算 key 的散列值，然后遍历该链表的所有节点，如果节点的 key 值和 hash 值都和要查找的元素相同则返回其 Entry 节点。③ 如果找到了对应的 Entry 节点，使用 getValue 方法获取其 value 值并返回，否则返回 null 值。

---

**JDK 8 开始 **

使用的是数组 + 链表/红黑树的形式，table 数组的元素数据类型换成了 Entry 的静态实现类 Node。

**1.put 方法：添加元素**

① 调用 putVal 方法添加元素。② 如果 table 为空或没有元素时就进行扩容，否则计算元素下标位置，如果不存在就新创建一个节点存入。③ 如果首节点和待插入元素的 hash值和 key 值都一样，直接更新 value 值。④ 如果首节点是 TreeNode 类型，调用 putTreeVal 方法增加一个树节点，每一次都比较插入节点和当前节点的大小，待插入节点小就往左子树查找，否则往右子树查找，找到空位后执行两个方法：balanceInsert 方法，把节点插入红黑树并对红黑树进行调整使之平衡。moveRootToFront 方法，由于调整平衡后根节点可能变化，table 里记录的节点不再是根节点，需要重置根节点。⑤ 如果是链表节点，就遍历链表，根据 hash 值和 key 值判断是否重复，决定更新值还是新增节点。如果遍历到了链表末尾，添加链表元素，如果达到了建树阈值，还需要调用 treeifyBin 方法把链表重构为红黑树。⑥ 存放元素后，将 modCount 值加 1，如果节点数 + 1大于扩容阈值，还需要进行扩容。

**2.get 方法：根据 key 获取元素的 value 值**

① 调用 getNode 方法获取 Node 节点，如果不是 null 值就返回 Node 节点的 value 值，否则返回 null。② 如果数组不为空，先比较第一个节点和要查找元素的 hash 值和 key 值，如果都相同则直接返回。③ 如果第二个节点是 TreeNode 节点则调用 getTreeNode 方法进行查找，否则遍历链表根据 hash 值和 key 值进行查找，如果没有找到就返回 null。

**3.hash 方法：计算元素 key 对应的散列值**

Java 8 的计算过程简单了许多，如果 key 非空就将 key 的 hashCode() 返回值的高低16位进行异或操作，这主要是为了让尽可能多的位参与运算，让结果中的 0 和 1 分布得更加均匀，从而降低哈希冲突的概率。

**4.resize 方法：扩容数组**

重新规划长度和阈值，如果长度发生了变化，部分数据节点也要重新排列。

**重新规划长度**

① 如果 size 超出扩容阈值，把 table 容量增加为之前的2倍。② 如果新的 table 容量小于默认的初始化容量16，那么将 table 容量重置为16。③ 如果新的 table 容量大于等于最大容量，那么将阈值设为 Integer 的最大值，并且 return 终止扩容，由于 size 不可能超过该值因此之后不会再发生扩容。

**重新排列数据节点**

① 如果节点为 null 值则不进行处理。② 如果节点不为 null 值且没有next节点，那么重新计算其散列值然后存入新的 table 数组中。③ 如果节点为 TreeNode 节点，那么调用 split 方法进行处理，该方法用于对红黑树调整，如果太小会退化回链表。④ 如果节点是链表节点，需要将链表拆分为 hashCode() 返回值超出旧容量的链表和未超出容量的链表。对于`hash & oldCap == 0` 的部分不需要做处理，反之需要放到新的下标位置上，新下标 = 旧下标 + 旧容量。

**线程不安全：**Java 7 扩容时 resize 方法调用的 transfer 方法中使用头插法迁移元素，多线程会导致 Entry 链表形成环形数据结构，Entry 节点的 next 永远不为空，引起死循环。Java 8 在 resize 方法中完成扩容，并且改用了尾插法，不会产生死循环的问题，但是在多线程的情况下还是可能会导致数据覆盖的问题，因此依旧线程不安全。

---

**红黑树：**红黑树是一种自平衡的二叉查找树。

**特性：**红黑树的每个节点只能是红色或者黑色、根节点是黑色的、每个叶子节点都是黑色的、如果一个叶子节点是红色的，它的子结点必须是黑色的、从一个节点到该节点的叶子节点的所有路径都包含相同数目的黑色节点。

**左旋：**对节点进行左旋，相当于把节点的右节点作为其父节点，即将节点变成一个左节点。

**右旋：**对节点进行右旋，相当于把节点的左节点作为其父节点，即将节点变成一个右节点。

**插入：**① 被插入的节点是根节点，直接将其涂为黑色。② 被插入节点的父节点是黑色的，不做处理，节点插入后仍是红黑树。③ 被插入节点的父节点是红色的，一定存在非空祖父节点，根据叔叔节点的颜色分类处理。

**删除：**① 被删除的节点没有子节点，直接将其删除。② 被删除节点只有一个子节点，直接删除该节点，并用其唯一子节点替换其位置。③ 被插入节点有两个子节点，先找出该节点的替换节点，然后把替换节点的数值复制给该节点，删除替换节点。

**调整平衡：**在插入和删除节点后，通过左旋、右旋或变色使其重新成为红黑树。① 如果当前节点的子节点是一红一黑，直接将该节点设为黑色。② 如果当前节点的子结点都是黑色，且当前节点是根节点，则不做处理。③ 如果当前节点的子节点都是黑色且当前节点不是根节点，根据兄弟节点的颜色分类处理。

---

## JVM 15

### <font color="red">P1：运行时数据区</font>

**程序计数器**

程序计数器是一块较小的内存空间，可以看作当前线程所执行字节码的行号指示器。字节码解释器工作时通过改变这个计数器的值来选取下一条需要执行的字节码指令，它是程序控制流的指示器，分支、循环、跳转、线程恢复等功能都需要依赖计数器完成。程序计数器是线程私有的，各条线程之间互不影响，独立存储。

如果线程正在执行的是一个 Java 方法，计数器记录的是正在执行的虚拟机字节码指令的地址。如果正在执行的是本地（Native）方法，计数器值则应为空（Undefined）。

此内存区域是唯一一个在《 Java 虚拟机规范》中没有规定任何内存溢出情况的区域。

**Java 虚拟机栈**

Java 虚拟机栈是线程私有的，每当有新的线程创建时就会给它分配一个栈空间，当线程结束后栈空间就被回收，因此栈与线程拥有相同的生命周期。栈主要用来实现方法的调用与执行，每个方法在执行的时候都会创建一个栈帧用来存储这个方法的局部变量、操作栈、动态链接和方法出口等信息。当一个方法被调用时，会压入一个新的栈帧到这个线程的栈中，当方法调用结束后会弹出这个栈帧，回收掉调用这个方法使用的栈空间。

该区域有两类异常情况：如果线程请求的栈深度大于虚拟机所允许的深度，将抛出 StackOverflowError 异常。如果 JVM 栈容量可以动态扩展，当栈扩展时无法申请到足够的内存会抛出 OutOfMemoryError 异常（HotSpot 不可以动态扩展，不存在此问题）。

**本地方法栈**

本地方法栈与虚拟机栈的作用相似，不同的是虚拟机栈为虚拟机执行 Java 方法（字节码）服务，而本地方法栈是为虚拟机栈用到的本地（Native）方法服务。调用本地方法时虚拟机栈保持不变，动态链接并直接调用指定的本地方法。

《 Java 虚拟机规范》对本地方法栈中方法所用语言、使用方式与数据结构无强制规定，具体的虚拟机可根据需要自由实现，例如 HotSpot 直接将虚拟机栈和本地方法栈合二为一。

与虚拟机栈一样，本地方法栈也会在栈深度异常和栈扩展失败时分别抛出 StackOverflowError 和 OutOfMemoryError 异常。

**Java 堆**

Java 堆是虚拟机所管理的内存中最大的一块。堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此区域的唯一目的就是存放对象实例，Java 里几乎所有的对象实例都在这里分配内存。

Java 堆可以处于物理上不连续的内存空间中，但在逻辑上它应该被视为连续的。但对于大对象（例如数组），多数虚拟机实现出于简单、存储高效的考虑会要求连续的内存空间。

Java 堆既可以被实现成固定大小的，也可以是可扩展的，不过当前主流的 JVM 都是按照可扩展来实现的。如果在堆中没有内存完成实例分配，并且堆也无法再扩展时，虚拟机将抛出 OutOfMemoryError 异常。

**方法区**

方法区和 Java 堆一样是各个线程共享的内存区域，它用于存储被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据。

JDK 8 之前使用永久代来实现方法区，这种设计导致了 Java 应用容易遇到内存溢出问题，因为永久代有`-XX:MaxPermSize`的上限，即使不设置也有默认大小。JDK 6 时 HotSpot 的开发团队就准备放弃永久代，改用本地内存来实现方法区，JDK  7 时已经把原本放在永久代的字符串常量池、静态变量等移出，到了 JDK8 时永久代被完全废弃，改用在本地内存中实现的元空间来代替，把 JDK 7 中永久代剩余内容（主要是类型信息）全部移到元空间。

《 Java 虚拟机规范》对方法区的约束很宽松，除了和 Java 堆一样不需要连续的内存和可以选择固定大小或可扩展外，还可以选择不实现垃圾回收。垃圾回收行为在该区域出现较少，主要回收目标是针对常量池的回收和对类型的卸载，一般来说该区域的回收效果比较难令人满意，尤其是类型的卸载，条件十分苛刻。如果方法区无法满足新的内存分配需求时，将抛出 OutOfMemoryError 异常。

**运行时常量池**

运行时常量池是方法区的一部分，Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表，用于存放编译器生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。一般来说，除了保存 Class 文件中描述的符号引用外，还会把符号引用翻译出来的直接引用也存储在运行时常量池中。

运行时常量池相对于 Class 文件常量池的另一个重要特征是具备动态性，Java 语言并不要求常量一定只有编译期才能产生，也就是说并非预置入 Class 文件中常量池的内容才能进入方法区运行时常量池，运行期间也可以将新的常量放入池中，这种特性被利用的较多的是String 类的 intern() 方法。

由于运行时常量池是方法区的一部分，自然受到方法区内存的限制，当常量池无法再申请到内存时会抛出 OutOfMemoryError 异常。

**直接内存**

直接内存不是 JVM 运行时数据区的一部分，也不是《 Java 虚拟机规范》中定义的内存区域，但是这部分内存也被频繁使用，而且也可能导致内存溢出异常。

JDK 1.4 中新加入了 NIO 模型，引入了一种基于通道与缓冲区的 IO 方式，它可以使用 Native 函数库直接分配堆外内存，然后通过一个存储在 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作，这样能在一些场景中显著提高性能，避免了在 Java 堆和 Native堆中来回复制数据。

本机直接内存的分配不会收到 Java 堆大小的限制，但还是会受到本机总内存大小以及处理器寻址空间的限制，一般配置虚拟机参数时会根据实际内存去设置 `-Xmx` 等参数信息，但经常忽略掉直接内存，使得各个内存区域总和大于物理内存限制，从而导致动态扩展时出现OutOfMemoryError 异常。

---

### P2：对象创建的过程

当 JVM 遇到一条字节码 new 指令时，首先将检查该指令的参数能否在常量池中定位到一个类的符号引用，并检查这个引用代表的类是否已被加载、解析和初始化，如果没有就必须先执行类加载过程。

在类加载检查通过后虚拟机将为新生对象分配内存。对象所需内存的大小在类加载完成后便可完全确定，分配空间的任务实际上等于把一块确定大小的内存块从 Java 堆中划分出来。假设 Java 堆内存是绝对规整的，所有被使用过的内存都被放在一边，空闲的内存被放在另一边，中间放着一个指针作为分界点指示器，分配内存就是把该指针向空闲方向挪动一段与对象大小相等的距离，这种方式叫"指针碰撞"。

如果 Java 堆中的内存不是规整的，那么虚拟机就必须维护一个列表记录哪些内存块是可用的，在分配时从列表中找到一块足够大的空间划分给对象实例并更新列表上的记录，这种方式叫做"空闲列表"。

选择哪种分配方式由堆是否规整决定，堆是否规整又由所用垃圾回收器是否带有空间压缩整理能力决定。因此使用 Serial、ParNew 等带压缩整理的收集器时，系统采用指针碰撞；当使用 CMS 这种基于清除算法的垃圾收集器时，理论上只能采用空间列表分配内存。

**分配内存的线程安全问题：**对象创建在虚拟机中十分频繁，即使修改一个指针所指向的位置在并发情况下也不是线程安全的，可能出现正给对象 A 分配内存，指针还没来得及修改，对象 B 又同时使用了原来的指针来分配内存的情况。解决该问题有两个方法：① 虚拟机采用 CAS 加失败重试的方式保证更新操作的原子性。② 把内存分配的动作按照线程划分在不同空间进行，即每个线程在 Java 堆中预先分配一小块内存，叫做本地线程分配缓冲 TLAB，哪个线程要分配内存就在对应的 TLAB 分配，只有 TLAB 用完了分配新缓冲区时才需要同步。

内存分配完成后虚拟机必须将分配到的内存空间（不包括对象头）都初始化为零值，保证对象的实例字段在 Java 代码中可以不赋初始值就直接使用，使程序能访问到这些字段的数据类型对应的零值。之后虚拟机还要对对象进行必要设置，例如对象是哪个类的实例、如何找到类的元数据信息等。

至此从虚拟机的视角来看一个新的对象已经产生了，但从程序的角度来说对象创建才刚开始。此时构造方法，即 Class 文件中的 init 方法还没有执行，所有字段都为默认零值，对象需要的其他资源和状态信息也还没有按照预定的意图构造好。一般来说 new 指令后会接着执行 init 方法，按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全被构造出来。

---

### **P3：对象的内存布局**

在 HotSpot 虚拟机中，对象在堆内存中的存储布局可分为三个部分。

**对象头**

对象头包括两类信息，第一类是用于存储对象自身的运行时数据，如哈希码、GC分代年龄、锁状态标志、线程持有的锁、偏向线程 ID等，这部分数据叫做"Mark Word"。

对象头的另一部分是类型指针，即对象指向它的类型元数据的指针，JVM 通过该指针来确定对象是哪个类的实例。并非所有虚拟机实现都必须在对象数据上保留类型指针，查找对象的元数据不一定要经过对象本身。此外如果对象是一个 Java 数组，在对象头还必须有一块用于记录数组长度的数据。

**实例数据**

实例数据部分是对象真正存储的有效信息，即程序员在代码里所定义的各种类型的字段内容。存储顺序会受到虚拟机分配策略参数和字段在源码中定义顺序的影响。相同宽度的字段总是被分配到一起存放，在满足该前提条件的情况下父类中定义的变量会出现在子类之前。

**对齐填充**

这部分不是必然存在的，仅仅起占位符的作用。由于 HotSpot 虚拟机的自动内存管理系统要求对象的起始地址必须是8字节的整数倍，而对象头已经被设为正好是 8 字节的整数倍，因此如果对象实例数据部分没有对齐，就需要对齐填充来补全。

---

### P4：对象的访问定位

Java 程序会通过栈上的 reference 数据来操作堆上的具体对象，而具体对象访问方式是由虚拟机决定的，主流的访问方式主要有使用句柄和直接指针两种。

**使用句柄**

如果使用句柄访问，Java 堆中将可能会划分出一块内存作为句柄池，reference 中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自具体的地址信息。

优点是 reference 中存储的是稳定句柄地址，在对象被移动（处于垃圾收集过程中）时只会改变句柄中的实例数据指针，而 reference 本身不需要被修改。

**直接指针**

如果使用直接指针访问的话，Java 堆中对象的内存布局就必须考虑如何放置访问类型数据的相关信息，reference中存储的直接就是对象地址，如果只是访问对象本身的话就不需要多一次间接访问的开销。

优点就是速度更快，节省了一次指针定位的时间开销，HotSpot 主要使用的就是直接指针来进行对象访问。

---

### P5：内存溢出异常

**Java 堆溢出**

Java 堆用于存储对象实例，我们只要不断创建对象，并且保证GC Roots到对象有可达路径来避免垃圾回收机制清除这些对象，那么随着对象数量的增加，总容量触及最大堆容量的限制后就会产生OOM异常。例如在 while 死循环中一直 new 创建实例。

Java 堆内存的 OOM 是实际应用中最常见的 OOM 情况，常规的处理方法是先通过内存映像分析工具对 Dump 出来的堆转储快照进行分析，确认内存中导致 OOM 的对象是否是必要的，即分清楚到底是出现了内存泄漏还是内存溢出。

如果是内存泄漏，可进一步通过工具查看泄漏对象到 GC Roots 的引用链，找到泄露对象是通过怎样的引用路径、与哪些GC Roots相关联才导致垃圾收集器无法回收它们，一般可以准确定位到对象创建的位置进而找出产生内存泄漏代码的具体位置。

如果不是内存泄漏，即内存中的对象确实都是必须存活的那就应当检查 JVM 的堆参数设置，与机器的内存相比是否还有向上调整的空间。再从代码上检查是否存在某些对象生命周期过长、持有状态时间过长、存储结构设计不合理等情况，尽量减少程序运行期的内存消耗。

**虚拟机栈和本地方法栈溢出**

由于HotSpot虚拟机不区分虚拟机和本地方法栈，因此设置本地方法栈大小的参数没有意义，栈容量只能由 `-Xss` 参数来设定，存在两种异常：

- **StackOverflowError：**如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常。例如一个递归方法不断调用自己。

  该异常有明确错误堆栈可供分析，容易定位到问题所在。


- **OutOfMemoryError：**如果 JVM 栈容量可以动态扩展，当栈扩展时无法申请到足够的内存会抛出OutOfMemoryError异常。HotSpot 虚拟机不支持虚拟机栈的扩展，所以除非在创建线程申请内存时就因无法获得足够内存而出现OOM异常，否则在线程运行时是不会因为扩展而导致内存溢出的，只会因为栈容量无法容纳新的栈帧而导致StackOverflowError异常。

**运行时常量池溢出**

String类的intern方法是一个本地方法，它的作用是如果字符串常量池中已经包含一个等于此 String 对象的字符串，则返回代表池中这个字符串的 String 对象的引用，否则会将此String对象包含的字符串添加到常量池中，并且返回此String对象的引用。

在 JDK6及之前常量池都分配在永久代，因此可以通过 `-XX:PermSize` 和 `-XX:MaxPermSize` 限制永久代的大小，间接限制常量池的容量。在 while 死循环中不断调用intern方法，之后将导致运行时常量池溢出。

在 JDK7 及之后版本不会导致该问题，因为存放在永久代的字符串常量池已经被移至 Java 堆中。

**方法区溢出**

方法区的主要职责是用于存放类型的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。只要不断在运行时产生大量的类去填满方法区，就会导致溢出。例如使用 JDK 的反射或 CGLib 直接操作字节码在运行时生成大量的类会导致溢出。当前的很多主流框架如Spring、Hibernate等对类增强是都会使用CGLib这类字节码技术，增强的类越多，就需要越大的方法区保证动态生成的新类型可以载入内存，也就更容易导致方法区溢出。

JDK 8 之后永久代完全被废弃，取而代之的是元空间，HotSpot 提供了一些参数作为元空间的防御措施：

`-XX:MaxMetaspaceSize`：设置元空间的最大值，默认 -1，表示不限制即只受限于本地内存大小。

`-XX:MetaspaceSize`：指定元空间的初始大小，以字节为单位，达到该值就会触发垃圾收集进行类型卸载，同时收集器会对该值进行调整：如果释放了大量空间就适当降低该值，如果释放了很少的空间就适当提高该值。

`-XX:MinMetaspaceFreeRatio`：作用是在垃圾收集之后控制最小的元空间剩余容量百分比，可减少因为元空间不足导致的垃圾收集的频率。类似的还有`-XX:MinMetaspaceFreeRatio`，用于控制最大的元空间剩余容量百分比。

**本机直接内存溢出**

直接内存的容量大小可通过 `-XX:MaxDirectMemorySize` 指定，如果不去指定则默认与 Java 堆的最大值一致。

由直接内存导致的内存溢出，一个明显的特征是在 Heap Dump 文件中不会看见有什么明显的异常情况，如果发现内存溢出后产生的Dump 文件很小，而程序中又直接或间接使用了直接内存（典型的间接使用就是 NIO），那么就可以考虑检查直接内存方面的原因。

---

### P6：判断对象是否是垃圾

在堆中存放着所有对象实例，垃圾收集器在对堆进行回收前，首先要判断对象是否还存活着。

**引用计数算法**

在对象中添加一个引用计数器，如果有一个地方引用它计数器就加1，引用失效时计数器就减1，如果计数器为0则该对象就是不再被使用的。该算法原理简单，效率也高，但是在 Java中很少使用，因为它存在对象之间互相循环引用的问题，导致计数器无法清零。

**可达性分析算法**

当前主流语言的内存管理子系统都是使用可达性分析算法来判断对象是否存活的。这个算法的基本思路就是通过一系列称为 GC Roots 的根对象作为起始节点集，从这些节点开始，根据引用关系向下搜索，搜索过程所走过的路径称为引用链，如果某个对象到GC Roots之间没有任何引用链相连，则此对象是不可能再被使用的。

可作为GC Roots的对象：

- 在虚拟机栈中引用的对象，如线程被调用的方法堆栈中的参数、局部变量等。
- 在方法区中类静态属性引用的对象，如类的引用类型静态变量。
- 在方法区中常量引用的对象，如字符串常量池中的引用。
- 在本地方法栈中 JNI 即 Native 方法引用的对象。
- JVM 内部的引用，如基本数据类型对应的 Class 对象，一些常驻异常对象，系统类加载器等。
- 所有被 synchronized 同步锁持有的对象。

---

### P7：引用类型

无论通过引用计数还是可达性分析判断对象是否存活，都和引用离不开关系。在 JDK1.2 之前引用的定义是：如果 reference 类型数据存储的数值代表另外一块内存的起始地址，那么就称该 reference 数据是代表某块内存、某个对象的引用。在 JDK 1.2之后 Java 对引用的概念进行了扩充，按强度分为四种：

**强引用**：最传统的引用定义，指代码中普遍存在的引用赋值。任何情况下只要强引用存在，垃圾收集器就永远不会回收被引用的对象。

**软引用**：描述一些还有用但非必需的对象。只被软引用关联的对象，在系统将要发生内存溢出异常前，会把这些对象列进回收范围中进行二次回收，如果这次回收还没有足够的内存才会抛出 OOM 异常。

**弱引用**：描述非必需对象，引用强度比软引用更弱，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器开始工作时无论当前内存是否足够都会回收只被弱引用关联的对象。

**虚引用**：也称幽灵引用或幻影引用，是最弱的引用关系。一个对象是否有虚引用存在，完全不会对其生存时间造成影响，也无法通过虚引用来取得一个对象实例。该引用的唯一目的就是为了能在这个对象被垃圾收集器回收时收到一个系统通知。

---

### <font color="red">P8：GC 算法</font>

**标记-清除算法**

- **原理：**分为标记和清除两个阶段，首先标记出所有需要回收的对象，在标记完成之后统一回收掉所有被标记的对象，或者标记存活的对象并统一回收所有未被标记的对象。标记过程就是判断对象是否属于垃圾的过程。

- **特点：**① 执行效率不稳定，如果堆中包含大量对象且其中大部分是需要被回收的，这时必须进行大量标记和清除，导致效率随对象数量增长而降低。② 内存空间碎片化问题，标记、清除之后会产生大量不连续的内存碎片，空间碎片太多可能会导致以后在程序运行中需要分配较大对象时无法找出足够的连续内存而不得不提前触发另一次垃圾收集。

**标记-复制算法**

- **原理：**将可用内存按容量划分为大小相等的两块，每次只使用其中一块。当这一块的空间用完了，就将还存活着的对象复制到另一块，然后再把已使用过的内存空间一次清理掉。

- **特点：**① 实现简单、运行高效，解决了内存碎片问题。② 代价是将可用内存缩小为原来的一半，浪费了过多空间。

- **HotSpot 的新生代划分：**

  把新生代划分为一块较大的 Eden 空间和两块较小的 Survivor 空间，每次分配内存只使用 Eden 和其中一块 Survivor。发生垃圾收集时将 Eden 和 Survivor 中仍然存活的对象一次性复制到另一块 Survivor 上，然后直接清理掉 Eden 和已用过的那块 Survivor 空间。HotSpot虚拟机默认Eden和Survivor的大小比例是8：1，即每次新生代中可用空间为整个新生代的90%。

**标记-整理算法**

- **原理：**标记-复制算法在对象存活率较高时要进行较多的复制操作，效率将会降低。并且如果不想浪费空间，就需要有额外空间进行分配担保，应对被使用内存中所有对象都100%存活的极端情况，所以老年代一般不使用此算法。老年代使用标记-整理算法，标记过程与标记-清除算法一样，只是后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向内存空间一端移动，然后直接清理掉边界以外的内存。
- **特点：**标记-清除与标记-整理的本质差异在于前者是一种非移动式回收算法而后者是移动式的。是否移动回收后的存活对象是一项优缺点并存的风险策略：① 如果移动存活对象，尤其是在老年代这种每次回收都有大量对象存活的区域，是一种极为负重的操作，而且这种移动必须全程暂停用户线程才能进行。② 如果不移动对象就会导致空间碎片问题，只能依赖更复杂的内存分配器和内存访问器来解决。所以是否移动对象都存在弊端，移动则内存回收时更复杂，不移动则内存分配时更复杂。

---

### P9：垃圾收集器

**经典垃圾收集器：**指 JDK 11之前的全部可用垃圾收集器。

**Serial**

最基础、历史最悠久的收集器，该收集器是一个使用复制算法的单线程工作收集器，单线程的意义不仅是说明它只会使用一个处理器或一条收集线程去完成垃圾收集工作，更重要的是强调它进行垃圾收集时必须暂停其他所有工作线程直到收集结束。

Serial 是虚拟机运行在客户端模式下的默认新生代收集器，优点是简单高效，对于内存受限的环境它是所有收集器中最小的；对于单核处理器或处理器核心较少的环境来说，Serial 收集器由于没有线程交互开销，因此可获得最高的单线程收集效率。

**ParNew**

实质上是 Serial 的多线程版本，除了使用多线程进行垃圾收集外其余行为完全一致。

ParNew 是虚拟机运行在服务端模式下的默认新生代收集器，一个重要原因是除了 Serial 外只有它能与 CMS 配合。自从 JDK 9 开始，ParNew 加 CMS 收集器的组合就不再是官方推荐的服务端模式下的收集器解决方案了，官方希望他能被 G1 完全取代。

**Parallel Scavenge**

新生代收集器，基于标记-复制算法，是可以并行的多线程收集器，与 ParNew 类似。

特点是它的关注点与其他收集器不同，CMS 等收集器的关注点是尽可能缩短收集时用户线程的停顿时间，而 Parallel Scavenge 的目标是达到一个可控制的吞吐量，吞吐量就是处理器用于运行用户代码的时间与处理器消耗总时间的比值。自适应调节策略也是它区别于 ParNew 的一个重要特性。

**Serial Old**

Serial 的老年代版本，同样是一个单线程收集器，使用标记-整理算法。

Serial Old 是虚拟机在客户端模式下的默认老年代收集器，用于服务端有两种用途：一种是 JDK 5 及之前与 Parallel Scavenge 搭配使用，另一种是作为CMS 发生失败时的预案。

**Parellel Old**

Parallel Scavenge 的老年代版本，支持多线程收集，基于标记-整理算法实现。这个收集器直到 JDK 6 才开始提供，在注重吞吐量优先的场景可以有效考虑Parallel Scavenge 加 Parallel Old 组合。

**CMS**

以获取最短回收停顿时间为目标的收集器，如果希望系统停顿时间尽可能短以给用户带来更好的体验就可以使用 CMS。

基于标记-清除算法，过程相对复杂，分为四个步骤：初始标记、并发标记、重新标记、并发清除。

其中初始标记和重新标记仍然需要 STW（Stop The World，表示系统停顿），初始标记仅是标记 GC Roots 能直接关联到的对象，速度很快。并发标记就是从 GC Roots 的直接关联对象开始遍历整个对象图的过程，耗时较长但不需要停顿用户线程，可以与垃圾收集线程并发运行。重新标记则是为了修正并发标记期间因用户程序运作而导致标记产生变动的那一部分对象的标记记录，该阶段停顿时间比初始标记稍长，但远比并发标记短。最后是并发清除，清理标记阶段判断的已死亡对象，由于不需要移动存活对象，因此该阶段也可以与用户线程并发。

由于整个过程中耗时最长的并发标记和并发清除阶段中，垃圾收集器都可以和用户线程一起工作，所以从总体上说CMS 的内存回收过程是与用户线程并发执行的。

CMS 是 HotSpot 追求低停顿的第一次成功尝试，但还存在三个明显缺点：① 对处理器资源非常敏感，在并发阶段虽然不会导致用户线程暂停，但会降低总吞吐量。② 无法处理浮动垃圾，有可能出现并发失败而导致另一次 FullGC。③ 由于基于标记-清除算法，因此会产生大量空间碎片，给大对象分配带来麻烦。

**G1**

开创了收集器面向局部收集的设计思路和基于Region的内存布局，是一款主要面向服务端的收集器，最初设计目标是替换CMS。

G1 之前的收集器，垃圾收集的目标要么是整个新生代，要么是整个老年代或整个堆。而 G1 可以面向堆内存任何部分来组成回收集进行回收，衡量标准不再是它属于哪个分代，而是哪块内存中存放的垃圾数量最多，回收受益最大，这就是 G1 的 MixedGC 模式。

不再坚持固定大小及数量的分代区域划分，而是把 Java 堆划分为多个大小相等的独立区域（Region），每一个 Region 都可以根据需要扮演新生代的 Eden 空间、Survivor 空间或老年代空间。收集器能够对扮演不同角色的 Region 采用不同的策略处理，这样无论是新创建的对象还是已经存活了一段时间、熬过多次收集的旧对象都能获取很好的收集效果。

跟踪各个 Region 里面的垃圾堆积的价值大小，价值即回收所获得的空间大小以及回收所需时间的经验值，在后台维护一个优先级列表，每次根据用户设定允许的收集停顿时间优先处理回收价值收益最大的 Region。这种回收方式保证了 G1 在有限的时间内获取尽可能高的收集效率。

G1的运作过程：

- **初始标记：**标记 GC Roots 能直接关联到的对象并修改 TAMS 指针的值，让下一阶段用户线程并发运行时能正确地在可用 Region 中分配新对象。该阶段需要 STW 但耗时很短，是借用进行 MinorGC 时同步完成的。
- **并发标记：**从 GC Roots 开始对堆中对象进行可达性分析，递归扫描整个堆的对象图，找出需要回收的对象。该阶段耗时长，但可与用户线程并发执行，当对扫描完成后还要重新处理 SATB 记录的在并发时有引用变动的对象。
- **最终标记：**对用户线程做一个短暂暂停，用于处理并发阶段结束后仍遗留下来的少量 SATB 记录。
- **筛选回收：**对各个 Region 的回收价值和成本排序，根据用户期望的停顿时间指定回收计划，可自由选择任意多个 Region 构成回收集然后把决定回收的那一部分的存活对象复制到空的 Region 中，再清理掉整个旧的 Region 的全部空间。该操作必须暂停用户线程，由多条收集器线程并行完成。

可以由用户指定期望的停顿时间是 G1 的一个强大功能，但该值不能设得太低，一般设置为100~300毫秒比较合适。G1不会存在内存空间碎片的问题，但 G1 为了垃圾收集产生的内存占用和程序运行时的额外执行负载都高于CMS。

---

**低延迟垃圾收集器：**指 Shenandoah 和 ZGC，这两个收集器几乎整个工作过程全都是并发的，只有初始标记、最终标记这些阶段有短暂停顿，停顿的时间基本上是固定的。

**Shenandoah**

相比 G1 内存布局同样基于 Region，默认回收策略也是优先处理回收价值最大的 Region。但在管理堆内存方面，与 G1 有不同：① 支持并发整理，G1 的回收阶段不能与用户线程并发。②默认不使用分代收集，不会有专门的新生代 Region 或老年代 Region。③ 摒弃了在 G1 中耗费大量内存和计算资源去维护的记忆集，改用名为连接矩阵的全局数据结构来记录跨 Region 的引用关系。

**ZGC**

JDK11中新加入的具有实验性质的低延迟垃圾收集器，和 Shenandoah 的目标高度相似，都希望在尽可能对吞吐量影响不大的前提下实现在任意堆大小下都可以把垃圾收集器的停顿时间限制在 10ms 以内的低延迟。

基于 Region 内存布局，不设分代，使用了读屏障、染色指针和内存多重映射等技术来实现可并发的标记-整理，以低延迟为首要目标。内存布局也采用基于 Region 的堆内存布局，但不同的是 ZGC 的 Region 具有动态性，是动态创建和销毁的，并且区容量大小也是动态变化的。

---

### P10：内存分配与回收策略

以 Seial + Serial Old 客户端默认收集器组合为例：

**对象优先在 Eden 区分配**

大多数情况下对象在新生代 Eden 区分配，当 Eden 区没有足够空间进行分配时虚拟机将发起一次 MinorGC。

可通过 `-XX:Xms` 和 `-XX:Xmx` 设置堆大小， `-Xmn` 设置新生代的大小， `-XX:SurvivorRatio` 设置新生代中 Eden 和 Survivor的比例。

**大对象直接进入老年代**

大对象是指需要大量连续内存空间的对象，最典型的是很长的字符串或者元素数量很庞大的数组。大对象容易导致内存明明还有不少空间时就提前触发垃圾收集以获得足够的连续空间才能安置它们，当复制对象时大对象就意味着高额内存复制开销。

HotSpot 提供了 `-XX:PretenureSizeThreshold` 参数，大于该值的对象直接在老年代分配，避免在 Eden 和 Survivor 之间来回复制产生大量内存复制操作。

**长期存活对象进入老年代**

虚拟机给每一个对象定义了一个对象年龄计数器，存储在对象头。对象通常在 Eden 诞生，如果经历过第一次 MinorGC 仍然存活并且能被 Survivor 容纳，该对象就会被移动到 Survivor 中并将年龄设置为 1。对象在 Survivor 中每熬过一次 MinorGC 年龄就加 1 ，当增加到一定程度（默认15）就会被晋升到老年代。对象晋升老年代的年龄阈值可通过 `-XX:MaxTenuringThreshold` 设置。

**动态对象年龄判定**

为了适应不同程序的内存状况，虚拟机并不永远要求对象年龄达到阈值才能晋升老年代，如果在 Survivor 中相同年龄所有对象大小的总和大于 Survivor 的一半，年龄不小于该年龄的对象就可以直接进入老年代，无需等到 `-XX:MaxTenuringThreshold` 参数设置的年龄。

**空间分配担保**

发生 MinorGC 前，虚拟机必须先检查老年代最大可用连续空间是否大于新生代所有对象总空间，如果这个条件成立，那这一次 MinorGC可以确定是安全的。

如果不成立，虚拟机会先查看 `-XX:HandlePromotionFailure` 参数的值是否允许担保失败，如果允许会继续检查老年代最大可用连续空间是否大于历次晋升老年代对象的平均大小，如果满足将冒险尝试一次 MinorGC，如果不满足或不允许担保失败就会改成一次 FullGC。

之所以说冒险是因为新生代使用复制算法，为了内存利用率只使用其中一个 Survivor 作为备份，因此当出现大量对象在 MinorGC 后仍然存活的情况时需要老年代进行分配担保，把 Survivor 无法容纳的对象直接送入老年代。

---

### P11：故障处理工具

**jps：虚拟机进程状况工具**

jps 即 JVM Process Status，参考了 UNIX 命令的命名格式，功能和 ps 命令类似：可以列出正在运行的虚拟机进程，并显示虚拟机执行主类名称以及这些进程的本地虚拟机唯一 ID（LVMID)。LVMID 与操作系统的进程 ID（PID）是一致的，使用 Windows 的任务管理器或 UNIX 的 ps 命令也可以查询到虚拟机进程的 LVMID，但如果同时启动了多个虚拟机进程，无法根据进程名称定位就必须依赖 jps 命令。

jps 还可以通过 RMI 协议查询开启了 RMI 服务的远程虚拟机进程状态，参数 hostid 为 RMI 注册表中注册的主机名。

**jstat：虚拟机统计信息监视工具**

jstat 即 JVM Statistic Monitoring Tool，是用于监视虚拟机各种运行状态信息的命令行工具。它可以显示本地或者远程虚拟机进程中的类加载、内存、垃圾收集、即时编译器等运行时数据，在没有 GUI 界面的服务器上是运行期定位虚拟机性能问题的常用工具。

一些参数的含义：S0 和 S1 表示两个 Survivor 区，E 表示新生代，O 表示老年代，YGC 表示 Young GC 次数，YGCT 表示Young GC 耗时，FGC 表示 Full GC 次数，FGCT 表示 Full GC 耗时，GCT 表示所有 GC 总耗时。

**jinfo：Java 配置信息工具**

jinfo 表示 Configuration Info for Java，作用是实时查看和调整虚拟机各项参数，使用 jps 的 -v 参数可以查看虚拟机启动时显式指定的参数列表，但如果想知道未被显式指定的参数的系统默认值就只能使用 jinfo 的 -flag 选项进行查询。jinfo 还可以把虚拟机进程的 System.getProperties() 的内容打印出来。

**jmap：Java 内存映像工具**

jmap 表示 Memory Map for Java，jamp 命令用于生成堆转储快照，还可以查询 finalize 执行队列、Java 堆和方法区的详细信息，如空间使用率，当前使用的是哪种收集器等。和 jinfo 命令一样，有部分功能在 Windows 平台下受限，除了生成堆转储快照的 -dump 选项和用于查看每个类实例的 -histo 选项外，其余选项都只能在 Linux 使用。

**jhat：虚拟机堆转储快照分析工具**

jhat 表示 JVM Heap Analysis Tool，JDK 提供 jhat 命令与 jmap 搭配使用来分析 jamp 生成的堆转储快照。jhat 内置了一个微型的 HTTP/Web 服务器，生成堆转储快照的分析结果后可以在浏览器中查看。

**jstack：Java 堆栈跟踪工具**

jstack 表示 Stack Trace for Java，jstack 命令用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的目的通常是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间挂起等。线程出现停顿时通过 jstack 查看各个线程的调用堆栈，就可以获知没有响应的现场到底在后台做什么或等待什么资源。

除了上述的基础故障处理工具，还有一些可视化故障处理工具，例如 JHSDB 基于服务性代理的调试工具、JConsole Java 监视与管理控制台、VisualVM 多合一故障处理工具、Java Mission Control 可持续在线监控工具。

---

### <font color="red">P12：类加载机制和初始化时机</font>

在 Class 文件中描述的各类信息最终都需要加载到虚拟机后才能运行和使用。JVM 把描述类的数据从 Class 文件加载到内存，并对数据进行校验、解析和初始化，最终形成可以被虚拟机直接使用的 Java类型，这个过程被称为虚拟机的类加载机制。与其他在编译时需要连接的语言不同，Java 中类型的加载、连接和初始化都是在程序运行期间完成的，这种策略让 Java 进行类加载时增加了性能开销，但却为 Java 应用提供了极高的扩展性和灵活性，Java 可以动态扩展的语言特性就是依赖运行期动态加载和动态连接这个特点实现的。

一个类型从被加载到虚拟机内存开始，到卸载出内存为止，整个生命周期将会经历加载、验证、准备、解析、初始化、使用和卸载七个阶段，其中验证、解析和初始化三个部分统称为连接。加载、验证、准备、初始化和卸载这五个阶段的顺序是确定的，而解析阶段则不一定：它在某些情况下可以在初始化阶段之后再开始，这是为了支持 Java 语言的动态绑定特性。

关于何时需要开始类加载的第一个阶段"加载"，《 Java 虚拟机规范》没有强制约束，但对于初始化严格规定了有且只有6种情况：

- 遇到 new、getstatic、putstatic 或 invokestatic 这四条字节码指令时，如果类型没有初始化则需要先触发初始化。典型场景有：① 使用new关键字实例化对象。② 读取或设置一个类型的静态字段。③ 调用一个类型的静态方法。

- 对类型进行反射调用时，如果类型没有初始化则需要先触发初始化。

- 当初始化类时，如果其父类没有初始化则需要先触发父类的初始化。

- 当虚拟机启动时，用户需要指定一个要执行的主类即包含 main 方法的类，虚拟机会先初始化该类。

- 当使用 JDK 7 新加入的动态语言支持时，如果 MethodHandle 实例的解析结果为指定类型的方法句柄且这个句柄对应的类没有进行过初始化，则需要先触发其初始化。

- 当一个接口定义了默认方法时，如果该接口的实现类发生初始化，那接口要在其之前初始化。

除了这六种情况外其余所有引用类型的方式都不会触发初始化，称为被动引用。被动引用的实例：① 子类使用父类的静态字段时，只有直接定义这个字段的父类会被初始化。② 通过数组定义使用类。③ 常量在编译期会存入调用类的常量池，不会初始化定义常量的类。

接口的加载过程和类真正有所区别的是当初始化类时，如果其父类没有初始化则需要先触发其父类的初始化，但在一个接口初始化时并不要求其父接口全部完成了初始化，只有在真正使用到父接口时（如引用接口中定义的常量）才会初始化。

----

### <font color="red">P13：类加载过程</font>

**加载**

加载是类加载的第一个阶段，在该阶段虚拟机需要完成三件事：① 通过一个类的全限定类名来获取定义此类的二进制字节流。② 将这个字节流所代表的静态存储结构转化为方法区的运行时数据区结构。③ 在内存中生成一个代表这个类的 Class 对象，作为方法区这个类的各种数据的访问入口。

加载结束后，虚拟机外部的二进制字节流就按照虚拟机所设定的格式存储在方法区中了，方法区中的数据存储格式完全由虚拟机自定义实现。类型数据安置在方法区之后，会在 Java 堆中实例化一个 Class 对象，这个对象将作为程序员访问方法区中类型数据的外部接口。加载与连接的部分动作是交叉进行的，加载尚未完成时连接可能已经开始。

**验证**

验证是连接的第一步，目的是确保 Class 文件的字节流中包含的信息符合约束要求，保证这些信息不会危害虚拟机的安全。Java 语言本身是安全的，但如果虚拟机不检查输入的字节流，对其完全信任的话，很可能因为载入了有错误或有恶意企图的字节码流而导致整个系统受攻击甚至崩溃。

验证主要包含了四个阶段：文件格式验证、元数据验证、字节码验证、符号引用验证。

验证对于虚拟机的类加载机制来说是一个非常重要但非必需的阶段，因为验证只有通过与否的区别，只要通过了验证其后就对程序运行期没有任何影响了。如果程序运行的全部代码都已被反复使用和验证过，在生产环境的就可以考虑关闭大部分类验证措施缩短类加载时间。

**准备**

准备是正式为类变量分配内存并设置零值的阶段，该阶段进行的内存分配仅包括类变量，而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在 Java 堆中。如果变量被final修饰，编译时 Javac 会为变量生成 ConstantValue 属性，那么在准备阶段虚拟机就会将该变量的值设为程序员指定的值。

**解析**

解析是将常量池内的符号引用替换为直接引用的过程。

- **符号引用：**符号引用以一组符号描述引用目标，可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。与虚拟机内存布局无关，引用目标并不一定是已经加载到虚拟机内存中的内容。
- **直接引用：**直接引用是可以直接指向目标的指针、相对偏移量或者能间接定位到目标的句柄。和虚拟机的内存布局直接相关，引用目标必须已在虚拟机的内存中存在。

解析部分主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符这7类符合引用进行。

**初始化**

初始化是类加载过程的最后一步，直到该阶段，JVM 才真正开始执行类中编写的代码。

准备阶段时变量已经赋过一次系统零值，而在初始化阶段会根据程序员的编码去初始化类变量和其他资源。

初始化阶段就是执行类构造器 \<client> 方法的过程，该方法是 Javac 编译器自动生成的。

---

### <font color="red">P14：类加载器和双亲委派模型</font>

类加载阶段中"通过一个类的全限定名来获取描述该类的二进制字节流"的动作被设计为放到 JVM 外部实现，以便让应用程序自己决定如何获取所需的类，实现这个动作的代码就是类加载器。

**比较两个类是否相等：**对于任意一个类，都必须由加载它的类加载器和这个类本身一起共同确立其在虚拟机中的唯一性，每一个类加载器都拥有一个独立的类名称空间。只有在两个类是由同一个类加载器加载的前提下才有意义，否则即使两个类来源于同一个 Class 文件，被同一个 JVM 加载，只要加载它们的类加载器不同，那这两个类就必定不相等。

从 JVM 的角度看只存在两种不同的类加载器：一种是启动类加载器，由 C++ 语言实现，是虚拟机自身的一部分；另一种是其他所有类加载器，由 Java 语言实现，独立存在于虚拟机外部且全部继承自抽象类 `java.lang.ClassLoader`。

自 JDK1.2 起 Java 一直保持着三层类加载器、双亲委派的类加载结构。

- **启动类加载器**：负载加载存放在 `JAVA_HOME/lib` 目录，或者指定路径中存放的能够被虚拟机识别的类库加载到虚拟机内存中。启动类加载器无法被 Java 程序直接引用，如果用户需要把加载请求委派给启动类加载器，直接使用 null 代替即可。
- **扩展类加载器**：负载加载 `JAVA_HOME/lib/ext` 目录，或者系统变量所指定的路径中的类库。这种扩展机制在 JDK 9 后被模块化所取代，由于扩展类加载器是由 Java 编写的，开发者可以直接在程序中使用扩展类加载器来加载 Class 文件。
- **应用程序类加载器**：也称系统类加载器，负载加载用户类路径上的所有类库，同样可以直接在代码中使用。如果应用程序中没有自定义类加载器，一般情况下该类加载器就是程序中默认的类加载器。

**双亲委派模型**

双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应该有自己的父类加载器。不过这里类加载器之间的父子关系一般不是以继承关系来实现的，而通常使用组合关系来复用父加载器的代码。

如果一个类加载器收到了类加载请求，它不会自己去尝试加载这个类，而首先将该请求委派给自己的父加载器完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中，只有当父加载器反馈自己无法完成请求时，子加载器才会尝试自己完成加载。

好处是 Java 中的类跟随它的类加载器一起具备了一种带有优先级的层次关系，可以保证某个类在程序的各个类加载器环境中都是同一个类，对于保证程序的稳定运行极为重要。

---

### P15：Java 程序运行的过程

**通过 Javac 编译器将 .java 代码转为 JVM 可以加载的 .class 字节码文件。**

Javac 编译器是由 Java 语言编写的程序，从 Javac 代码的总体结构看，编译过程可以分为 1 个准备过程和 3 个处理过程：① 准备过程：初始化插入式注解处理器。② 解析与填充符号表过程：进行词法、语法分析，将源代码的字符流转为标记集合，构造出抽象语法树。填充符号表，产生符号地址和符号信息。③ 插入式注解处理器的注解处理过程。④ 分析与字节码生成过程，包括标注检查，对语法的静态信息进行检查；数据流及控制流分析，对程序动态运行过程进行检查；解语法糖，将简化代码编写的语法糖还原为原有的形式；字节码生成，将前面各个步骤的信息转换为字节码。

Javac 属于前端编译器，完成了从程序到抽象语法树或中间字节码的生成，在此之后还有一组内置于 JVM 内部的后端编译器，即即时编译器或提前编译器，来完成代码优化以及从字节码生成本地机器码的过程。 

**通过即时编译器 JIT 把字节码文件编译成本地机器码。**

Java 程序最初都是通过解释器进行解释执行的，当虚拟机发现某个方法或代码块的运行特别频繁，就会把这些代码认定为"热点代码"，热点代码的检测主要有基于采样和基于计数器两种方式，为了提高热点代码的执行效率，在运行时虚拟机会把这些代码编译成本地机器码，并尽可能对代码优化，在运行时完成这个任务的后端编译器被称为即时编译器。

客户端编译器的执行过程：① 平台独立的前端将字节码构造成一种高级中间代码表示 HIR。② 平台相关的后端从 HIR 中产生低级中间代码表示 LIR。③ 在平台相关的后端使用线性扫描算法在 LIR 上分配寄存器，并在 LIR 上做窥孔优化，然后产生机器代码。

服务端编译器专门面向服务端的典型应用场景，并为服务器的性能配置针对性调整过的编译器，也是一个能容忍很高优化复杂度的高级编译器，它会执行大部分经典的优化动作，如无用代码消除、循环表达式外提、消除公共子表达式、基本块重排序等，还会实施一些与 Java 语言特性相关的优化，如范围检查消除、空值检查消除等，也可能根据解释器或客户端编译器提供的性能监控信息进行一些不稳定的预测性激进优化。

**还可以通过静态的提前编译器 AOT 直接把程序编译成与目标机器指令集相关的二进制代码。**

----

## 并发 20

### <font color="red">P1：Java 内存模型</font>

Java 线程的通信由 JMM 控制，JMM 的主要目的是定义程序中各种变量的访问规则，关注在虚拟机中把变量值存储到内存和从内存中取出变量值这样的底层细节。此处的变量包括实例字段、静态字段和构成数组元素的对象，但不包括局部变量与方法参数，因为它们是线程私有的，不存在多线程竞争问题。为了获得更好的执行效率，JMM 没有限制执行引擎使用处理器的特定寄存器或缓存来和主内存进行交互，也没有限制即时编译器是否要进行调整代码执行顺序这类优化措施，JMM 遵循一个基本原则：只要不改变程序执行结果，编译器和处理器怎么优化都行。例如编译器分析某个锁只会单线程访问就消除该锁，某个 volatile 变量只会单线程访问就把它当作普通变量。

JMM 规定了所有变量都存储在主内存中，每条线程还有自己的工作内存，工作内存中保存了被该线程使用的变量的主内存副本，线程对变量的所有操作都必须在工作空间中进行，而不能直接读写主内存中的数据。不同线程之间也无法直接访问对方工作内存中的变量，两个线程之间的通信必须经过主内存，JMM 通过控制主内存与每个线程的工作内存之间的交互来提供内存可见性保证。

关于主内存与工作内存之间的交互，即一个变量如何从主内存拷贝到工作内存、如何从工作内存同步回主内存等实现细节，JMM 定义了 8 种原子操作：

- **lock：**作用于主内存变量，把变量标识为一条线程独占的状态。
- **unlock：**作用于主内存变量，把处于锁定状态的变量释放出来，释放后的变量才能被其他线程锁定。
- **read：**作用于主内存变量，把变量值从主内存传到工作内存。
- **load：**作用于工作内存变量，把 read 从主存中得到的值放入工作内存的变量副本。
- **use：**作用于工作内存变量，把工作内存中的变量值传给执行引擎，每当虚拟机遇到需要使用变量值的字节码指令时执行该操作。
- **assign：**作用于工作内存变量，把从执行引擎接收的值赋给工作内存变量，每当虚拟机遇到给变量赋值的字节码指令时执行该操作。
- **store：**作用于工作内存变量，把工作内存中的变量值传送到主内存。
- **write：**作用于主内存变量，把 store 从工作内存取到的变量值放入主内存变量中。

如果要把一个变量从主内存拷贝到工作内存，就要按顺序执行 read 和 load ，如果要把变量从工作内存同步回主内存，就要按顺序执行 store 和 write 。JMM 只要求这两种操作必须按顺序执行，但不要求连续，也就是说 read 和 load、store 和 write 之间可插入其他指令。这种定义十分严谨但过于复杂，之后 Java 将内存操作简化为 lock、unlock、read 和 write 四种，但这只是语言描述上的等价化简。

---

### P2：as-if-serial 和 happens-before 规则

**as-if-serial**

as-if-serial 的语义是：不管怎么重排序，单线程程序的执行结果不能被改变，编译器和处理器必须遵循 as-if-serial 语义。

为了遵循 as-if-serial 语义，编译器和处理器不会对存在数据依赖关系的操作重排序，因为这种重排序会改变执行结果。但是如果操作之间不存在数据依赖关系，这些操作就可能被编译器和处理器重排序。

as-if-serial 语义把单线程程序保护了起来，给了程序员一种幻觉：单线程程序是按程序的顺序执行的，使程序员无需担心重排序会干扰他们，也无需担心内存可见性问题。

**happens-before**

先行发生原则，是 JMM 中定义的两项操作之间的偏序关系，它是判断数据是否存在竞争，线程是否安全的重要手段。

JMM 将 happens-before 要求禁止的重排序按是否会改变程序执行结果分为两类。对于会改变结果的重排序 JMM 要求编译器和处理器必须禁止这种重排序，对于不会改变结果的重排序，JMM 对编译器和处理器不做要求。 

JMM 存在一些天然的 happens-before 关系，无需任何同步器协助就已经存在。如果两个操作的关系不在此列，并且无法从这些规则推导出来，它们就没有顺序性保障，虚拟机可以对它们随意进行重排序。

- **程序次序规则：**在一个线程内，按照控制流顺序，书写在前面的操作先行发生于书写在后面的操作。
- **管程锁定规则：**一个 unlock 操作先行发生于后面对同一个锁的 lock 操作。
- **volatile 规则：**对一个 volatile 变量的写操作先行发生于后面对这个变量的读操作。
- **线程启动规则：**线程对象的 start 方法先行发生于此线程的每一个动作。
- **线程终止规则：**线程中的所有操作都先行发生于对此线程的终止检测。
- **线程中断规则：**对线程 interrupt 方法的调用先行发生于被中断线程的代码检测到中断事件的发生。
- **对象终结规则：**一个对象的初始化完成先行发生于它的 finalize 方法的开始。
- **传递性：**如果操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C 。

**区别**

as-if-serial 保证单线程程序的执行结果不被改变，happens-before 保证正确同步的多线程程序的执行结果不被改变。这两种语义的目的都是为了在不改变程序执行结果的前提下尽可能提高程序执行的并行度。

---

### P3：指令重排序

重排序指从源代码到指令序列的重排序，在执行程序时为了提高性能，编译器和处理器通常会对指令进行重排序，分为三种：

- 编译器优化的重排序：编译器在不改变单线程程序语义的前提下可以重排语句的执行顺序。

- 指令级并行的重排序：如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。

- 内存系统的重排序：由于处理器使用缓存和读/写缓冲区，这使得加载和存储操作操作看上去可能是乱序执行。

从 Java 源代码到最终实际执行的指令序列，会分别经历编译器优化重排序、指令级并行重排序和内存系统重排序，这些重排序可能会导致多线程程序出现内存可见性问题。

对于编译器，JMM 的编译器重排序规则会禁止特定类型的编译器重排序。对于处理器重排序，JMM 的处理器重排序规则会要求 Java 编译器在生成指令序列时，插入特定类型的内存屏障指令，即一组用于实现对内存操作顺序限制的处理器指令，通过内存屏障指令来禁止特定类型的处理器重排序。JMM 属于语言级的内存模型，它确保在不同的编译器和处理器平台上，通过禁止特定类型的编译器重排序和处理器重排序，为程序员提供一致的内存可见性保证。

---

### P4：原子性、可见性和有序性

**原子性**

由 JMM 直接保证的原子性变量操作包括 read、load、assign、use、store 和 write，基本数据类型的访问都是具备原子性的，例外就是 long 和 double 的非原子性协定，允许虚拟机将没有被 volatile 修饰的 64 位数据的操作划分为两次 32 位的操作。

如果应用场景需要更大范围的原子性保证，JMM 还提供了 lock 和 unlock 操作满足这种需求，尽管 JVM 没有把这两种操作直接开放给用户使用，但是却提供了更高层次的字节码指令 monitorenter 和 monitorexit 来隐式地使用这两个操作，这两个字节码指令反映到 Java 代码中就是 synchronized 关键字。

**可见性**

可见性就是指当一个线程修改了共享变量的值时，其他线程能够立即得知修改。JMM 通过在变量修改后将值同步回主内存，在变量读取前从主内存刷新变量值这种依赖主内存作为传递媒介的方式实现可见性，无论是普通变量还是volatile变量都是如此，区别是 volatile 保证新值能立即同步到主内存以及每次使用前立即从主内存刷新，因此说 volatile 保证了多线程操作变量的可见性，而普通变量则不能保证。

除了 volatile 之外，还有两个关键字能实现可见性，分别是 synchronized 和 final，同步块的可见性是由"对一个变量执行unlock 前必须先把此变量同步回主内存中，即先执行 store 和 write"这条规则获得的。final 的可见性是指：被 final 修饰的字段在构造器中一旦被初始化完成，并且构造器没有把"this"引用传递出去，那么其他线程就能看到 final 字段的值。

**有序性**

有序性可以总结为：在本线程内观察所有操作是有序的，在一个线程内观察另一个线程，所有操作都是无序的。前半句是指"as-if-serial 语义"，后半句是指"指令重排序"和"工作内存与主内存同步延迟"现象。

Java 提供了 volatile 和 synchronized 保证线程间操作的有序性，volatile 本身就包含了禁止指令重排序的语义，而 synchronized 则是由"一个变量在同一个时刻只允许一条线程对其进行lock操作"这条规则获得的，该规则决定了持有同一个锁的两个同步块只能串行进入。

---

### <font color="red">P5：volatile 关键字</font>

JVM 提供的最轻量级的同步机制，JMM 为 volatile 定义了一些特殊的访问规则，当一个变量被定义为 volatile 后具备两种特性：

- **保证此变量对所有线程的可见性**

  可见性是指当一条线程修改了这个变量的值，新值对于其他线程来说是立即可以得知的。而普通变量并不能做到这一点，普通变量的值在线程间传递时均需要通过主内存来完成。

  volatile 变量在各个线程的工作内存中不存在一致性问题，但 Java 的运算操作符并非原子操作，这导致 volatile 变量运算在并发下仍是不安全的。

- **禁止指令重排序优化**

  使用 volatile 变量进行写操作，汇编指令操作是带有 lock 前缀的，相当于一个内存屏障，后面的指令不能重排到内存屏障之前的位置。只有一个处理器时不需要使用内存屏障，但如果有两个或更多的处理器访问同一块内存，且其中有一个在观测另一个，就需要使用内存屏障来保证一致性了。

  使用 lock 前缀的指令在多核处理器中会引发两件事：① 将当前处理器缓存行的数据写回到系统内存。② 这个写回内存的操作会使其他在CPU里缓存了该内存地址的数据无效。

  这种操作相当于对缓存中的变量做了一次 store 和 write 操作，可以让 volatile 变量的修改对其他处理器立即可见。

---

**静态变量 i 执行多线程 i++ 的不安全问题**

通过反编译会发现一个自增语句是由 4 条字节码指令构成的，依次为getstatic、iconst_1、iadd、putstatic，当getstatic把 i 的值取到操作栈顶时，volatile保证了 i 的值在此刻是正确的，但是在执行iconst_1、iadd这些指令时，其他线程可能已经改变了i的值，而操作栈顶的值就变成了过期的数据，所以 putstatic 指令执行后就可能把较小的 i 值同步回了主内存。 

即使编译出来只有一条字节码指令也不能意味着这条指令就是一个原子操作，一条字节码指令在解释执行时，解释器要运行很多行代码才能实现它的语义。如果是编译执行，一条字节码指令也可能转化成若干条本地机器码指令。

---

**适用场景**

运算结果并不依赖变量的当前值，或者能够确保只有单一的线程修改变量的值。

变量不需要与其他状态变量共同参与不变约束。

---

**volatile的内存语义**

从内存语义角度来说，volatile的写-读与锁的释放-获取具有相同的内存效果。

- **写内存语义：**当写一个volatile变量时，JMM 会把该线程对应的本地内存中的共享变量值刷新到主内存。

- **读内存语义：**当读一个volatile变量时，JMM 会把该线程对应的本地内存置为无效，线程接下来将从主内存中读取共享变量。

---

**volatile指令重排序的特点**

当第二个操作是volatile 写时，不管第一个操作是什么都不能重排序，确保写之前的操作不会被编译器重排序到写之后。

当第一个操作是volatile 读时，不管第二个操作是什么都不能重排序，确保读之后的操作不会被编译器重排序到读之前。

当第一个操作是volatile 写，第二个操作是 volatile 读时不能重排序。

---

**JSR-133 增强 volatile 语义的原因**

在旧的内存模型中，虽然不允许 volatile 变量之间重排序，但允许 volatile 变量与普通变量重排序，可能导致内存不可见问题。为了提供一种比锁更轻量级的线程通信机制，严格限制了编译器和处理器对 volatile 变量与普通变量的重排序，确保 volatile 的写-读和锁的释放-获取具有相同的内存语义。

---

### <font color="red">P6：final 关键字</font>

final 可以保证可见性，被 final 修饰的字段在构造器中一旦被初始化完成，并且构造器没有把 this 的引用传递出去，那么在其他线程中就能看见 final 字段的值。

**JSR-133 增强 final语义的原因**

在旧的 JMM 中，一个严重的缺陷就是线程可能看到 final 域的值会改变。比如一个线程看到一个 int 类型 final 域的值为0，此时该值是还未初始化之前的零值，过一段时间之后该值被某线程初始化后这个线程再去读这个 final 域的值会发现值变为1。

为了修复该漏洞，JSR-133 通过为 final 域增加写和读重排序规则，提供初始化安全保证：只要对象是正确构造的（被构造对象的引用在构造方法中没有逸出），那么不需要使用同步就可以保证任意线程都能看到这个final域在构造方法中被初始化之后的值。

**写 final 域重排序规则**

禁止把 final 域的写重排序到构造方法之外，编译器会在final域的写之后，构造方法的 return之前，插入一个Store Store屏障。该规则可以确保在对象引用为任意线程可见之前，对象的 final 域已经被正确初始化过了，而普通域不具有这个保障。

对于引用类型，增加了约束：在构造方法内对一个 final 引用的对象的成员域的写入，与随后在构造方法外把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。

**读 final 域重排序规则**

在一个线程中，初次读对象引用和初次读该对象包含的final域，JMM 禁止处理器重排序这两个操作。编译器会在读final域操作的前面插入一个Load Load 屏障，该规则可以确保在读一个对象的 final 域之前，一定会先读包含这个 final 域的对象的引用。

---

### <font color="red">P7：synchronized 关键字</font>

最基本的互斥同步手段就是 synchronized 关键字，它是一种块结构的同步语法。synchronized 经过 Javac 编译后，会在同步块的前后分别形成 monitorenter 和 monitorexit 两个字节码指令，这两个字节码指令都需要一个引用类型的参数来指明要锁定和解锁的对象，对于同步普通方法，锁是当前实例对象；对于静态同步方法，锁是当前类的Class对象；对于同步方法块，锁是 synchronized 括号里的对象。

根据《 Java 虚拟机规范》的要求，在执行 monitorenter 指令时，首先要去尝试获取对象的锁。如果这个对象没有被锁定，或者当前线程已经持有了那个对象的锁，那么就把锁的计数器的值增加 1，而在执行 monitorexit 指令时会将锁计数器的值减 1。一旦计数器的值为 0，锁随即就被释放了。如果获取锁对象失败，那当前线程就应该被阻塞等待，直到请求锁定的对象被持有它的线程释放为止。

被 synchronized 修饰的同步块对一条线程来说是可重入的，并且同步块在持有锁的线程执行完毕并释放锁之前，会无条件地阻塞后面其他线程的进入。从执行成本的角度看，持有锁是一个重量级的操作。在主流 JVM 实现中，Java 的线程是映射到操作系统的原生内核线程之上的，如果要阻塞或唤醒一条线程，则需要操作系统帮忙完成，这就不可避免陷入用户态到核心态的转换中，进行这些状态转换需要耗费很多的处理器时间。

每个 Java 对象都有一个关联的 monitor 监视器，当这个对象由同步块或同步方法调用时，执行方法的线程必须先获取到对象的监视器才能进入同步块或同步方法。例如有两个线程 A 和 B 竞争锁资源对应的 monitor，当线程 A 竞争到锁时，会将 monitor 中的 owner 设置为 A，把线程 B 阻塞并放到等待竞争资源的 ContentionList 队列。ContentionList 中的部分线程会进入 EntryList，EntryList 中的线程会被指定为 OnDeck 竞争候选者线程，如果获得了锁资源将进入 Owner 状态，释放锁资源后进入 !Owner 状态。被阻塞的线程会进入 WaitSet。

**不公平的原因**

所有收到锁请求的线程首先自旋，如果通过自旋也没有获取锁资源将被放入 ContentionList 队列，该做法对于已经进入队列的线程是不公平的。

为了防止 ContentionList 尾部的元素被大量线程进行 CAS 访问影响性能，Owner 线程会在释放锁时将 ContentionList 的部分线程移动到 EntryList 并指定某个线程为 OnDeck 线程，Owner 并没有将锁直接传递给 OnDeck 线程而是把锁竞争的权利交给它，该行为叫做竞争切换，牺牲了公平性但提高了性能。

---

### <font color="red">P8：锁优化</font>

JDK 6 对 synchronized 做了很多优化，引入了适应自旋、锁消除、锁粗化、偏向锁和轻量级锁等提高锁的效率，锁一共有 4 个状态，级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态，这几个状态会随着竞争情况逐渐升级。锁可以升级但不能降级，这种只能升级不能降级的锁策略是为了提高获得锁和释放锁的效率。

**自旋锁与自适应自旋**

互斥同步对性能最大的影响是阻塞的实现，挂起线程和恢复线程的操作都需要转入内核态中完成，这些操作给 JVM 的并发性能带来了很大压力。同时虚拟机开发团队也注意到了在许多应用上，共享数据的锁定状态只会持续很短的一段时间，为了这段时间去挂机和恢复线程并不值得。现在绝大多数的个人电脑和服务器都是多核心处理器系统，如果物理机器有一个以上的处理器或者处理器核心，能让两个或以上的线程同时并行执行，我们就可以让后面请求锁的那个线程稍等一会，但不放弃处理器的执行时间，看看持有锁的线程是否很快就会释放锁。为了让线程等待，我们只需让线程执行一个忙循环，这项技术就是所谓的自旋锁。

自旋锁在 JDK 1.4中就已经引入，只不过默认是关闭的，在 JDK 6中就已经改为默认开启了。自旋等待不能代替阻塞，自旋等待本身虽然避免了线程切换的开销，但它要占用处理器时间，所以如果锁被占用的时间很短，自旋的效果就会非常好，反之只会白白消耗处理器资源。因此自旋的时间必须有一定的限度，如果自旋超过了限定的次数仍然没有成功获得锁，就应当使用传统的方式去挂起线程。自旋次数的默认次数是 10 次。

在 JDK 6 中对自旋锁的优化，引入了自适应自旋。自旋的时间不再是固定的了，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定的。如果在同一个锁对象上，自旋等待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也很有可能再次成功，进而允许自旋等待持续相对更长的时间。如果对于某个锁，自旋很少成功获得过锁，那在以后要获取这个锁时将有可能之间省略掉自旋过程，以避免浪费处理器资源。有了自适应自旋，随着程序运行时间的增长以及性能监控信息的不断完善，虚拟机对程序锁的状况预测就会越来越精准。

**锁消除**

锁消除是指虚拟机即时编译器在运行时，对一些代码要求同步，但是对被检测到不可能存在共享数据竞争的锁进行消除。锁消除的主要判定依据来源于逃逸分析的数据支持，如果判断到一段代码中，在堆上的所有数据都不会逃逸出去被其他线程访问到，那就可以把它们当作栈上的数据对待，认为它们是线程私有的，同步加锁自然就无须再进行。

**锁粗化**

原则上我们在编写代码时，总是推荐将同步块的作用范围限制得尽量小，只在共享数据得实际作用域中才进行同步，这样是为了使得需要同步的操作数量尽可能变少，即使存在锁竞争，等待锁得线程也能尽可能快拿到锁。

大多数情况下这种原则是正确的，但是如果一系列的连续操作都对同一个对象反复加锁和解锁，甚至加锁操作是出现在循环体之外的，那么即使没有线程竞争，频繁地进行互斥同步操作也会导致不必要的性能消耗。

如果虚拟机探测到有一串零碎的操作都对同一个对象加锁，将会把加锁同步的范围扩展（粗化）到整个操作序列的外部。

**偏向锁**

它的目的是消除数据在无竞争情况下的同步原语，进一步提高程序的运行性能。如果说轻量级锁是在无竞争的情况下使用 CAS 操作去消除同步使用的互斥量，那偏向锁就是在无竞争的情况下把整个同步都去掉，连 CAS 操作都不去做了。

偏向锁的意思就是这个锁会偏向于第一个获得它的线程，如果在接下来的执行过程中，该锁一直没有被其他线程获取，则持有偏向锁的线程将永远不需要再进行同步。

当一个线程访问同步代码块并获取锁时，会在对象头和帧栈中的锁记录里存储锁偏向的线程 ID，以后该线程再进入和退出同步代码块不需要进行 CAS 操作来加锁和解锁，只需要简单地测试一下对象头的"Mark Word"里是否存储着指向当前线程的偏向锁。如果测试成功表示线程已经获得了锁，如果失败则需要再测试一下"Mark Word"中偏向锁的标识是否设置成了 1 即表示当前使用偏向锁，如果设置了就尝试使用 CAS 将对象头的偏向锁指向当前线程，否则使用 CAS 方式竞争锁。

偏向锁使用了一种等到竞争出现才释放锁的机制，所以当其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁。偏向锁的撤销需要等待全局安全点即此时没有正在执行的字节码，它会首先暂停拥有偏向锁的线程，然后检查持有偏向锁的线程是否活着，如果线程不处于活动状态则将对象头设为无锁状态。如果线程还活着，拥有偏向锁的栈会被执行，遍历偏向对象的锁记录，栈中的锁记录和对象头的"Mark Word"要么重新偏向于其他线程，要么恢复到无锁或者标记对象不适合作为偏向锁，最后唤醒暂停的线程。

**轻量级锁**

轻量级是相对于操作系统互斥量来实现的传统锁而言的，因此传统的锁机制就被称为重量级锁。轻量级锁并不是用来代替重量级锁的，它设计的初衷是在没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能消耗。

在代码即将进入同步块的时候，如果此同步对象没有被锁定，虚拟机首先将在当前线程的栈帧中建立一个名为锁记录的空间，用于存储锁对象目前的Mark Word的拷贝。然后虚拟机将使用 CAS 操作尝试把对象的 Mark Word 更新为指向锁记录的指针，如果这个更新操作成功了，即代表该线程拥有了这个对象的锁，并且锁标志位将转变为"00"，表示此对象处于轻量级锁定状态。

如果这个更新操作失败了，那就意味着至少存在一条线程与当前线程竞争获取该对象的锁。虚拟机首先会检查对象的Mark Word是否指向当前线程的栈帧，如果是则说明当前线程以及拥有了这个对象的锁，直接进入同步块继续执行，否则说明这个锁对象已经被其他线程抢占了。如果出现两条以上的线程争用同一个锁的情况，那轻量级锁就不再有效，必须要膨胀为重量级锁，锁标志的状态变为"10"，此时Mark Word中存储的就是指向重量级锁的指针，后面等待锁的线程也必须进入阻塞状态。

解锁操作也同样是通过 CAS 操作来进行，如果对象的 Mark Word 仍然指向线程的锁记录，那就用 CAS 操作把对象当前的 Mark Word 和线程复制的 Mark Word 替换回来。假如能够替换成功，那整个同步过程就顺利完成了，如果替换失败，则说明有其他线程尝试过获取该锁，就要在释放锁的同时唤醒被挂起的线程。

**偏向锁、轻量级锁和重量级锁的区别**

偏向锁的优点是加锁和解锁不需要额外的消耗，和执行非同步方法相比仅存在纳秒级的差距，缺点是如果线程间存在锁竞争会带来额外锁撤销的消耗，适用于只有一个线程访问同步代码块的场景。

轻量级锁的优点是竞争的线程不会阻塞，提高了程序的响应速度，缺点是如果线程始终得不到锁会自旋消耗CPU，适用于追求响应时间和同步代码块执行非常快的场景。

重量级锁的优点是线程竞争不使用自旋不会消耗CPU，缺点是线程会被阻塞，响应时间很慢，适应于追求吞吐量、同步代码块执行较慢的场景。

---

### P9：Lock 接口

自 JDK 5 起 Java类库提供了 juc 并发包，其中的 Lock 接口成为了一种全新的互斥同步手段。基于Lock 接口，用户能够以非块结构来实现互斥同步，从而摆脱了语言特性的束缚，改为在类库层面去实现同步。

重入锁 ReentrantLock 是 Lock 接口最常见的一种实现，它与 synchronized 一样是可重入的，在基本用法上也很相似，不过它增加了一些高级功能，主要包括以下三项：

- **等待可中断：**是指持有锁的线程长期不释放锁时，正在等待的线程可以选择放弃等待而处理其他事情。可中断特性对处理执行时间非常长的同步块很有帮助。
- **公平锁：**是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁，而非公平锁则不保证这一点，在锁被释放时，任何一个等待锁的线程都有机会获得锁。synchronized中的锁是非公平的，ReentrantLock在默认情况下也是非公平的，但可以通过带有布尔值的构造器要求使用公平锁。不过一旦使用了公平锁，将会导致性能急剧下降，明显影响吞吐量。
- **锁绑定多个条件：**是指一个 ReentrantLock 对象可以同时绑定多个 Condition 对象。在 synchronized中，锁对象的 wait 跟它的notify/notifyAll 方法配合可以实现一个隐含的条件，如果要和多于一个的条件关联时就不得不额外添加一个锁，而 ReentrantLock 可以多次调用 newCondition 方法。

一般优先考虑使用synchronized：① synchronized 是 Java 语法层面的同步，足够清晰和简单。② Lock 应该确保在 finally 中释放锁，否则一旦受同步保护的代码块中抛出异常，则有可能永远不会释放持有的锁。这一点必须由程序员自己来保证，而使用 synchronized 可以由 JVM 来确保即使出现异常锁也能被正常释放。③ 尽管在 JDK 5 时ReentrantLock 的性能领先于 synchronized，但在 JDK 6 进行锁优化之后，二者的性能基本能够持平。从长远来看 JVM 更容易针对synchronized进行优化，因为 JVM 可以在线程和对象的元数据中记录 synchronized 中锁的相关信息，而使用Lock的话 JVM 很难得知具体哪些锁对象是由特定线程持有的。

**ReentrantLock 的可重入实现**

以非公平锁为例，通过 nonfairTryAcquire 方法获取锁，该方法增加了再次获取同步状态的处理逻辑：通过判断当前线程是否为获取锁的线程来决定获取操作是否成功，如果是获取锁的线程再次请求则将同步状态值进行增加并返回 true，表示获取同步状态成功。

成功获取锁的线程再次获取锁，只是增加了同步状态值，这就要求 ReentrantLock 在释放同步状态时减少同步状态值。如果该锁被获取了 n 次，那么前 n-1 次 tryRelease 方法必须都返回fasle，只有同步状态完全释放了才能返回 true，该方法将同步状态是否为 0 作为最终释放的条件，当同步状态为 0 时，将占有线程设置为null，并返回 true 表示释放成功。

对于非公平锁只要 CAS 设置同步状态成功则表示当前线程获取了锁，而公平锁则不同。公平锁使用 tryAcquire 方法，该方法与nonfairTryAcquire 的唯一区别就是判断条件中多了对同步队列中当前节点是否有前驱节点的判断，如果该方法返回 true 表示有线程比当前线程更早地请求获取锁，因此需要等待前驱线程获取并释放锁之后才能继续获取锁。、

---

### P10：读写锁

ReentrantLock 是排他锁，在同一时刻只允许一个线程进行访问，而读写锁在同一时刻可以允许多个读线程访问，但是在写线程访问时，所有的读线程和其他写线程均被阻塞。读写锁维护了一个读锁和一个写锁，通过分离读写锁使并发性相比一般的排他锁有了很大提升。

除了保证写操作对读操作的可见性以及并发性的提升之外，读写锁能够简化读写交互场景的编程方式。只需要在读操作时获取读锁，写操作时获取写锁即可，当写锁被获取时后续的读写操作都会被阻塞，写锁释放之后所有操作继续执行，编程方式相对于使用等待/通知机制的实现方式变得简单。

读写锁同样依赖自定义同步器来实现同步功能，而读写状态就是其同步器的同步状态。读写锁的自定义同步器需要在同步状态即一个整形变量上维护多个读线程和一个写线程的状态。如果在一个 int 型变量上维护多种状态，就一定要按位切割使用这个变量，读写锁将变量切分成了两个部分，高 16 位表示读，低 16 位表示写。

写锁是一个支持重入的排他锁，如果当前线程已经获得了写锁则增加写状态，如果当前线程在获取写锁时，读锁已经被获取或者该线程不是已经获得写锁的线程则当前线程进入等待状态。写锁的释放与 ReentrantLock 的释放过程类似，每次释放均减少写状态，当写状态为 0时表示写锁已被释放，从而等待的读写线程能够继续访问读写锁，同时前次写线程的修改对后续读写线程可见。

读锁是一个支持重入的共享锁，它能够被多个线程同时获取，在没有其他写线程访问时，读锁总会被成功地获取，而所做的只是线程安全地增加读状态。如果当前线程已经获取了读锁，则增加读状态。如果当前线程在获取读锁时，写锁已被其他线程获取则进入等待状态。读锁的每次释放均会减少读状态，减少的值是（1<<16），读锁的每次释放是线程安全的。

锁降级指的是写锁降级成为读锁，如果当前线程拥有写锁，然后将其释放，最后再获取读锁，这种分段完成的过程不能称之为锁降级。锁降级指的是把持住当前拥有的写锁，再获取到读锁，随后释放先前拥有的写锁的过程。

锁降级中读锁的获取是必要的，主要是为了保证数据的可见性，如果当前线程不获取读锁而是直接释放写锁，假设此刻另一个线程 A 获取了写锁修改了数据，那么当前线程是无法感知线程 A 的数据更新的。如果当前线程获取读锁，即遵循锁降级的步骤，线程 A 将会被阻塞，直到当前线程使用数据并释放读锁之后，线程 A 才能获取写锁进行数据更新。

---

### P11：AQS 队列同步器 

队列同步器是用来构建锁或者其他同步组件的基础框架，它使用了一个 int 类型的成员变量表示同步状态，通过内置的 FIFO 队列来完成资源获取线程的排队工作。

**使用方式**

同步器的主要使用方式是继承，子类通过继承同步器并实现它的抽象方法来管理同步状态，在抽象方法的实现过程中免不了要对同步状态进行更改，这时就需要使用同步器提供的3个方法 getState、setState 和 compareAndSetState 来进行操作，因为它们能够保证状态的改变是安全的。子类推荐被定义为自定义同步组件的静态内部类，同步器自身没有实现任何同步接口，它仅仅是定义了若干同步状态获取和释放的方法来供自定义同步组件使用，同步器既可以支持独占式地获取同步状态，也可以支持共享式地获取同步状态，这样就可以方便实现不同类型地同步组件。

**和锁的关系**

同步器是实现锁的关键，在锁的实现中聚合同步器，利用同步器实现锁的语义。锁是面向使用者的，它定义了使用者与锁交互的接口，隐藏了实现细节；同步器面向的是锁的实现者，它简化了锁的实现方式，屏蔽了同步状态管理、线程的排队、等待与唤醒等底层操作。锁和同步器很好地隔离了使用者和实现者所关注的领域。

**同步队列**

AQS 中每当有新的线程请求资源时，该线程都会进入一个等待队列，只有当持有锁的线程释放锁资源后该线程才能持有资源。等待队列通过双向链表实现，线程会被封装在链表的 Node 节点中，Node 的等待状态包括：CANCELLED 表示线程已取消、SIGNAL 表示线程需要唤醒、CONDITION 表示线程正在等待、PROPAGATE 表示后继节点会传播唤醒操作，只会在共享模式下起作用。

**两种模式**

独占模式表示锁会被一个线程占用，其他线程必须等到持有锁的线程释放锁后才能获取到锁继续执行，在同一时间内只能有一个线程获取到这个锁，ReentrantLock 就采用的是独占模式。

共享模式表示多个线程获取同一个锁的时候有可能会成功，ReadLock 就采用的是共享模式。

独占模式通过 acquire 和 release 方法获取和释放锁，共享模式通过 acquireShared 和 releaseShared 方法获取和释放锁。

**独占式的获取和释放流程**

在获取同步状态时，同步器调用 acquire 方法，维护一个同步队列，使用 tryAcquire 方法安全地获取线程同步状态，获取状态失败的线程会构造同步节点并通过 addWaiter 方法被加入到同步队列的尾部，并在队列中进行自旋。之后会调用 acquireQueued 方法使得该节点以死循环的方式获取同步状态，如果获取不到则阻塞节点中的线程，而被阻塞线程的唤醒主要依靠前驱节点的出队或阻塞节点被中断实现，移出队列或停止自旋的条件是前驱节点是头结点并且成功获取了同步状态。

在释放同步状态时，同步器调用 tryRelease 方法释放同步状态，然后调用 unparkSuccessor 方法（该方法使用 LockSupport 唤醒处于等待状态的线程）唤醒头节点的后继节点，进而使后继节点重新尝试获取同步状态。

**只有当前驱节点是头节点时才能够尝试获取同步状态原因**

头节点是成功获取到同步状态的节点，而头节点的线程释放同步状态之后，将会唤醒其后继节点，后继节点的线程被唤醒后需要检查自己的前驱节点是否是头节点。

维护同步队列的FIFO原则，节点和节点在循环检查的过程中基本不相互通信，而是简单地判断自己的前驱是否为头节点，这样就使得节点的释放规则符合FIFO，并且也便于对过早通知的处理（过早通知是指前驱节点不是头结点的线程由于中断而被唤醒）。

**共享式的获取和释放流程**

在获取同步状态时，同步器调用 acquireShared 方法，该方法调用 tryAcquireShared 方法尝试获取同步状态，返回值为 int 类型，当返回值大于等于 0 时表示能够获取到同步状态。因此在共享式获取锁的自旋过程中，成功获取到同步状态并退出自旋的条件就是该方法的返回值大于等于0。

释放同步状态时，调用 releaseShared 方法，释放同步状态后会唤醒后续处于等待状态的节点。对于能够支持多线程同时访问的并发组件，它和独占式的主要区别在于 tryReleaseShared 方法必须确保同步状态安全释放，一般通过循环和 CAS 来保证，因为释放同步状态的操作会同时来自多个线程。

---

### <font color="red">P12：线程</font>

现代操作系统在运行一个程序时会为其创建一个进程，而操作系统调度的最小单位是线程，线程也叫轻量级进程。在一个进程中可以创建多个线程，这些线程都拥有各自的计数器、堆栈和局部变量等属性，并且能够访问共享的内存变量。处理器在这些线程上告诉切换，让使用者感觉到这些线程在同时执行。

**生命周期**

①NEW：初始状态，创建后还没有启动的线程处于这种状态，此时还没有调用 start 方法。②RUNNABLE：Java 线程将操作系统中的就绪和运行两种状态统称为 RUNNABLE，此时线程有可能正在等待操作系统分配CPU时间片，也有可能正在执行③BLOCKED：阻塞状态，阻塞状态与等待状态的区别是阻塞状态在等待一个排它锁，在程序等待进入同步区域时线程将进入这种状态。④WAITING：等待状态，表示线程进入等待状态，处于该状态的线程不会被分配CPU时间片，进入该状态表示当前线程需要等待其他线程通知或中断。会导致线程陷入等待状态的方法：没有设置超时参数的wait 和 join方法、LockSupport的 park 方法。⑤TIME_WAITING：限期等待状态，该状态不同于 WAITING，可以在指定时间内自行返回。会导致线程陷入限期等待状态的方法：设置了超时参数的 wait 和 join 方法、LockSupport 的 parkNanos 和 parkUntil 方法。⑥TERMINATED：终止状态，表示当前线程已经执行完毕或异常退出。

**实现方式**

①继承 Thread 类并重写 run方法。优点是实现简单，缺点是不能继承其他类，功能单一。②实现 Runnable 接口并重写 run 方法，并将该实现类作为参数传入 Thread 构造器。优点是避免了单继承的局限性，适合多个相同程序代码的线程共享一个资源，实现解耦操作，代码和线程独立。③实现 Callable 接口并重写 call 方法，包装成 FutureTask 对象并作为参数传入Thread构造器。优点是可以获取线程执行结果的返回值。④可以通过线程池创建。

**方法**

① wait是Object类的方法，调用wait方法的线程会进入WAITING状态，只有等待其他线程的通知或被中断后才会解除阻塞，调用wait方法会释放锁资源。② sleep 是 Thread 类的方法，调用 sleep 方法会导致当前线程进入休眠状态，与 wait 不同的是该方法不会释放锁资源，进入的是 TIMED-WAITING 状态。③ yiled 方法会使当前线程让出 CPU 时间片给优先级相同或更高的线程，回到 RUNNABLE 状态，与其他线程一起重新竞争CPU时间片。④ join 方法用于等待其他线程运行终止，如果当前线程调用了另一个线程的 join 方法，则当前线程进入阻塞状态，当另一个线程结束时当前线程才能从阻塞状态转为就绪态，等待获取CPU时间片。底层使用的是wait，也会释放锁。

**守护线程**

守护线程是一种支持型线程，因为它主要被用作程序中后台调度以及支持性工作，当 JVM 中不存在非守护线程时，JVM 将会退出，可以通过 setDaemon(true) 将线程设置为daemon线程，但必须在线程启动之前设置。守护线程被用于完成支持性工作，但是在 JVM 退出时守护线程中的 finally 块并不一定会被执行，因为当 JVM 中没有非守护线程时需要立即退出，所有守护线程都将立即终止，因此不能依靠 finally 确保执行关闭或清理资源的逻辑。

---

### P13：线程间通信

通信是指线程之间以何种机制来交换信息，在命令式编程中线程之间的通信机制有两种，共享内存和消息传递。在共享内存的并发模型里线程之间共享程序的公共状态，通过写-读内存中的公共状态进行隐式通信。在消息传递的并发模型里线程之间没有公共状态，线程之间必须通过发送消息来显示通信。Java 并发采用共享内存模型，线程之间的通信总是隐式进行，整个通信过程对程序员完全透明。

**volatile 和 synchronized 关键字**

volatile 可以修饰字段，告知程序任何对该变量的访问均需要从共享内存中获取，而对它的改变必须同步刷新回主内存，它能保证所有线程对变量访问的可见性。

synchronized 可以修饰方法或以同步块的形式使用，它主要确保多个线程在同一个时刻只能有一个线程处于方法或同步块中，保证了线程对变量访问的可见性和排他性。

**等待/通知机制**

等待通知机制是指一个线程 A 调用了对象 O 的 wait 方法进入等待状态，而另一个线程 B 调用了对象 O 的 notify 或 notifyAll 方法，线程 A 收到通知后从 wait 方法返回，进而执行后序操作。两个线程通过对象 O 完成交互，对象上的 wait 和 notify/notifyAll 就如同开关信号，用来完成等待方和通知方之间的交互工作。

**管道 IO 流**

管道 IO 流和普通文件IO 流或网络 IO 流的不同之处在于它主要用于线程之间的数据传输，传输的媒介为内存。管道流主要包括了四种具体实现：PipedOutputStream、PipedInputStream、PipedReader 和 PipedWriter，对于 Piped 类型的流必须要通过 connect 方法进行绑定。

**Thread.join**

如果一个线程执行了某个线程的 join 方法，这个线程就会阻塞等待执行了 join 方法的线程终止之后才返回，这里涉及了等待/通知机制。join 方法的底层是通过 wait 方法实现的，当线程终止时会调用自身的 notifyAll 方法，通知所有等待在该线程对象上的线程。

**ThreadLocal**

ThreadLoacl 是线程变量，内部以 ThreadLocal 为键，任意对象为值的存储结构实现。该结构绑定在每个线程上，存储的值在每个线程中都是一个唯一副本，每个线程可以通过 ThreadLocal 对象访问自己唯一的值。

这种存储结构叫 ThreadLocalMap ，是 ThreadLocal 的一个静态内部类，是一个弱引用集合，它的存值、取值实现类似于 HashMap，使用 set 设置值，使用 get 获取值。使用弱引用的目的是为了节约资源，如果执行过程中发生了 GC，ThreadLocal 是 null 但由于 ThreadLocalMap 生命周期和线程一样，不会被回收，这时候就会导致 ThreadLocalMap 的 key 不存在而 value 还在的内存泄漏问题，解决办法是使用完 ThreadLocal 后执行remove操作。

---

### P14：ConcurrentHashMap

**JDK 8 之前**

ConcurrentHashMap 用于解决 HashMap 的线程不安全和 HashTable 的并发效率低下问题，HashTable 之所以效率低下是因为所有线程都必须竞争同一把锁，假如容器里有多把锁，每一把锁用于锁容器一部分数据，那么多线程访问容器不同数据段的数据时线程间就不会存在锁竞争，从而有效提高并发效率，这就是 ConcurrentHashMap 的锁分段技术。首先将数据分成 Segment 数据段，然后给每一个数据段配一把锁，当一个线程占用锁访问其中一个段的数据时，其他段的数据也能被其他线程访问。

get 操作实现简单高效，先经过一次再散列，然后使用这个散列值通过散列运算定位到 Segment，再通过散列算法定位到元素。get 的高效在于这个过程不需要加锁，除非读到空值才会加锁重读。get 方法里将要使用的共享变量都定义为 volatile 类型，volatile 保证了多线程的可见性，可以多线程读，但是只能保证单线程写，在 get 操作里只需要读所以不用加锁。

put 操作必须加锁，put 方法首先定位到 Segment，然后进行插入操作，第一步判断是否需要对 Segment 里的 HashEntry 数组进行扩容，第二步定位添加元素的位置，然后将其放入数组。

size 操作用于统计元素的数量，必须统计每个 Segment 的大小然后求和，在统计结果累加的过程中，之前累加过的 count 变化的几率很小，因此 ConcurrentHashMap 的做法是先尝试两次通过不加锁的方式统计结果，如果统计过程中容器大小发生了变化则再通过加锁的方式统计所有 Segment 的大小。判断容器是否发生变化是根据 modCount 变量确定的。

**JDK 8 开始**

JDK 8 的实现摒弃了 Segment 分段概念，使用 Synchronized 和 CAS 来保证线程安全。

get 操作同样不需要同步控制，put 操作时如果没有出现哈希冲突，就使用 CAS 方式来添加元素，如果出现了哈希冲突就使用 synchronized 加锁的方式添加元素。

---

### P15：CAS 操作

CAS 表示 Compare And Swap，比较并交换，CAS 需要三个操作数，分别是内存位置 V、旧的预期值 A 和准备设置的新值 B。CAS 指令执行时，当且仅当 V 符合 A 时，处理器才会用 B 更新 V 的值，否则它就不执行更新。但不管是否更新都会返回 V 的旧值，这些处理过程是原子操作，执行期间不会被其他线程打断。

在 JDK 5 后，Java 类库中才开始使用 CAS 操作，该操作由 Unsafe 类里的 `compareAndSwapInt` 等几个方法包装提供。HotSpot 在内部对这些方法做了特殊处理，即时编译的结果是一条平台相关的处理器 CAS 指令。Unsafe 类不是给用户程序调用的类，因此在 JDK 9 之前只有 Java 类库可以使用 CAS，譬如 juc 包里的 AtomicInteger类中 `compareAndSet` 等方法都使用了Unsafe 类的 CAS 操作来实现。

尽管 CAS 既简单又高效，但这种操作无法涵盖互斥同步的所有场景，并且 CAS 从语义上来说存在一个逻辑漏洞：如果 V 初次读取的时候是 A，并且在准备赋值的时候检查到它的值仍为 A，这依旧不能说明它的值没有被其他线程更改过，因为这段时间内假设它的值先改为了 B 又改回 A，那么 CAS 操作就会误认为它从来没有被改变过。这个漏洞称为 ABA 问题，juc 包提供了一个 AtomicStampedReference，原子更新带有版本号的引用类型，它可以通过控制变量值的版本来解决 ABA 问题。这个类并不常用，大部分情况下 ABA 问题不会影响程序并发的正确性，如果需要解决该问题，改用传统的互斥同步可能会比原子类更高效。

---

### P16：原子操作类

Java 从 JDK 5 开始提供了 java.util.concurrent.atomic 包，这个包中的原子操作类提供了一种用法简单、性能高效、线程安全地更新一个变量的方式。到 JDK 8 该包共有17个类，依据作用分为四种：原子更新基本类型类、原子更新数组类、原子更新引用类以及原子更新字段类，atomic 包里的类基本都是使用 Unsafe 实现的包装类。

**原子更新基本类型**

AtomicInteger 原子更新整形、 AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。

getAndIncrement 以原子方式将当前的值加 1，首先在 for 死循环中取得 AtomicInteger  里存储的数值，第二步对 AtomicInteger  当前的值进行加 1 操作，第三步调用 compareAndSet 方法进行原子更新，该操作先检查当前数值是否等于 expect，如果等于则说明当前值没有被其他线程修改，则将值更新为 next，否则会更新失败返回 false，程序会进入 for 循环重新进行 compareAndSet 操作。

atomic 包中只提供了 三种基本类型的原子更新，atomic 包里的类基本都是使用 Unsafe 实现的，Unsafe 只提供三种 CAS 方法：compareAndSwapInt、compareAndSwapLong 和 compareAndSwapObject，例如原子更新 Boolean 时是先转成整形再使用 compareAndSwapInt 进行 CAS，所以原子更新 char、float、double 也可以用类似思路实现。

**原子更新数组**

AtomicIntegerArray，原子更新整形数组里的元素、 AtomicLongArray 原子更新长整型数组里的元素、 AtomicReferenceArray 原子更新引用类型数组里的元素。

**原子更新引用**

AtomicReference 原子更新引用类型、AtomicMarkableReference 原子更新带有标记位的引用类型，可以绑定一个 boolean 类型的标记位、 AtomicStampedReference 原子更新带有版本号的引用类型，关联一个整数值用于原子更新数据和数据的版本号，可以解决 ABA 问题。

**原子更新字段**

AtomicIntegerFieldUpdater 原子更新整形字段的更新器、 AtomicLongFieldUpdater 原子更新长整形字段的更新器AtomicReferenceFieldUpdater 原子更新引用类型字段的更新器。

由于原子更新字段类都是抽象类，每次使用的时候必须使用静态方法 newUpdater 创建一个更新器，并且需要设置想要更新的类和字段。并且更新类的字段必须使用 public volatile 修饰。

**JDK 8 更新的类**

DoubleAccumulator 、 LongAccumulator、DoubleAdder、LongAdder、Striped64。

---

### P17：并发工具类

**等待多线程完成的 CountDownLatch**

CountDownLatch 允许一个或多个线程等待其他线程完成操作，构造器接收一个 int 类型的参数作为计数器，如果要等待 n 个点就传入 n。每次调用 countDown 方法时计数器减 1，await 方法会阻塞当前线程直到计数器变为0，由于 countDown方法可用在任何地方，所以 n 个点既可以是 n 个线程也可以是一个线程里的 n 个执行步骤。

**同步屏障 CyclicBarrier**

同步屏障的作用是让一组线程到达一个屏障或同步点时被阻塞，直到最后一个线程到达屏障时，屏障才会解除，所有被拦截的线程才会继续运行。构造器中的参数表示屏障拦截的线程数量，每个线程调用 await 方法告诉 CyclicBarrier 自己已到达屏障，然后当前线程被阻塞。还支持在构造器中传入一个 Runable 类型的任务，当线程到达屏障时会优先执行该任务。适用于多线程计算数据，最后合并计算结果的应用场景。

CountDownLacth 的计数器只能用一次，而 CyclicBarrier 的计数器可使用 reset 方法重置，所以 CyclicBarrier 能处理更为复杂的业务场景，例如计算错误时可用重置计数器重新计算。

**控制并发线程数的 Semaphore**

信号量用来控制同时访问特定资源的线程数量，它通过协调各个线程以保证合理使用公共资源。信号量可以用于流量控制，特别是公共资源有限的应用场景，比如数据库连接。Semaphore 的构造器参数接收一个 int 值，表示可用的许可数量即最大并发数。使用acquire 方法获得一个许可证，使用 release 方法归还许可，还可以用 tryAcquire 尝试获得许可。

**线程间交换数据的 Exchanger** 

交换者是用于线程间协作的工具类，用于进行线程间的数据交换。它提供一个同步点，在这个同步点两个线程可以交换彼此的数据。这两个线程通过 exchange 方法交换数据，如果第一个线程先执行exchange方法它会阻塞等待第二个线程执行exchange方法，当两个线程都到达同步点时这两个线程就可以交换数据，将本线程生产出的数据传递给对方。应用场景包括遗传算法、校对工作等。

---

### P18：线程池

**好处**

① 降低资源消耗，复用已创建的线程降低线程创建和消耗的开销。② 提高响应速度，当任务到达时，任务可以不需要等到线程创建就可以立即执行。③ 提高线程的可管理性，线程是稀缺资源，如果无限制地创建不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一分配、调优和监控。

**当提交一个新任务到线程池时的处理流程**

① 判断核心线程池里的线程是否都在执行任务，如果不是则创建一个新的工作线程来执行任务，此时 workCount < corePoolSize，这一步需要获取全局锁。② 如果核心线程池已满，判断工作队列是否已满，如果没有就将新提交的任务存储在工作队列中，此时 workCount >= corePoolSize。③ 如果工作队列已满，判断线程池的线程是否都处于工作状态，如果没有则创建一个新的工作线程来执行任务，此时 workCount < maximumPoolSize，这一步也需要获取全局锁。④ 如果线程池已满，按照拒绝策略来处理任务，此时 workCount > maximumPoolSize。

线程池采取这种设计思路是为了在执行 execute 方法时尽可能地避免获取全局锁，在线程池完成预热之后，即当前运行的线程数大于等于corePoolSize 时，几乎所有的 execute 方法都是执行步骤 2，不需要获取全局锁。

线程池创建线程时，会将线程封装成工作线程 Worker，Worker 在执行完任务后还会循环获取工作队列中的任务来执行。线程池中的线程执行任务分为两种情况：①在 execute 方法中创建一个线程时会让这个线程执行当前任务。②这个线程执行完任务之后，就会反复从工作队列中获取任务并执行。

可以使用 execute 和 submit 方法向线程池提交任务。execute 用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功了。submit 用于提交需要返回值的任务，线程池会返回一个 Future 类型的对象，通过该对象可以判断任务是否执行成功，并且可以通过该对象的 get 方法获取返回值，get 会阻塞当前线程直到任务完成，带超时参数的 get 方法会在阻塞当前线程一段时间后立即返回，这时任务可能还没有完成。

**创建线程池的参数**

① **corePoolSize：**线程池的基本大小，当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池的基本大小时就不再创建。② **workQueue：**工作队列，用于保存等待执行任务的阻塞队列。③ **maximumPoolSize：**线程池最大数量，如果工作队列已满，并且创建的线程数小于最大线程数，则线程池还会创建新的线程执行任务，如果使用无界阻塞队列该参数无意义。④ **threadFactory：**用于设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字。⑤ **handler：**拒绝策略，默认策略下使用 AbortPolicy 直接抛出异常，CallerRunsPolicy 表示重新尝试提交该任务，DiscardOldestPolicy 表示抛弃队列里最近的一个任务并执行当前任务，DiscardPolicy 表示直接抛弃当前任务不处理。⑥ **keepAliveTime：**线程活动的保持时间，线程池工作线程空闲后保持存活的时间，所以如果任务很多，且每个任务的执行时间较短，可以调大时间提高线程的利用率。⑦ **unit：**线程活动保持时间的单位。

**关闭线程池**

可以通过调用 shutdown 或 shutdownNow 方法关闭线程池，原理是遍历线程池中的工作线程，然后逐个调用线程的 interrupt 方法中断线程，所以无法响应中断的任务可能永远无法终止。区别是 shutdownNow 首先将线程池的状态设为 STOP，然后尝试停止所有正在执行或暂停任务的线程，并返回等待执行任务的列表，而 shutdown 只是将线程池的状态设为 SHUTDOWN，然后中断所有没有正在执行任务的线程。通常调用 shutdown 来关闭线程池，如果任务不一定要执行完则可以调用 shutdownNow。

**合理设置线程池**

首先可以从以下角度分析：①任务的性质：CPU密集型任务、IO密集型任务和混合型任务。②任务的优先级：高、中和低。③任务的执行时间：长、中和短。④任务的依赖性：是否依赖其他系统资源，如数据库连接。

性质不同的任务可以用不同规模的线程池分开处理，CPU密集型任务应配置尽可能小的线程，如配置 N~cpu~+1 个线程的线程池。由于IO密集型任务线程并不是一直在执行任务，则应配置尽可能多的线程，如 2 * N~cpu~。混合型的任务，如果可以拆分，将其拆分为一个 CPU 密集型任务和一个 IO 密集型任务，只要这两个任务执行的时间相差不是太大那么分解后的吞吐量将高于串行执行的吞吐量，如果相差太大则没必要分解。

优先级不同的任务可以使用优先级队列 PriorityBlockingQueue 处理。

执行时间不同的任务可以交给不同规模的线程池处理，或者使用优先级队列让执行时间短的任务先执行。

依赖数据库连接池的任务，由于线程提交 SQL 后需要等待数据库返回的结果，等待的时间越长 CPU 空闲的时间就越长，因此线程数应该尽可能地设置大一些提高CPU的利用率。

建议使用有界队列，能增加系统的稳定性和预警能力，可以根据需要设置的稍微大一些。

---

### P19：阻塞队列

阻塞队列支持阻塞的插入和移除，当队列满时，阻塞插入元素的线程直到队列不满。当队列为空时，获取元素的线程会被阻塞直到队列非空。阻塞队列常用于生产者和消费者的场景，阻塞队列就是生产者用来存放元素，消费者用来获取元素的容器。

**Java中的阻塞队列**

ArrayBlockingQueue，由数组组成的有界阻塞队列，默认情况下不保证线程公平，有可能先阻塞的线程最后才访问队列。

LinkedBlockingQueue，由链表结构组成的有界阻塞队列，队列的默认和最大长度为 Integer 的最大值。

PriorityBlockingQueue，支持优先级排序的无界阻塞队列，默认情况下元素按照顺序升序排序。可以自定义 compareTo 方法指定元素排序规则，或者初始化时指定 Comparator 对元素排序，不能保证同优先级元素的顺序。

DelayQueue，支持延时获取元素的无界阻塞队列，使用优先级队列实现。创建元素时可以指定多久才能从队列中获取当前元素，只有延迟期满时才能从队列中获取元素，适用于缓存系统和定时任务调度。

SynchronousQueue，不存储元素的阻塞队列，每一个 put 操作必须等待一个 take 操作。默认使用非公平策略，也支持公平策略，适用于传递性场景，吞吐量高于 ArrayBlockingQueue 和 LinkedBlockingQueue。

LinkedTransferQueue，由链表组成的无界阻塞队列，相对于其他阻塞队列多了 tryTransfer 和 transfer 方法。transfe方法：如果当前有消费者正在等待接收元素，可以把生产者传入的元素立刻传输给消费者，如果没有消费者等待接收元素，会将元素放在队列的尾节点并等到该元素被消费者消费了才返回。tryTransfer 方法用来试探生产者传入的元素能否直接传给消费者，如果没有消费者等待接收元素则返回 false，和transfer 的区别是无论消费者是否消费都会立即返回。

LinkedBlockingDeque，由链表组成的双向阻塞队列，可以从队列的两端插入和移出元素，在多线程同时入队时减少了竞争。

**实现原理**

使用通知模式实现，当生产者往满的队列里添加元素时会阻塞生产者，当消费者消费了一个队列中的元素后，会通知生产者当前队列可用。例如 JDK 中的 ArrayBlockingQueue 使用了 Condition 实现。当往队列里插入一个元素，如果队列不可用，那么阻塞生产者主要通过LockSupport 的 park 方法实现，park 在不同的操作系统中使用不同的方式实现，在 Linux 下使用的是系统方法 pthread_cond_wait 实现。

---

### P20：Executor 框架

Java 的线程既是工作单元，也是执行机制。从 JDK 5开始把工作单元与执行机制分离开来，工作单元包括 Runnable 和 Callable，而执行机制由 Exectuor 框架提供。

在 HotSpot 的线程模型中，Java 线程被一对一映射为本地操作系统线程，Java 线程启动时会创建一个本地操作系统线程，当该 Java线程终止时，这个操作系统线程也会被回收，操作系统会调度所有线程并将它们分配给可用的CPU。

在上层，Java 多线程程序通常把应用分解为若干任务，然后使用用户级的调度器即 Executor 框架将这些任务映射为固定数量的线程；在底层，操作系统内核将这些线程映射到硬件处理器上。

Executor 框架主要由三部分组成：①任务，包括被执行任务需要实现的接口，Runnable 或 Callable。②任务的执行，包括任务执行机制的核心接口 Executor 以及继承自 Executor 的 ExecutorService 。③异步计算的结果，包括接口 Future 和实现 Future 接口的 FutureTask 类。

**ThreadPoolExecutor**

ThreadPoolExecutor 是 Executor框架最核心的类，是线程池的实现类，主要有三种。

① FixedThreadPool，可重用固定线程数的线程池，corePoolSize 和 maximumPoolSize都被设置为创建时的指定参数 nThreads，当线程池中的线程数大于 corePoolSize 时，keepAliveTime 为多余的空闲线程等待新任务的最长时间，超过这个时间后的多余线程将被终止，将其设置为 0L 时多余空闲线程将被立即终止。该线程池使用的工作队列是无界阻塞队列 LinkedBlockingQueue，适用于为了满足资源管理的需求，而需要限制当前线程数量的应用场景，适用于负载比较重的服务器。

② SingleThreadExecutor，使用单个线程的线程池，corePoolSize 和 maximumPoolSize都被设置为 1，其他参数和 FiexedThreadPool相同。适用于需要保证顺序执行各个任务，并且在任意时间点不会有多个线程是活动的的应用场景。

③ CachedThreadPool，一个根据需要创建线程的线程池，corePoolSize 被设置为0，maximumPoolSize 被设置为 Integer 最大值。该线程池使用的工作队列是没有容量的 SynchronousQueue，但由于 maximumPoolSize 设为 Integer最大值，如果主线程提交任务的速度高于线程处理的速度，线程池会不断创建新线程，极端情况下会创建过多线程而耗尽CPU和内存资源。适用于执行很多短期异步任务的小程序，或者负载较轻的服务器。

**ScheduledThreadPoolExecutor**

继承自 ThreadPoolExecutor，主要用来在给定的延迟之后运行任务，或者定期执行任务。其功能与 Timer 类似但更强大和灵活。Timer对应的是单个后台线程，而 ScheduledThreadPoolExecutor 可以在构造器中指定多个对应的后台线程数。

主要有两种：① ScheduledThreadPool：创建固定线程个数的线程池，适用于需要多个后台线程执行周期任务，同时需要限制后台线程数量的应用场景。② SingleThreadScheduledExecutor：只包含一个线程的线程池，适用于单个后台线程执行周期任务，同时需要保证顺序执行任务的应用场景。

实现原理是将待调度任务放入一个DelayQueue 中，调度任务主要有三个 long 类型的参数，time 表示这个任务将要被执行的具体时间，sequenceNumber 表示这个任务被添加到线程池的序号，period 表示任务执行时间间隔。DelayQueue封装了一个PriorityQueue，队列按照 time 进行排序，如果相同则比较sequenceNumber，越小的排在前面，即如果两个任务的执行时间相同，先提交的任务先被执行。

---

## Spring 10

### P1：Spring 框架

Spring 是分层的企业级应用轻量级开源框架，以 IoC 和 AOP为内核。Spring 可以降低企业级应用开发的复杂性，对此主要采取了四个关键策略：基于 POJO 的轻量级和最小侵入性编程、通过依赖注入和面向接口实现松耦合、基于切面和惯性进行声明式编程、通过切面和模板减少样板式代码。

**好处**

降低代码耦合度、简化开发。通过 Spring 提供的 IoC 容器可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造成的过度程序耦合。用户也不必再为单例模式类、属性文件解析等这些底层的需求编写代码，可以更专注于上层的应用。

AOP 编程以及声明式事务的支持。通过 Spring 的 AOP 功能可以方便进行面向切面的编程，通过声明式事务可以灵活进行事务管理，提高开发效率和质量。

方便程序的测试和集成各种框架。可以用非容器依赖的编程方式进行几乎所有的测试工作，可以降低各种框架的使用难度，提供了对 Mybatis 和 Hibernate 等框架的直接支持。

降低了 JavaEE API 的使用难度。Spring 对 JDBC、JavaMail、远程调用等 API 进行了封装，使这些 API 的使用难度大幅降低。

**核心容器**

核心容器由 spring-beans、spring-core、spring-context 和 spring-expression 四个模块组成。

spring-beans 和 spring-core 模块是 Spring 的核心模块，包括了控制反转和依赖注入。BeanFactory 使用控制反转对应用程序的配置和依赖性规范与实际的应用代码进行分离，BeanFactory 实例化后并不会自动实例化 Bean，只有当 Bean 被使用时才会对其进行实例化与依赖关系的装配。

spring-context 模块构架于核心模块之上，扩展了 BeanFactory，为它添加了 Bean 的生命周期控制、框架事件体系及资源透明化加载等功能。ApplicationConext 是该模块的核心接口，它是 BeanFactory 的子接口，它实例化后会自动对所有单例 Bean 进行实例化与依赖关系的装配，使之处于待用状态。

spring-expression 是 EL 语言的扩展模块，可以查询、管理运行中的对象，同时也可以方便地调用对象方法，以及操作数组、集合等。

---

### P2：IoC 控制反转

IoC 即控制反转，是一种给予应用程序中目标组件更多控制的设计范式，简单来说就是把原来代码里需要实现的对象创建、依赖反转给容器来帮忙实现，需要创建一个容器并且需要一种描述来让容器知道要创建的对象之间的关系，在 Spring 框架中管理对象及其依赖关系是通过 Spring 的 IoC 容器实现的，IoC 的作用是降低代码耦合度。

IoC 的实现方式有依赖注入和依赖查找，由于依赖查找使用的很少，因此 IoC 也叫做依赖注入。依赖注入指对象被动地接受依赖类而不用自己主动去找，对象不是从容器中查找它依赖的类，而是在容器实例化对象时主动将它依赖的类注入给它。假设一个 Car 类需要一个 Engine 的对象，那么一般需要需要手动 new 一个 Engine，利用 IoC 就只需要定义一个私有的 Engine 类型的成员变量，容器会在运行时自动创建一个 Engine 的实例对象并将引用自动注入给成员变量。

**基于 XML 的容器初始化**

当创建一个 ClassPathXmlApplicationContext 时，构造器做了两件事：首先调用父容器的构造器为容器设置好 Bean 资源加载器，然后调用父类的 setConfigLocations 方法设置 Bean 配置信息的定位路径。

ClassPathXmlApplicationContext 通过调用父类 AbstractApplicationContext 的 refresh 方法启动整个 IoC 容器对 Bean 定义的载入过程，refresh 是一个模板方法，规定了 IoC 容器的启动流程。refresh 方法的主要作用是：在创建 IoC 容器之前如果已有容器存在，需要把已有的容器销毁和关闭，以保证在 refresh 方法之后使用的是新创建的 IoC 容器。

容器创建后通过 loadBeanDefinitions 方法加载 Bean 配置资源，该方法会做两件事：首先调用资源加载器的方法获取要加载的资源，其次真正执行加载功能，由子类 XmlBeanDefinitionReader 实现。在加载资源时，首先会解析配置文件路径，读取配置文件的内容，然后通过 XML 解析器将 Bean 配置信息转换成文档对象，之后再按照 Spring Bean 的定义规则对文档对象进行解析。

Spring IoC 容器中注册解析的 Bean 信息存放在一个 HashMap 集合中，key 是 String 字符串，值是 BeanDefinition，在注册过程中需要使用 synchronized 同步块保证线程安全。当 Bean 配置信息中配置的 Bean 被解析后且被注册到 IoC 容器中，初始化就算真正完成了，Bean 定义信息已经可以使用，并且可以被检索。Spring IoC 容器的作用就是对这些注册的 Bean 定义信息进行处理和维护，注册的 Bean 定义信息是控制反转和依赖注入的基础。

**基于注解的容器初始化**

Spring 对注解的处理分为两种方式：① 直接将注解 Bean 注册到容器中，可以在初始化容器时注册，也可以在容器创建之后手动注册，然后刷新容器使其对注册的注解 Bean 进行处理。② 通过扫描指定的包及其子包的所有类处理，在初始化注解容器时指定要自动扫描的路径。

---

### P3：DI 依赖注入

**可注入的数据类型**

基本数据类型和 String、集合类型、Bean 类型。

**实现方式**

构造器注入：IoC Service Provider 会检查被注入对象的构造器，取得它所需要的依赖对象列表，进而为其注入相应的对象。这种方法的优点是在对象构造完成后就处于就绪状态，可以马上使用。缺点是当依赖对象较多时，构造器的参数列表会比较长，构造器无法被继承，无法设置默认值。对于非必需的依赖处理可能需要引入多个构造器，参数数量的变动可能会造成维护的困难。

setter 方法注入：当前对象只需要为其依赖对象对应的属性添加 setter 方法，就可以通过 setter 方法将依赖对象注入到被依赖对象中。setter 方法注入在描述性上要比构造器注入强，并且可以被继承，允许设置默认值。缺点是无法在对象构造完成后马上进入就绪状态。

接口注入：必须实现某个接口，这个接口提供一个方法来为其注入依赖对象。使用较少，因为它强制要求被注入对象实现不必要的接口，侵入性强。

**相关注解**

`@Autowired`：自动按类型注入，如果有多个匹配则按照指定 Bean 的 id 查找，查找不到会报错。

`@Qualifier`：在自动按照类型注入的基础上再按照 Bean 的 id 注入，给变量注入时必须搭配 `@Autowired`，给方法注入时可单独使用。

`@Resource` ：直接按照 Bean 的 id 注入，只能注入 Bean 类型。

`@Value` ：用于注入基本数据类型和 String 类型。

**依赖注入的过程**

getBean 方法是获取 Bean 实例的方法，该方法会调用 doGetBean 方法，doGetBean 真正实现向 IoC 容器获取 Bean 的功能，也是触发依赖注入的地方。如果 Bean 定义为单例模式，容器在创建之前先从缓存中查找以确保整个容器中只存在一个实例对象。如果 Bean 定义为原型模式，则容器每次都会创建一个新的实例。

具体创建 Bean 实例对象的过程由 ObjectFactory 的 createBean 方法完成，该方法主要通过 createBeanInstance 方法生成 Bean 包含的 Java 对象实例和 populateBean 方法对 Bean 属性的依赖注入进行处理。

在 createBeanInstance 方法中根据指定的初始化策略，通过简单工厂、工厂方法或容器的自动装配特性生成 Java 实例对象，对工厂方法和自动装配特性的 Bean，调用相应的工厂方法或参数匹配的构造器即可完成实例化对象的工作，但最常用的默认无参构造器需要使用 JDK 的反射或 CGLib 来进行初始化。

在 populateBean 方法中，注入过程主要分为两种情况：① 属性值类型不需要强制转换时，不需要解析属性值，直接进行依赖注入。② 属性值类型需要强制转换时，首先需要解析属性值，然后对解析后的属性值进行依赖注入。依赖注入的过程就是将 Bean 对象实例设置到它所依赖的 Bean 对象属性上，真正的依赖注入是通过 setPropertyValues 方法实现的，该方法使用了委派模式。

BeanWrapperImpl 类负责对容器完成初始化的 Bean 实例对象进行属性的依赖注入，对于非集合类型的属性，大量使用 JDK 的反射机制，通过属性的 getter 方法获取指定属性注入前的值，同时调用属性的 setter 方法为属性设置注入后的值。对于集合类型的属性，将属性值解析为目标类型的集合后直接赋值给属性。

当 Spring IoC 容器对 Bean 定义资源的定位、载入、解析和依赖注入全部完成后，就不再需要我们手动创建所需的对象，Spring IoC 容器会自动为我们创建对象并且注入好相关依赖。

---

### P4：Bean 对象

**生命周期**

在 IoC 容器的初始化过程中会对 Bean 定义完成资源定位，加载读取配置并解析，最后将解析的 Bean 信息放在一个 HashMap 集合中。当 IoC 容器初始化完成后，会进行对 Bean 实例的创建和依赖注入过程，注入对象依赖的各种属性值，在初始化时可以指定自定义的初始化方法。经过这一系列初始化操作后 Bean 达到可用状态，接下来就可以使用 Bean 了，当使用完成后会调用 destroy 方法进行销毁，此时也可以指定自定义的销毁方法，最终 Bean 被销毁且从容器中移除。

指定 Bean 初始化和销毁的方法：

XML 方式通过配置 bean 标签中的 init-Method 和 destory-Method 指定自定义初始化和销毁方法。 

注解方式通过 `@PreConstruct` 和 `@PostConstruct` 注解指定自定义初始化和销毁方法。

**作用范围**

通过 scope 属性指定 bean 的作用范围，包括：① singleton：单例模式，是默认作用域，不管收到多少 Bean 请求每个容器中只有一个唯一的 Bean 实例。② prototype：原型模式，和 singleton 相反，每次 Bean 请求都会创建一个新的实例。③ request：每次 HTTP 请求都会创建一个新的 Bean 并把它放到 request 域中，在请求完成后 Bean 会失效并被垃圾收集器回收。④ session：和 request 类似，确保每个 session 中有一个 Bean 实例，session 过期后 bean 会随之失效。⑤ global session：当应用部署在 Portlet 容器中时，如果想让所有 Portlet 共用全局存储变量，那么这个变量需要存储在 global session 中。

**创建方式**

**XML**

通过默认无参构造器，只需要指明 bean 标签中的 id 和 class 属性，如果没有无参构造器会报错。

使用静态工厂方法，通过 bean 标签中的 class 属性指明静态工厂，factory-method 属性指明静态工厂方法。

使用实例工厂方法，通过 bean 标签中的 factory-bean 属性指明实例工厂，factory-method 属性指明实例工厂方法。

**注解**

`@Component` 把当前类对象存入 Spring 容器中，相当于在 xml 中配置一个 bean 标签。value 属性指定 bean 的 id，默认使用当前类的首字母小写的类名。

`@Controller`，` @Service`，` @Repository` 三个注解都是 `@Component`  的衍生注解，作用及属性都是一模一样的。只是提供了更加明确语义，`@Controller` 用于表现层，`@Service`用于业务层，`@Repository`用于持久层。如果注解中有且只有一个 value 属性要赋值时可以省略 value。

如果想将第三方的类变成组件又没有源代码，也就没办法使用 `@Component` 进行自动配置，这种时候就要使用 `@Bean` 注解。被 `@Bean` 注解的方法返回值是一个对象，将会实例化，配置和初始化一个新对象并返回，这个对象由 Spring 的 IoC 容器管理。name 属性用于给当前 `@Bean`  注解方法创建的对象指定一个名称，即 bean 的 id。当使用注解配置方法时，如果方法有参数，Spring 会去容器查找是否有可用 bean对象，查找方式和 `@Autowired` 一样。

`@Configuration` 用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解，value 属性用于指定配置类的字节码。

`@ComponentScan` 用于指定 Spring 在初始化容器时要扫描的包。basePackages 属性用于指定要扫描的包。

`@PropertySource` 用于加载 `.properties` 文件中的配置。value 属性用于指定文件位置，如果是在类路径下需要加上 classpath。

`@Import` 用于导入其他配置类，在引入其他配置类时可以不用再写 `@Configuration` 注解。有 `@Import` 的是父配置类，引入的是子配置类。value 属性用于指定其他配置类的字节码。

**BeanFactory、FactoryBean 和 ApplicationContext 的区别**

BeanFactory 是一个 Bean 工厂，实现了工厂模式，是 Spring IoC 容器最顶级的接口，可以理解为含有 Bean 集合的工厂类，它的作用是管理 Bean，包括实例化、定位、配置应用程序中的对象及建立这些对象之间的依赖。BeanFactory 实例化后并不会自动实例化 Bean，只有当 Bean 被使用时才会对其进行实例化与依赖关系的装配，属于延迟加载，适合多例模式。

FactoryBean 是一个工厂 Bean，作用是生产其他 Bean 实例，可以通过实现该接口，提供一个工厂方法来自定义实例化 Bean 的逻辑。

ApplicationConext 是 BeanFactory 的子接口，扩展了 BeanFactory 的功能，提供了支持国际化的文本消息，统一的资源文件读取方式，事件传播以及应用层的特别配置等。容器会在初始化时对配置的 Bean 进行预实例化，Bean 的依赖注入在容器初始化时就已经完成，属于立即加载，适合单例模式，一般推荐使用 ApplicationContext。

---

### P5：AOP 面向切面编程

**概念和原理**

AOP 即面向切面编程，简单地说就是将代码中重复的部分抽取出来，在需要执行的时候使用动态代理的技术，在不修改源码的基础上对方法进行增强。优点是可以减少代码的冗余，提高开发效率，维护方便。

Spring 会根据类是否实现了接口来判断动态代理的方式，如果实现了接口会使用 JDK 的动态代理，核心是 InvocationHandler 接口和 Proxy 类，如果没有实现接口会使用 CGLib 动态代理，CGLib 是在运行时动态生成某个类的子类，如果某一个类被标记为 final，是不能使用 CGLib 动态代理的。

JDK 动态代理主要通过重组字节码实现，首先获得被代理对象的引用和所有接口，生成新的类必须实现被代理类的所有接口，动态生成Java 代码后编译新生成的 `.class` 文件并重新加载到 JVM 运行。JDK 代理直接写 Class 字节码，CGLib是采用ASM框架写字节码，生成代理类的效率低。但是CGLib调用方法的效率高，因为 JDK 使用反射调用方法，CGLib 使用 FastClass 机制为代理类和被代理类各生成一个类，这个类会为代理类或被代理类的方法生成一个 index，这个 index 可以作为参数直接定位要调用的方法。

常用场景包括权限认证、自动缓存、错误处理、日志、调试和事务等。

**相关注解**

`@Aspect`：声明被注解的类是一个切面 Bean。

`@Before`：前置通知，指在某个连接点之前执行的通知。

`@After`：后置通知，指某个连接点退出时执行的通知（不论正常返回还是异常退出）。

`@AfterReturning`：返回后通知，指某连接点正常完成之后执行的通知，返回值使用returning属性接收。

`@AfterThrowing`：异常通知，指方法抛出异常导致退出时执行的通知，和`@AfterReturning`只会有一个执行，异常使用throwing属性接收。

**相关术语**

`Aspect`：切面，一个关注点的模块化，这个关注点可能会横切多个对象。

`Joinpoint`：连接点，程序执行过程中的某一行为，即业务层中的所有方法。。

`Advice`：通知，指切面对于某个连接点所产生的动作，包括前置通知、后置通知、返回后通知、异常通知和环绕通知。

`Pointcut`：切入点，指被拦截的连接点，切入点一定是连接点，但连接点不一定是切入点。

`Proxy`：代理，Spring AOP 中有 JDK 动态代理和 CGLib 代理，目标对象实现了接口时采用 JDK 动态代理，反之采用 CGLib 代理。

`Target`：代理的目标对象，指一个或多个切面所通知的对象。

`Weaving` ：织入，指把增强应用到目标对象来创建代理对象的过程。

**AOP 的过程**

Spring AOP 是由 BeanPostProcessor 后置处理器开始的，这个后置处理器是一个监听器，可以监听容器触发的 Bean 生命周期事件，向容器注册后置处理器以后，容器中管理的 Bean 就具备了接收 IoC 容器回调事件的能力。BeanPostProcessor 的调用发生在 Spring IoC 容器完成 Bean 实例对象的创建和属性的依赖注入之后，为 Bean 对象添加后置处理器的入口是 initializeBean 方法。

Spring 中 JDK 动态代理生通过 JdkDynamicAopProxy 调用 Proxy 的 newInstance 方法来生成代理类，JdkDynamicAopProxy 也实现了 InvocationHandler 接口，invoke 方法的具体逻辑是先获取应用到此方法上的拦截器链，如果有拦截器则创建 MethodInvocation 并调用其 proceed 方法，否则直接反射调用目标方法。因此 Spring AOP 对目标对象的增强是通过拦截器实现的。

---

### P6：Spring MVC 核心组件

`DispatcherServlet`：SpringMVC 中的前端控制器，是整个流程控制的核心，负责接收请求并转发给对应的处理组件。

`Handler`：处理器，完成具体业务逻辑，相当于 Servlet 或 Action。

`HandlerMapping`：完成URL 到 Controller映射的组件，DispatcherServlet 接收到请求之后，通过 HandlerMapping 将不同的请求映射到不同的 Handler。

`HandlerInterceptor`：处理器拦截器，是一个接口，如果需要完成一些拦截处理，可以实现该接口。

`HandlerExecutionChain`：处理器执行链，包括两部分内容：Handler 和 HandlerInterceptor。

`HandlerAdapter`：处理器适配器，Handler执行业务方法前需要进行一系列操作，包括表单数据验证、数据类型转换、将表单数据封装到JavaBean等，这些操作都由 HandlerAdapter 完成。DispatcherServlet 通过 HandlerAdapter 来执行不同的 Handler。

`ModelAndView`：装载了模型数据和视图信息，作为 Handler 的处理结果返回给 DispatcherServlet。

`ViewResolver`：视图解析器，DispatcherServlet 通过它将逻辑视图解析为物理视图，最终将渲染的结果响应给客户端。

---

### P7：Spring MVC 处理流程

Web 容器启动时会通知 Spring 初始化容器，加载 Bean 的定义信息并初始化所有单例 Bean，然后遍历容器中的 Bean，获取每一个 Controller 中的所有方法访问的 URL，将 URL 和对应的 Controller 保存到一个 Map 集合中。

所有的请求会转发给 DispatcherServlet 前端处理器处理，DispatcherServlet 会请求 HandlerMapping 找出容器中被 `@Controler` 注解修饰的 Bean 以及被 `@RequestMapping` 修饰的方法和类，生成 Handler 和 HandlerInterceptor 并以一个 HandlerExcutionChain 处理器执行链的形式返回。

之后 DispatcherServlet 使用 Handler 找到对应的 HandlerApapter，通过 HandlerApapter 调用 Handler 的方法，将请求参数绑定到方法的形参上，执行方法处理请求并得到 ModelAndView。

最后 DispatcherServlet 根据使用 ViewResolver 试图解析器对得到的 ModelAndView 逻辑视图进行解析得到 View 物理视图，然后对视图渲染，将数据填充到视图中并返回给客户端。

**注解**

`@Controller`：在类定义处添加，将类交给IoC容器管理。

`@RequtestMapping`：将URL请求和业务方法映射起来，在类和方法定义上都可以添加该注解。`value ` 属性指定URL请求的实际地址，是默认值。`method` 属性限制请求的方法类型，包括GET、POST、PUT、DELETE等。如果没有使用指定的请求方法请求URL，会报405 Method Not Allowed 错误。`params` 属性限制必须提供的参数，如果没有会报错。

`@RequestParam`：如果 Controller 方法的形参和 URL 参数名一致可以不添加注解，如果不一致可以使用该注解绑定。`value` 属性表示HTTP请求中的参数名。`required` 属性设置参数是否必要，默认false。`defaultValue` 属性指定没有给参数赋值时的默认值。

`@PathVariable`：Spring MVC 也支持 RESTful 风格的 URL，通过 `@PathVariable` 完成请求参数与形参的绑定。

----

### P8：Spring Data JPA 框架

Spring Data JPA 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套 JPA 应用框架，可使开发者用极简的代码实现对数据库的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展，可以极大提高开发效率。

ORM 即 Object-Relational Mapping ，表示对象关系映射，映射的不只是对象的值还有对象之间的关系，通过 ORM 就可以把对象映射到关系型数据库中。操作实体类就相当于操作数据库表，可以不再重点关注 SQL 语句。

使用时只需要持久层接口继承 JpaRepository 即可，泛型参数列表中第一个参数是实体类类型，第二个参数是主键类型。运行时通过 `JdkDynamicAopProxy` 的 `invoke` 方法创建了一个动态代理对象 `SimpleJpaRepository`，`SimpleJpaRepository` 中封装了 JPA 的操作，通过 `hibernate`（封装了JDBC）完成数据库操作。

**注解**

` @Entity `：表明当前类是一个实体类。

`@Table` ：关联实体类和数据库表。

`@Column` ：关联实体类属性和数据库表中字段。

`@Id` ：声明当前属性为数据库表主键对应的属性。

`@GeneratedValue`： 配置主键生成策略。

`@OneToMany` ：配置一对多关系，mappedBy 属性值为主表实体类在从表实体类中对应的属性名。

`@ManyToOne` ：配置多对一关系，targetEntity 属性值为主表对应实体类的字节码。

`@JoinColumn`：配置外键关系，name 属性值为外键名称，referencedColumnName 属性值为主表主键名称。

**对象导航查询**

通过 get 方法查询一个对象的同时，通过此对象可以查询它的关联对象。

对象导航查询一到多默认使用延迟加载的形式， 关联对象是集合，因此使用立即加载可能浪费资源。

对象导航查询多到一默认使用立即加载的形式， 关联对象是一个对象，因此使用立即加载。

如果要改变加载方式，在实体类注解配置加上 fetch 属性即可，LAZY 表示延迟加载，EAGER 表示立即加载。

---

### P9：Mybatis 框架

Mybatis 是一个实现了数据持久化的 ORM 框架，简单理解就是对 JDBC 进行了封装。

**优点**

相比 JDBC 减少了大量代码量，减少冗余代码。

使用灵活，SQL 语句写在 XML 里，从程序代码中彻底分离，降低了耦合度，便于管理。

提供 XML 标签，支持编写动态 SQL 语句。

提供映射标签，支持对象与数据库的 ORM 字段映射关系。

**缺点**

SQL 语句编写工作量较大，尤其是字段和关联表多时。

SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。

**映射文件标签**

`select`、`insert`、`update`、`delete` 标签分别对应查询、添加、更新、删除操作。

`parameterType`  属性表示参数的数据类型，包括基本数据类型和对应的包装类型、String 和 Java Bean 类型，当有多个参数时可以使用 `#{argn}` 的形式表示第 n 个参数。除了基本数据类型都要以全限定类名的形式指定参数类型。

`resultType`  表示返回的结果类型，包括基本数据类型和对应的包装类型、String 和 Java Bean 类型。还可以使用把返回结果封装为复杂类型的 `resultMap` 。

**缓存**

使用缓存可以减少程序和数据库交互的次数，从而提高程序的运行效率。第一次查询后会自动将结果保存到缓存中，下一次查询时直接从缓存中返回结果无需再次查询数据库。

- **一级缓存**

  SqlSession 级别，默认开启且不能关闭。

  操作数据库时需要创建 SqlSession 对象，在对象中有一个 HashMap 用于存储缓存数据，不同 SqlSession 之间缓存数据区域互不影响。

  一级缓存的作用域是 SqlSession 范围的，在同一个 SqlSession 中执行两次相同的 SQL 语句时，第一次执行完毕会将结果保存在缓存中，第二次查询直接从缓存中获取。

  如果 SqlSession 执行了 DML 操作（insert、update、delete），Mybatis 必须将缓存清空以保证数据的有效性。 

- **二级缓存**

  Mapper 级别，默认关闭。

  使用二级缓存时多个 SqlSession 使用同一个 Mapper 的 SQL 语句操作数据库，得到的数据会存在二级缓存区，同样使用 HashMap 进行数据存储，相比于一级缓存，二级缓存范围更大，多个 SqlSession 可以共用二级缓存，作用域是 Mapper 的同一个 namespace，不同 SqlSession 两次执行相同的 namespace 下的 SQL 语句，参数也相等，则第一次执行成功后会将数据保存在二级缓存中，第二次可直接从二级缓存中取出数据。

  要使用二级缓存，先在在全局配置文件中配置：

  ```xml
  <!-- 开启二级缓存 -->
  <setting name="cacheEnabled" value="true"/>
  ```

  再在对应的映射文件中配置一个 cache 标签即可。

  ```xml
  <cache/>
  ```

---

### P10：Spring Cloud 框架

**单体应用存在的问题**

随着业务发展，开发越来越复杂。

修改、新增某个功能，需要对整个系统进行测试、重新部署。

一个模块出现问题，可能导致整个系统崩溃。

多个开发团队同时对数据进行管理，容易产生安全漏洞。

各个模块使用同一种技术开发，各个模块很难根据实际情况选择更合适的技术框架，局限性很大。

**分布式和集群的区别**

集群：一台服务器无法负荷高并发的数据访问量，就设置多台服务器一起分担压力，是在物理层面解决问题。

分布式：将一个复杂的问题拆分成若干简单的小问题，将一个大型的项目架构拆分成若干个微服务来协同完成，在软件设计层面解决问题。

**微服务的优点**

各个服务的开发、测试、部署都相互独立，用户服务可以拆分为独立服务，如果用户量很大，可以很容易对其实现负载。

当新需求出现时，使用微服务不再需要考虑各方面的问题，例如兼容性、影响度等。

使用微服务拆分项目后，各个服务之间消除了很多限制，只需要保证对外提供的接口正常可用，而不限制语言和框架等选择。

**服务治理 Eureka**

服务治理的核心由三部分组成：**服务提供者**、**服务消费者**、**注册中心**。

服务注册：在分布式系统架构中，每个微服务在启动时，将自己的信息存储在注册中心。

服务发现：服务消费者从注册中心获取服务提供者的网络信息，通过该信息调用服务。

Spring Cloud 的服务治理使用 Eureka 实现，Eureka 是 Netflix 开源的基于 REST 的服务治理解决方案，Spring Cloud 集成了 Eureka，提供服务注册和服务发现的功能，可以和基于 Spring Boot 搭建的微服务应用轻松完成整合，将 Eureka 二次封装为 Spring Cloud Eureka。**Eureka Server** 是注册中心，所有要进行注册的微服务通过 Eureka Client 连接到 Eureka Server 完成注册。

**服务网关 Zuul**

Spring Cloud 集成了 Zuul 组件，实现服务网关。Zuul 是 Netflix 提供的一个开源的 API 网关服务器，是客户端和网站后端所有请求的中间层，对外开放一个 API，将所有请求导入统一的入口，屏蔽了服务端的具体实现逻辑，可以实现方向代理功能，在网关内部实现动态路由、身份认证、IP过滤、数据监控等。

**负载均衡 Ribbon**

Spring Cloud Ribbon 是一个负载均衡的解决方案，Ribbon 是 Netflix 发布的均衡负载器，Spring Cloud Ribbon是基于 Netflix Ribbon 实现的，是一个用于对 HTTP 请求进行控制的负载均衡客户端。

在注册中心对 Ribbon 进行注册之后，Ribbon 就可以基于某种负载均衡算法（轮循、随机、加权轮询、加权随机等）自动帮助服务消费者调用接口，开发者也可以根据具体需求自定义 Ribbon 负载均衡算法。实际开发中 Spring Clooud Ribbon 需要结合 Spring Cloud Eureka 使用，Eureka 提供所有可以调用的服务提供者列表，Ribbon 基于特定的负载均衡算法从这些服务提供者中选择要调用的具体实例。

**声明式接口调用 Feign**

Feign 与 Ribbon 一样也是 Netflix 提供的，Feign 是一个声明式、模板化的 Web Service 客户端，简化了开发者编写 Web 服务客户端的操作，开发者可以通过简单的接口和注解来调用 HTTP API，Spring Cloud Feign 整合了 Ribbon 和 Hystrix，具有可插拔、基于注解、负载均衡、服务熔断等一系列功能。

相比于 Ribbon + RestTemplate 的方式，Feign 可以大大简化代码开发，支持多种注解，包括 Feign 注解、JAX-RS 注解、Spring MVC 注解等。RestTemplate 是 Spring 框架提供的基于 REST 的服务组件，底层是对 HTTP 请求及响应进行了封装，提供了很多访问 REST 服务的方法，可以简化代码开发。

**服务熔断 Hystrix**

熔断器的作用是在不改变各个微服务调用关系的前提下，针对错误情况进行预先处理。

设计原则：服务隔离机制、服务降级机制、熔断机制、提供实时监控和报警功能和提供实时配置修改功能

Hystrix 数据监控需要结合 `Spring Boot Actuator` 使用，Actuator 提供了对服务的数据监控、数据统计，可以通过 `hystirx-stream` 节点获取监控的请求数据，同时提供了可视化监控界面。

**服务配置 Config**

Spring Cloud Config 通过服务端可以为多个客户端提供配置服务，既可以将配置文件存储在本地，也可以将配置文件存储在远程的 Git 仓库，创建 Config Server，通过它管理所有的配置文件。

**服务跟踪 Zipkin**

Spring Cloud Zipkin 是一个可以采集并跟踪分布式系统中请求数据的组件，让开发者更直观地监控到请求在各个微服务耗费的时间，Zipkin 包括两部分 Zipkin Server 和 Zipkin Client。

---

## MySQL 15

### P1：逻辑架构

第一层是服务器层，主要提供连接处理、授权认证、安全等功能，该层的服务不是 MySQL 独有的，大多数基于网络的 C/S 服务都有类似架构。

第二层实现了 MySQL 核心服务功能，包括查询解析、分析、优化、缓存以及日期和时间等所有内置函数，所有跨存储引擎的功能都在这一层实现，例如存储过程、触发器、视图等。

第三层是存储引擎层，存储引擎负责 MySQL 中数据的存储和提取。服务器通过 API 与存储引擎通信，这些接口屏蔽了不同存储引擎的差异，使得差异对上层查询过程透明。除了会解析外键定义的 InnoDB 外，存储引擎不会解析 SQL，不同存储引擎之间也不会相互通信，只是简单响应上层服务器请求。

---

### P2：锁

当有多个查询需要在同一时刻修改数据时就会产生并发控制的问题，MySQL 在两个层面进行并发控制：服务器层与存储引擎层。

**读写锁**

在处理并发读或写时，可以通过实现一个由两种类型组成的锁系统来解决问题。这两种类型的锁通常被称为共享锁和排它锁，也叫读锁和写锁。读锁是共享的，相互不阻塞，多个客户在同一时刻可以同时读取同一个资源而不相互干扰。写锁则是排他的，也就是说一个写锁会阻塞其他的写锁和读锁，确保在给定时间内只有一个用户能执行写入并防止其他用户读取正在写入的同一资源。

在实际的数据库系统中，每时每刻都在发生锁定，当某个用户在修改某一部分数据时，MySQL 会通过锁定防止其他用户读取同一数据。写锁比读锁有更高的优先级，一个写锁请求可能会被插入到读锁队列的前面，但是读锁不能插入到写锁前面。

**锁策略**

一种提高共享资源并发性的方法就是让锁定对象更有选择性，尽量只锁定需要修改的部分数据而不是所有资源，更理想的方式是只对会修改的数据进行精确锁定。任何时刻在给定的资源上，锁定的数据量越少，系统的并发程度就越高，只要不发生冲突即可。

锁策略就是在锁的开销和数据安全性之间寻求平衡，这种平衡也会影响性能。大多数商业数据库系统没有提供更多选择，一般都是在表上加行锁，而 MySQL 提供了多种选择，每种MySQL存储引擎都可以实现自己的锁策略和锁粒度。MySQL 最重要的两种锁策略：

- 表锁是MySQL中最基本的锁策略，并且是开销最小的策略。表锁会锁定整张表，一个用户在对表进行写操作前需要先获得写锁，这会阻塞其他用户对该表的所有读写操作。只有没有写锁时，其他读取的用户才能获取读锁，读锁之间不相互阻塞。

- 行锁可以最大程度地支持并发处理，同时也带来了最大的锁开销。InnoDB 和 XtraDB 以及一些其他存储引擎实现了行锁。行锁只在存储引擎层实现，而服务器层没有实现。


**死锁**

死锁是指两个或者多个事务在同一资源上相互占用并请求锁定对方占用的资源，从而导致恶性循环的现象。当多个事务试图以不同顺序锁定资源时就可能会产生死锁，多个事务同时锁定同一个资源时也会产生死锁。

为了解决死锁问题，数据库系统实现了各种死锁检测和死锁超时机制。越复杂的系统，例如InnoDB 存储引擎，越能检测到死锁的循环依赖，并立即返回一个错误。这种解决方式很有效，否则死锁会导致出现非常慢的查询。还有一种解决方法，就是当查询的时间达到锁等待超时的设定后放弃锁请求，这种方式通常来说不太好。InnoDB 目前处理死锁的方法是将持有最少行级排它锁的事务进行回滚。

锁的行为与顺序是和存储引擎相关的，以同样的顺序执行语句，有些存储引擎会产生死锁有些则不会。死锁的产生有双重原因：有些是真正的数据冲突，这种情况很难避免，有些则完全是由于存储引擎的实现方式导致的。

死锁发生之后，只有部分或者完全回滚其中一个事务，才能打破死锁。对于事务型系统这是无法避免的，所以应用程序在设计时必须考虑如何处理死锁。大多数情况下只需要重新执行因死锁回滚的事务即可。

---

### <font color = 'red'>P3：事务</font>

事务就是一组原子性的 SQL 查询，或者说一个独立的工作单元。如果数据库引擎能够成功地对数据库应用该组查询的全部语句，那么就执行该组查询。如果其中有任何一条语句因为崩溃或其他原因无法执行，那么所有的语句都不会执行。也就是说事务内的语句要么全部执行成功，要么全部执行失败。

**ACID 特性**

一个运行良好的事务处理系统必须具备 ACID 特性，实现了 ACID 的数据库需要更强的CPU处理能力、更大的内存和磁盘空间。

- **原子性 atomicity**

  一个事务在逻辑上是必须不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说不可能只执行其中的一部分。

- **一致性 consistency**

  数据库总是从一个一致性的状态转换到另一个一致性的状态。

- **隔离性 isolation**

  针对并发事务而言，隔离性就是要隔离并发运行的多个事务之间的相互影响，一般来说一个事务所做的修改在最终提交以前，对其他事务是不可见的。

- **持久性 durability**

  一旦事务提交成功，其修改就会永久保存到数据库中，此时即使系统崩溃，修改的数据也不会丢失。

**隔离级别**

在 SQL 标准中定义了四种隔离级别，每一种隔离级别都规定了一个事务中所做的修改，哪些在事务内和事务间是可见的，哪些是不可见的。较低级别的隔离通常可以执行更高的并发，系统的开销也更低。

- **未提交读 READ UNCOMMITTED**

  在该级别事务中的修改即使没有被提交，对其他事务也是可见的。事务可以读取其他事务修改完但未提交的数据，这种问题称为脏读。这个级别还会导致不可重复读和幻读，从性能上说也没有比其他级别好很多，因此很少使用。

- **提交读 READ COMMITTED**

  大多数数据库系统默认的隔离级别就是提交读，但 MySQL 不是。提交读满足了隔离性的简单定义：一个事务开始时只能"看见"已经提交的事务所做的修改。换句话说，一个事务从开始直到提交之前的任何修改对其他事务都是不可见的。这个级别有时也叫不可重复读，因为两次执行同样的查询可能会得到不同结果。提交读存在不可重复读和幻读的问题。

- **可重复读 REPEATABLE READ**（MySQL默认的隔离级别）

  可重复读解决了不可重复读的问题，该级别保证了在同一个事务中多次读取同样的记录结果是一致的。但可重复读隔离级别还是无法解决幻读的问题，所谓幻读，指的是当某个事务在读取某个范围内的记录时，会产生幻行。InnoDB 存储引擎通过多版本并发控制MVCC 解决幻读的问题。

- **可串行化 SERIALIZABLE**

  该级别是最高的隔离级别，通过强制事务串行执行，避免了幻读的问题。可串行化会在读取的每一行数据上都加锁，可能导致大量的超时和锁争用的问题。实际应用中很少用到这个隔离级别，只有非常需要确保数据一致性且可以接受没有并发的情况下才考虑该级别。

**MySQL 中的事务**

MySQL 提供了两种事务型的存储引擎：InnoDB 和 NDB Cluster。

MySQL 事务默认采用自动提交模式，如果不是显式地开始一个事务，则每个查询都将被当作一个事务执行提交操作。在当前连接中，可以通过设置 AUTOCOMMIT 变量来启用或禁用自动提交模式。

1 或 ON 表示启用，0 或 OFF表示禁用，当禁用自动提交时，所有的查询都是在一个事务中，直到显式地执行 COMMIT 或 ROLLBACK 后该事务才会结束，同时又开始了一个新事务。修改 AUTOCOMMIT 对非事务型表，例如 MyISAM 或内存表不会有任何影响，对这类表来说没有 COMMIT 或 ROLLBACK 的概念，也可以理解为一直处于启用自动提交的模式

有一些命令在执行之前会强制执行提交当前的活动事务，例如`ALTER TABLE`和`LOCK TABLES`等。

MySQL能够识别所有的 4个 ANSI 隔离级别，InnoDB 引擎也支持所有隔离级别。

---

### P4：MVCC 多版本并发控制

可以认为 MVCC 是行级锁的一个变种，但它在很多情况下避免了加锁操作，因此开销更低。虽然实现机制有所不同，但大都实现了非阻塞的读操作，写操作也只锁定必要的行。

MVCC 的实现，是通过保存数据在某个时间点的快照来实现的。也就是说不管需要执行多长时间，每个事务看到的数据都是一致的。根据事务开始的时间不同，每个事务对同一张表，同一时刻看到的数据可能是不一样的。

不同的存储引擎的 MVCC 实现是不同的，典型的有乐观并发控制和悲观并发控制。

**InnoDB 的 MVCC 实现**

InnoDB 的MVCC 通过在每行记录后面保存两个隐藏的列来实现，这两个列一个保存了行的创建时间，一个保存行的过期时间间。不过存储的不是实际的时间值而是系统版本号，每开始一个新的事务系统版本号都会自动递增，事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。

**REPEATABLE READ 级别下 MVCC 的具体实现**

SELECT：InnoDB 会根据以下两个条件检查每行记录：

- 只查找版本早于当前事务版本的数据行，可以确保事务读取的行要么是事务开始前已经存在的，要么是事物自身插入或修改过的。

- 行的删除版本要么未定义，要么大于当前事务版本号，可以确保事务读取到的行在事务开始前未被删除。

INSERT ：为新插入的每一行保存当前系统版本号作为行版本号。

DELETE：为删除的每一行保存当前系统版本号作为行删除标识。

UPDATE：为插入的每一行新记录保存当前系统版本号作为行版本号，同时保存当前系统版本号到原来的行作为行删除标识。

保存这两个额外系统版本号使大多数读操作都可以不用加锁。这样设计使读数据操作简单且高效，并且能保证只会读取到符合标准的行。不足之处是每行记录都需要额外存储空间，需要做更多行检查工作以及一些额外维护工作。

MVCC 只能在 `READ COMMITTED` 和 `REPEATABLE READ` 两个隔离级别下工作，因为 `READ UNCOMMITTED` 总是读取最新的数据行，而不是符合当前事务版本的数据行，而 `SERIALIZABLE` 则会对所有读取的行都加锁。

----

### P5：InnoDB 存储引擎

InnoDB 是 MySQL 的默认事务型引擎，它被设计用来处理大量的短期事务。InnoDB 的性能和自动崩溃恢复特性，使得它在非事务型存储需求中也很流行，除非有特别原因否则应该优先考虑 InnoDB 引擎。

InnoDB 的数据存储在表空间中，表空间由一系列数据文件组成。MySQL4.1 后 InnoDB 可以将每个表的数据和索引放在单独的文件中。

InnoDB 采用 MVCC 来支持高并发，并且实现了四个标准的隔离级别。其默认级别是 `REPEATABLE READ`，并且通过间隙锁策略防止幻读，间隙锁使 InnoDB 不仅仅锁定查询涉及的行，还会对索引中的间隙进行锁定防止幻行的插入。

InnoDB 表是基于聚簇索引建立的，InnoDB 的索引结构和其他存储引擎有很大不同，聚簇索引对主键查询有很高的性能，不过它的二级索引中必须包含主键列，所以如果主键很大的话其他所有索引都会很大，因此如果表上索引较多的话主键应当尽可能小。

InnoDB 的存储格式是平台独立的，可以将数据和索引文件从一个平台复制到另一个平台。

InnoDB 内部做了很多优化，包括从磁盘读取数据时采用的可预测性预读，能够自动在内存中创建加速读操作的自适应哈希索引，以及能够加速插入操作的插入缓冲区等。

**选择合适的存储引擎**

MySQL5.5 将 InnoDB 作为默认存储引擎，除非需要用到某些 InnoDB 不具备的特性，并且没有其他方法可以代替，否则都应该优先选用InnoDB。

如果应用需要事务支持，那么 InnoDB 是目前最稳定并且经过验证的选择。如果不需要事务并且主要是 SELECT 和 INSERT 操作，那么MyISAM 是不错的选择。相对而言，MyISAM 崩溃后发生损坏的概率要比 InnoDB 大很多而且恢复速度也要慢，因此即使不需要事务支持，也可以选择InnoDB。

如果可以定期地关闭服务器来执行备份，那么备份的因素可以忽略。反之如果需要在线热备份，那么 InnoDB 就是基本的要求。

---

### P6：MyISAM 存储引擎

在 MySQL5.1及之前，MyISAM 是默认的存储引擎，MyISAM 提供了大量的特性，包括全文索引、压缩、空间函数等，但不支持事务和行锁，最大的缺陷就是崩溃后无法安全恢复。对于只读的数据或者表比较小、可以忍受修复操作的情况仍然可以使用 MyISAM。

MyISAM 将表存储在数据文件和索引文件中，分别以 `.MYD` 和 `.MYI` 作为扩展名。MyISAM 表可以包含动态或者静态行，MySQL 会根据表的定义决定行格式。MyISAM 表可以存储的行记录数一般受限于可用磁盘空间或者操作系统中单个文件的最大尺寸。

MyISAM 对整张表进行加锁，读取时会对需要读到的所有表加共享锁，写入时则对表加排它锁。但是在表有读取查询的同时，也支持并发往表中插入新的记录。

对于MyISAM 表，MySQL 可以手动或自动执行检查和修复操作，这里的修复和事务恢复以及崩溃恢复的概念不同。执行表的修复可能导致一些数据丢失，而且修复操作很慢。

对于 MyISAM 表，即使是 BLOB 和 TEXT 等长字段，也可以基于其前 500 个字符创建索引。MyISAM 也支持全文索引，这是一种基于分词创建的索引，可以支持复杂的查询。

创建 MyISAM 表时如果指定了 DELAY_KEY_WRITE 选项，在每次修改执行完成时不会立刻将修改的索引数据写入磁盘，而是会写到内存中的键缓冲区，只有在清理缓冲区或关闭表的时候才会将对应的索引库写入磁盘。这种方式可以极大提升写性能，但在数据库或主机崩溃时会造成索引损坏，需要执行修复。延迟更新索引键的特性可以在全局设置也可以单个表设置。

MyISAM 设计简单，数据以紧密格式存储，所以在某些场景下性能很好。MyISAM 最典型的性能问题还是表锁问题，如果所有的查询长期处于 Locked 状态，那么原因毫无疑问就是表锁。

### P7：Memory 存储引擎

如果需要快速访问数据，并且这些数据不会被修改，重启以后丢失也没有关系，那么使用 Memory 表是非常有用的。Memory 表至少要比 MyISAM 表快一个数量级，因为所有的数据都保存在内存中，不需要进行磁盘 IO，Memory 表的结构在重启以后还会保留，但数据会丢失。

Memory 表适合的场景：查找或者映射表、缓存周期性聚合数据的结果、保存数据分析中产生的中间数据。

Memory 表支持哈希索引，因此查找速度极快。虽然速度很快但还是无法取代传统的基于磁盘的表，Memory 表使用表级锁，因此并发写入的性能较低。它不支持 BLOB 和 TEXT 类型的列，并且每行的长度是固定的，所以即使指定了 VARCHAR 列，实际存储时也会转换成CHAR，这可能导致部分内存的浪费。

如果 MySQL 在执行查询的过程中需要使用临时表来保持中间结果，内部使用的临时表就是 Memory 表。如果中间结果太大超出了Memory 表的限制，或者含有 BLOB 或 TEXT 字段，临时表会转换成 MyISAM 表。

----

### P8：数据类型

**整数类型**

如果存储整数可以使用这几种整数类型：TINYINT、SMALLINT、MEDIUMINT、INT，BIGINT，它们分别使用8、16、24、32、64 位存储空间。

整数类型有可选的 UNSIGNED 属性，表示不允许负值，可以使整数的上限提高一倍。有符号和无符号类型使用相同的存储空间并具有相同的性能，可以根据实际情况选择合适的类型。

MySQL 可以为整数类型指定宽度，例如 INT(11)，这对大多数应用没有意义，不会限制值的范围，只是规定了 MySQL 的交互工具显示字符的个数，对于存储和计算来说 INT(1) 和 INT(11) 是相同的。

**实数类型**

实数是带有小数部分的数字，但它们不只是为了存储小数，也可以使用 DECIMAL 存储比 BIGINT 还大的整数。MySQL既支持精确类型，也支持不精确类型。

FLOAT 和 DOUBLE 支持使用标准的浮点运算进行近似运算，DECIMAL 用于存储精确的小数。

浮点类型在存储同样范围的值时，通常比 DECIMAL 使用更少的空间。FLOAT 使用 4 字节存储，DOUBLE 占用8字节，MySQL 内部使用DOUBLE 作为内部浮点计算的类型。

因为需要额外空间和计算开销，所以应当尽量只在对小数进行精确计算时才使用 DECIMAL。在数据量较大时可以考虑 BIGINT 代替DECIMAL，将需要存储的货币单位根据小数的位数乘以相应的倍数即可。假设要存储的数据精确到万分之一分，则可以把所有金额乘以一百万将结果存储在 BIGINT 中，这样可以同时避免浮点存储计算不精确和 DECIMAL 精确计算代价高的问题。

**VARCHAR**

VARCHAR 用于存储可变字符串，是最常见的字符串数据类型。它比定长字符串更节省空间，因为它仅使用必要的空间。VARCHAR 需要 1或 2 个额外字节记录字符串长度，如果列的最大长度不大于 255 字节则只需要1 字节。VARCHAR 不会删除末尾空格。

VARCHAR 节省了存储空间，但由于行是变长的，在 UPDATE 时可能使行变得比原来更长，这就导致需要做额外的工作。如果一个行占用的空间增长并且页内没有更多的空间可以存储，这种情况下不同存储引擎处理不同，InnoDB 会分裂页而 MyISAM 会将行拆分成不同片。

适用场景：字符串列的最大长度比平均长度大很多、列的更新很少、使用了 UTF8 这种复杂字符集，每个字符都使用不同的字节数存储。

InnoDB 可以把过长的 VARCHAR 存储为 BLOB。

**CHAR**

CHAR 是定长的，根据定义的字符串长度分配足够的空间。CHAR 会删除末尾空格。

CHAR 适合存储很短的字符串，或所有值都接近同一个长度，例如存储密码的 MD5 值。对于经常变更的数据，CHAR 也比 VARCHAR更好，因为定长的 CHAR 不容易产生碎片。对于非常短的列，CHAR 在存储空间上也更有效率，例如用 CHAR 来存储只有 Y 和 N 的值只需要一个字节，但是 VARCHAR 需要两个字节，因为还有一个记录长度的额外字节。

**BLOB 和 TEXT 类型**

BLOB 和TEXT 都是为了存储大数据而设计的字符串数据类型，分别采用二进制和字符串方式存储。MySQL会把每个 BLOB 和 TEXT 值当作一个独立的对象处理，存储引擎在存储时通常会做特殊处理。当值太大时，InnoDB 会使用专门的外部存储区来进行存储。BLOB 和TEXT 仅有的不同是 BLOB 存储的是二进制数据，没有排序规则或字符集，而 TEXT 有字符集和排序规则。

MySQL 对 BLOB 和TEXT 列进行排序与其他类型不同：它只对每个列最前 `max_sort_length` 字节而不是整个字符串做排序，如果只需要排序前面一小部分字符，则可以减小 `max_sort_length` 的配置。MySQL 不能将 BLOB 和 TEXT 列全部长度的字符串进行索引，也不能使用这些索引消除排序。

**DATETIME**

这个类型能保存大范围的值，从 1001 年到 9999 年，精度为秒。它把日期和时间封装到了一个整数中，与时区无关，使用 8 字节的存储空间。

**TIMESTAMP**

它和 UNIX 时间戳相同。TIMESTAMP 只使用 4 字节的存储空间，因此它的范围比DATETIME 小得多，只能表示1970年到2038年，并且依赖于时区。通常应该选择 TIMESTAMP，因为它比 DATETIME 空间效率更高。

----

### P9：索引的分类

索引在也叫做键，是存储引擎用于快速找到记录的一种数据结构。索引对于良好的性能很关键，尤其是当表中数据量越来越大时，索引对性能的影响愈发重要。在数据量较小且负载较低时，不恰当的索引对性能的影响可能还不明显，但数据量逐渐增大时，性能会急剧下降。

索引大大减少了服务器需要扫描的数据量、可以帮助服务器避免排序和临时表、可以将随机 IO 变成顺序 IO。但索引并不总是最好的工具，对于非常小的表，大部分情况下会采用全表扫描。对于中到大型的表，索引就非常有效。但对于特大型的表，建立和使用索引的代价也随之增长，这种情况下应该使用分区技术。

在MySQL中，首先在索引中找到对应的值，然后根据匹配的索引记录找到对应的数据行。索引可以包括一个或多个列的值，如果索引包含多个列，那么列的顺序也十分重要，因为 MySQL 只能高效地使用索引的最左前缀列。

**B-Tree 索引**

大多数 MySQL 引擎都支持这种索引，使用术语 B-Tree 是因为 MySQL 在 CREATE TABLE 和其他语句中也使用该关键字。不过底层的存储引擎可能使用不同的存储结构，例如 NDB 集群实际使用 T-Tree，而 InnoDB 则使用 B+Tree。

存储引擎以不同方式使用 B-Tree 索引，性能也不同。例如 MyISAM 使用前缀压缩技术使得索引更小，但 InnoDB 则按照原数据格式进行存储。再例如 MyISAM 索引通过数据的物理位置引用被索引的行，而 InnoDB 则根据主键引用被索引的行。

B-Tree 通常意味着所有的值都是按顺序存储的，并且每个叶子页到根的距离相同。B-Tree 索引能够加快访问数据的速度，因为存储引擎不再需要进行全表扫描来获取需要的数据，取而代之的是从索引的根节点开始进行搜索。根节点的槽中存放了指向子节点的指针，存储引擎根据这些指针向下层查找。通过比较节点页的值和要查找的值可以找到合适的指针进入下层子节点，这些指针实际上定义了子节点页中值的上限和下限。最终存储引擎要么找到对应的值，要么该记录不存在。叶子节点的指针指向的是被索引的数据，而不是其他的节点页。

B-Tree索引适用于全键值、键值范围或键前缀查找，其中键前缀查找只适用于最左前缀查找。索引对如下类型的查询有效：

- 全值匹配：全值匹配指的是和索引中的所有列进行匹配。
- 匹配最左前缀：只使用索引的第一列。
- 匹配列前缀：只匹配某一列的值的开头部分。
- 匹配范围值：查找某两个值之间的范围。
- 精确匹配某一列并范围匹配另一列：有一列全匹配而另一列范围匹配。
- 只访问索引的查询：B-Tree 通常可以支持只访问索引的查询，即查询只需要访问索引而无需访问数据行。

因为索引树中的节点有序，所以除了按值查找之外索引还可以用于查询中的 ORDER BY 操作。一般如果 B-Tree 可以按照某种方式查找到值，那么也可以按照这种方式排序。

B-Tree索引的限制：

- 如果不是按照索引的最左列开始查找，则无法使用索引。
- 不能跳过索引中的列，例如索引为 (id,name,sex)，不能只使用 id 和 sex 而跳过 name。
- 如果查询中有某个列的范围查询，则其右边的所有列都无法使用索引。

**哈希索引**

哈希索引基于哈希表实现，只有精确匹配索引所有列的查询才有效。对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码，哈希码是一个较小的值，并且不同键值的行计算出的哈希码也不一样。哈希索引将所有的哈希码存储在索引中，同时在哈希表中保存指向每个数据行的指针。

只有 Memory 引擎显式支持哈希索引，这也是 Memory 引擎的默认索引类型。

因为索引自身只需存储对应的哈希值，所以索引的结构十分紧凑，这让哈希索引的速度非常快，但它也有一些限制：

- 哈希索引只包含哈希值和行指针而不存储字段值，所以不能使用索引中的值来避免读取行。
- 哈希索引数据并不是按照索引值顺序存储的，因此无法用于排序。
- 哈希索引不支持部分索引列匹配查找，因为哈希索引始终是使用索引列的全部内容来计算哈希值的。例如在数据列(a,b)上建立哈希索引，如果查询的列只有a就无法使用该索引。
- 哈希索引只支持等值比较查询，不支持任何范围查询。
- 访问哈希索引的数据非常快，除非有很多哈希冲突。当出现哈希冲突时，存储引擎必须遍历链表中所有的行指针，逐行进行比较直到找到所有符合条件的行。
- 如果哈希冲突很高的话，索引维护的代价也会很高。

自适应哈希索引是 InnoDB 引擎的一个特殊功能，当它注意到某些索引值被使用的非常频繁时，会在内存中基于 B-Tree 索引之上再创键一个哈希索引，这样就让 B-Tree 索引也具有哈希索引的一些优点，比如快速哈希查找。这是一个完全自动的内部行为，用户无法控制或配置，但如果有必要可以关闭该功能。

如果存储引擎不支持哈希索引，可以创建自定义哈希索引，在 B-Tree基础 上创建一个伪哈希索引，它使用哈希值而不是键本身进行索引查找，需要在查询的 WHERE 子句中手动指定哈希函数。当数据表非常大时，CRC32 会出现大量的哈希冲突，可以考虑自己实现 64 位哈希函数，或者使用 MD5 函数返回值的一部分作为自定义哈希函数。

**空间索引**

MyISAM 表支持空间索引，可以用作地理数据存储。和 B-Tree 索引不同，这类索引无需前缀查询。空间索引会从所有维度来索引数据，查询时可以有效地使用任意维度来组合查询。必须使用 MySQL 的 GIS 即地理信息系统的相关函数来维护数据，但 MySQL 对 GIS 的支持并不完善，因此大部分人都不会使用这个特性。

**全文索引**

通过数值比较、范围过滤等就可以完成绝大多数需要的查询，但如果希望通过关键字的匹配进行查询过滤，那么就需要基于相似度的查询，而不是精确的数值比较，全文索引就是为这种场景设计的。全文索引有自己独特的语法，没有索引也可以工作，如果有索引效率会更高。

全文索引可以支持各种字符内容的搜索，包括 CHAR、VARCHAR 和 TEXT 类型，也支持自然语言搜索和布尔搜索。在 MySQL 中全文索引有很多限制，例如表锁对性能的影响、数据文件的崩溃恢复等，这使得 MyISAM 的全文索引对很多应用场景并不合适。MyISAM 的全文索引作用对象是一个"全文集合"，可能是某个数据表的一列，也可能是多个列。具体的对某一条记录，MySQL 会将需要索引的列全部拼接成一个字符串然后进行索引。

MyISAM 的全文索引是一种特殊的 B-Tree 索引，一共有两层。第一层是所有关键字，然后对于每一个关键字的第二层，包含的是一组相关的"文档指针"。全文索引不会索引文档对象中的所有词语，它会根据规则过滤掉一些词语，例如停用词列表中的词都不会被索引。

**聚簇索引**

聚簇索引并不是一种单独的索引类型，而是一种数据存储方式。InnoDB 的聚簇索引实际上在同一个结构中保存了 B-Tree 索引和数据行。当表有聚餐索引时，它的行数据实际上存放在索引的叶子页中，因为无法同时把数据行存放在两个不同的地方，所以一个表只能有一个聚簇索引。

优点：① 可以把相关数据保存在一起，例如实现电子邮箱时可以根据用户 ID 聚集数据，这样只需要从磁盘读取少数数据页就能获取某个用户的全部邮件，如果没有使用聚簇索引，每封邮件可能都导致一次磁盘 IO。② 数据访问更快，聚簇索引将索引和数据保存在同一个 B-Tree 中，因此获取数据比非聚簇索引要更快。③ 使用覆盖索引扫描的查询可以直接使用页节点中的主键值。

缺点：① 聚簇索引最大限度提高了 IO 密集型应用的性能，如果数据全部在内存中将会失去优势。② 插入速度验证依赖于插入顺序，按照主键的顺序插入是加载数据到 InnoDB 引擎最快的方式。③ 更新聚簇索引列的代价很高，因为会强制每个被更新的行移动到新位置。④ 基于聚簇索引的表插入新行或主键被更新导致行移动时，可能导致页分裂，表会占用更多磁盘空间。④ 当行稀疏或由于页分裂导致数据存储不连续时，全表扫描可能很慢。

**覆盖索引**

覆盖索引指一个索引包含或覆盖了所有需要查询的字段的值，不再需要根据索引回表查询数据。覆盖索引必须要存储索引列的值，因此 MySQL 只能使用 B-Tree 索引做覆盖索引。

优点：① 索引条目通常远小于数据行大小，可以极大减少数据访问量。② 因为索引按照列值顺序存储，所以对于 IO 密集型防伪查询回避随机从磁盘读取每一行数据的 IO 少得多。③ 由于 InnoDB 使用聚簇索引，覆盖索引对 InnoDB 很有帮助。InnoDB 的二级索引在叶子节点保存了行的主键值，如果二级主键能覆盖查询那么可以避免对主键索引的二次查询。

----

### <font color = 'red'>P10：索引使用原则</font>

**建立索引**

对查询频次较高，且数据量比较大的表建立索引。索引字段的选择，最佳候选列应当从 WHERE 子句的条件中提取，如果 WHERE 子句中的组合比较多，那么应当挑选最常用、过滤效果最好的列的组合。

**使用前缀索引**

索引列开始的部分字符，索引创建后也是使用硬盘来存储的，因此短索引可以提升索引访问的 IO 效率。对于 BLOB、TEXT 或很长的 VARCHAR 列必须使用前缀索引，MySQL 不允许索引这些列的完整长度。前缀索引是一种能使索引更小更快的有效方法，但缺点是 MySQL 无法使用前缀索引做 ORDER BY 和 GROUP BY，也无法使用前缀索引做覆盖扫描。

**选择合适的索引顺序**

当不需要考虑排序和分组时，将选择性最高的列放在前面。索引的选择性是指不重复的索引值和数据表的记录总数之比，索引的选择性越高则查询效率越高，唯一索引的选择性是 1，因此也可以使用唯一索引提升查询效率。

**删除无用索引**

MySQL 允许在相同列上创建多个索引，重复的索引需要单独维护，并且优化器在优化查询时也需要逐个考虑，这会影响性能。重复索引是指在相同的列上按照相同的顺序创建的相同类型的索引，应该避免创建重复索引。如果创建了索引 (A,B) 再创建索引 (A) 就是冗余索引，因为这只是前一个索引的前缀索引，对于 B-Tree 索引来说是冗余的。解决重复索引和冗余索引的方法就是删除这些索引。除了重复索引和冗余索引，可能还会有一些服务器永远不用的索引，也应该考虑删除。

**减少碎片**

B-Tree 索引可能会碎片化，碎片化的索引可能会以很差或无序的方式存储在磁盘上，这会降低查询的效率。表的数据存储也可能碎片化，包括行碎片、行间碎片、剩余空间碎片，对于 MyISAM 这三类碎片化都有可能发生，对于 InnoDB 不会出现短小的行碎片，它会移动短小的行重写到一个片段中。可以通过执行 OPTIMIZE TABLE 或者导出再导入的方式重新整理数据，对于 MyISAM 可以通过排序重建索引消除碎片。InnoDB 可以通过先删除再重新创建索引的方式消除索引碎片。

**索引失效情况**

如果索引列出现了隐式类型转换，则 MySQL 不会使用索引。常见的情况是在 SQL 的 WHERE 条件中字段类型为字符串，其值为数值，如果没有加引号那么 MySQL 不会使用索引。

如果 WHERE 条件中含有 OR，除非 OR 前使用了索引列而 OR 之后是非索引列，索引会失效。

MySQL 不能在索引中执行 LIKE 操作，这是底层存储引擎 API 的限制，最左匹配的 LIKE 比较会被转换为简单的比较操作，但如果是以通配符开头的 LIKE 查询，存储引擎就无法做笔记。这种情况下 MySQL 服务器只能提取数据行的值而不是索引值来做比较。

如果查询中的列不是独立的，则 MySQL 不会使用索引。独立的列是指索引列不能是表达式的一部分，也不能是函数的参数。

对于多个范围条件查询，MySQL 无法使用第一个范围列后面的其他索引列，对于多个等值查询则没有这种限制。

如果 MySQL 判断全表扫描比使用索引查询更快，则不会使用索引。

----

### P11：优化数据类型

**更小的通常更好**

一般情况下尽量使用可以正确存储数据的最小数据类型，更小的数据类型通常也更快，因为它们占用更少的磁盘、内存和 CPU 缓存。

**尽可能简单**

简单数据类型的操作通常需要更少的 CPU 周期，例如整数比字符操作代价更低，因为字符集和校对规则使字符相比整形更复杂。应该使用 MySQL 的内建类型 date、time 和 datetime 而不是字符串来存储日期和时间，另一点是应该使用整形存储 IP 地址。

**尽量避免 NULL**

通常情况下最好指定列为 NOT NULL，除非需要存储 NULL值。因为如果查询中包含可为 NULL 的列对 MySQL 来说更难优化，可为 NULL 的列使索引、索引统计和值比较都更复杂，并且会使用更多存储空间。当可为 NULL 的列被索引时，每个索引记录需要一个额外字节，在MyISAM 中还可能导致固定大小的索引变成可变大小的索引。

通常把可为 NULL 的列设置为 NOT NULL 带来的性能提升较小，因此调优时没必要首先查找并修改这种情况。但如果计划在列上建索引，就应该尽量避免设计成可为 NULL 的列。

在为列选择数据类型时，第一步需要确定合适的大类型：数字、字符串、时间等。下一步是选择具体类型，很多 MySQL 数据类型可以存储相同类型的数据，只是存储的长度和范围不一样，允许的精度不同或需要的物理空间不同。

----

### P12：优化查询概述

**优化数据访问**

如果把查询看作一个任务，那么它由一系列子任务组成，每个子任务都会消耗一定时间。如果要优化查询，要么消除一些子任务，要么减少子任务的执行次数。查询性能低下最基本的原因是访问的数据太多，大部分性能低下的查询都可以通过减少访问的数据量进行优化。可以通过以下两个步骤分析。

是否向数据库请求了不需要的数据：有些查询会请求超过实际需要的数据，然后这些多余的数据会被应用程序丢弃，这会给 MySQL 服务器造成额外负担并增加网络开销，另外也会消耗应用服务器的 CPU 和内存资源。例如多表关联时返回全部列，取出全部列会让优化器无法完成索引覆盖扫描这类优化，还会为服务器带来额外的 IO、内存和 CPU 的消耗，因此使用 SELECT * 时需要仔细考虑是否真的需要返回全部列。再例如总是重复查询相同的数据，比较好的解决方案是初次查询时将数据缓存起来，需要的时候从缓存中取出。

MySQL 是否在扫描额外的记录：在确定查询只返回需要的数据后，应该看看查询为了返回结果是否扫描了过多的数据，最简单的三个衡量指标时响应时间、扫描的行数和返回的行数。如果发现查询需要扫描大量数据但只返回少数的行，可以使用以下手动优化：① 使用覆盖索引扫描，把所有需要用的列都放到索引中，这样存储引擎无需回表查询对应行就可以返回结果。② 改变库表结构。 ③ 重写这个复杂的查询，让 MySQL 优化器能够以更优化的方式执行这个查询。

**重构查询方式**

在优化有问题的查询时，目标应该是找到一个更优的方法获取实际需要的结果，而不一定总是需要从 MySQL 获取一模一样的结果集。

切分查询：有时候对于一个大查询可以将其切分成小查询，每个查询功能完全一样，只完成一小部分，每次只返回一小部分查询结果。例如删除旧数据，定期清除大量数据时，如果用一个大的语句一次性完成的话可能需要一次锁住很多数据、占满整个事务日志、耗尽系统资源、阻塞很多小的但重要的查询。将一个大的 DELETE 语句切分成多个较小的查询可以尽可能小地影响 MySQL 的性能，同时还可以减少MySQL 复制的延迟。

分解关联查询：很多高性能应用都会对关联查询进行分解，可以对每一个表进行单表查询，然后将结果在应用程序中进行关联。分解关联查询可以让缓存的效率更高、减少锁的竞争、提升查询效率、还可以减少冗余记录的查询。

----

### P13：查询执行流程

简单来说分为五步：① 客户端发送一条查询给服务器。② 服务器先检查查询缓存，如果命中了缓存则立刻返回存储在缓存中的结果，否则进入下一阶段。③ 服务器端进行 SQL 解析、预处理，再由优化器生成对应的执行计划。④ MySQL 根据优化器生成的执行计划，调用存储引擎的 API 来执行查询。⑤ 将结果返回给客户端。 

**查询缓存**

在解析一个查询语句之前，如果查询缓存是打开的，那么 MySQL 会优先检查这个查询是否命中查询缓存中的数据。这个检查是通过一个对大小写敏感的哈希查找实现的。查询和缓存中的查询即使只有一个字节不同，也不会匹配缓存结果，这种情况下会进行下一个阶段的处理。如果当前的查询恰好命中了查询缓存，那么在返回查询结果之前 MySQL 会检查一次用户权限。如果权限没有问题，MySQL 会跳过其他阶段，直接从缓冲中拿到结果并返回给客户端，这种情况下查询不会被解析，不用生成执行计划，不会被执行。

**查询优化处理**

该阶段包括多个子阶段：解析 SQL、预处理、优化 SQL 执行计划。首先 MySQL 通过关键字将 SQL 语句进行解析，并生成一颗对应的解析树，MySQL 解析器将使用 MySQL 语法规则验证和解析查询。例如它将验证是否使用了错误的关键字，或者使用关键字的顺序是否正确等。预处理器则根据一些 MySQL 规则进一步检查解析树是否合法，例如检查数据表和数据列是否存在，还会解析名字和别名看它们是否有歧义。下一步预处理器会验证权限，这一步通常很快，除非服务器上有非常多的权限配置。

语法树被认为合法后，查询优化器将其转成执行计划。一条查询可以有多种查询方式，最后都返回相同的结果，优化器的作用就是找到这其中最好的执行计划。MySQL 使用基于成本的优化器，它将尝试预测一个查询使用某种执行计划时的成本，并选择其中成本最小的一个。优化策略可以简单分为两种，一种是静态优化，可以直接对解析树分析并完成优化，不依赖于特别的数值，可以认为是一种编译时优化。另一种是动态优化，和查询的上下文有关，每次查询时都需要重新评估。

MySQL 可以处理的优化类型包括：重新定义表的关联顺序、将外连接转化成内连接、使用等价变换规则、优化 COUNT() 和 MIN() 以及 MAX() 函数、预估并转为常数表达式、覆盖索引扫描、子查询优化等。

**查询执行引擎**

在解析和优化阶段，MySQL 将生成查询对应的执行计划，MySQL 的查询执行引擎则根据这个计划来完成整个查询。执行计划是一个数据结构，而不是其他关系型数据库那样会生成对应的字节码。查询执行阶段并不复杂，MySQL 只是简单的根据执行计划给出的指令逐步执行，再根据执行计划执行的过程中，有大量操作需要通过调用存储引擎实现的接口来完成。

**返回结果给客户端**

查询执行的最后一个阶段是将结果返回给客户端，即使查询不需要返回结果集，MySQL 仍然会返回这个查询的一些信息，如该查询影响到的行数。如果查询可以被缓存，那么 MySQL 会在这个阶段将结果存放到查询缓冲中。MySQL 将结果集返回客户端是一个增量、逐步返回的过程，这样做的好处是服务器无需存储太多的结果，减少内存消耗，也可以让客户端第一时间获得响应结果。结果集中的每一行给都会以一个满足 MySQL 客户端/服务器通信协议的包发送，再通过 TCP 协议进行传输，在 TCP 传输过程中可能对包进行缓存然后批量传输。

----

### <font color = 'red'>P14：优化 SQL</font>

**优化 COUNT 查询**

COUNT 是一个特殊的函数，它可以统计某个列值的数量，在统计列值时要求列值是非空的，不会统计 NULL 值。如果在 COUNT 中指定了列或列的表达式，则统计的就是这个表达式有值的结果数，而不是 NULL。

COUNT 的另一个作用是统计结果集的行数，当 MySQL 确定括号内的表达式不可能为 NULL 时，实际上就是在统计行数。当使用 COUNT(*) 时，\* 不会扩展成所有列，它会忽略所有的列而直接统计所有的行数。

某些业务场景并不要求完全精确的 COUNT 值，此时可以使用近似值来代替，EXPLAIN 出来的优化器估算的行数就是一个不错的近似值，因为执行 EXPLAIN 并不需要真正地执行查询。

通常来说 COUNT 都需要扫描大量的行才能获取精确的结果，因此很难优化。在 MySQL 层还能做的就只有覆盖扫描了，如果还不够就需要修改应用的架构，可以增加汇总表或者外部缓存系统。

**优化关联查询**

确保 ON 或 USING 子句中的列上有索引，在创建索引时就要考虑到关联的顺序。

确保任何 GROUP BY 和 ORDER BY 的表达式只涉及到一个表中的列，这样 MySQL 才有可能使用索引来优化这个过程。

在 MySQL 5.5 及以下版本尽量避免子查询，可以用关联查询代替，因为执行器会先执行外部的 SQL 再执行内部的 SQL。

**优化 GROUP BY**

如果没有通过 ORDER BY 子句显式指定要排序的列，当查询使用 GROUP BY 子句的时候，结果集会自动按照分组的字段进行排序，如果不关心结果集的顺序，可以使用 ORDER BY NULL 禁止排序。

**优化 LIMIT 分页**

在偏移量非常大的时候，需要查询很多条数据再舍弃，这样的代价非常高。要优化这种查询，要么是在页面中限制分页的数量，要么是优化大偏移量的性能。最简单的办法是尽可能地使用覆盖索引扫描，而不是查询所有的列，然后根据需要做一次关联操作再返回所需的列。

还有一种方法是从上一次取数据的位置开始扫描，这样就可以避免使用 OFFSET。其他优化方法还包括使用预先计算的汇总表，或者关联到一个冗余表，冗余表只包含主键列和需要做排序的数据列。

**优化 UNION 查询**

MySQL 通过创建并填充临时表的方式来执行 UNION 查询，除非确实需要服务器消除重复的行，否则一定要使用 UNION ALL，如果没有 ALL 关键字，MySQL 会给临时表加上 DISTINCT 选项，这会导致对整个临时表的数据做唯一性检查，这样做的代价非常高。

**使用用户自定义变量**

在查询中混合使用过程化和关系化逻辑的时候，自定义变量可能会非常有用。用户自定义变量是一个用来存储内容的临时容器，在连接 MySQL 的整个过程中都存在，可以在任何可以使用表达式的地方使用自定义变量。例如可以使用变量来避免重复查询刚刚更新过的数据、统计更新和插入的数量等。

**优化 INSERT** 

需要对一张表插入很多行数据时，应该尽量使用一次性插入多个值的 INSERT 语句，这种方式将缩减客户端与数据库之间的连接、关闭等消耗，效率比多条插入单个值的 INSERT 语句高。也可以关闭事务的自动提交，在插入完数据后提交。当插入的数据是按主键的顺序插入时，效率更高。

----

### P15：复制

复制解决的基本问题是让一台服务器的数据与其他服务器保持同步，一台主库的数据可以同步到多台备库上，备库本身也可以被配置成另外一台服务器的主库。主库和备库之间可以有多种不同的组合方式。

MySQL 支持两种复制方式：基于行的复制和基于语句的复制，基于语句的复制也称为逻辑复制，从 MySQL 3.23 版本就已存在，基于行的复制方式在 5.1 版本才被加进来。这两种方式都是通过在主库上记录二进制日志、在备库重放日志的方式来实现异步的数据复制。因此同一时刻备库的数据可能与主库存在不一致，并且无法包装主备之间的延迟。

MySQL 复制大部分是向后兼容的，新版本的服务器可以作为老版本服务器的备库，但是老版本不能作为新版本服务器的备库，因为它可能无法解析新版本所用的新特性或语法，另外所使用的二进制文件格式也可能不同。

复制解决的问题：数据分布、负载均衡、备份、高可用性和故障切换、MySQL 升级测试。

**复制步骤**

概述：① 在主库上把数据更改记录到二进制日志中。② 备库将主库的日志复制到自己的中继日志中。 ③ 备库读取中继日志中的事件，将其重放到备库数据之上。

第一步是在主库上记录二进制日志，每次准备提交事务完成数据更新前，主库将数据更新的事件记录到二进制日志中。MySQL 会按事务提交的顺序而非每条语句的执行顺序来记录二进制日志，在记录二进制日志后，主库会告诉存储引擎可以提交事务了。

下一步，备库将主库的二进制日志复制到其本地的中继日志中。备库首先会启动一个工作的 IO 线程，IO 线程跟主库建立一个普通的客户端连接，然后在主库上启动一个特殊的二进制转储线程，这个线程会读取主库上二进制日志中的事件。它不会对事件进行轮询。如果该线程追赶上了主库将进入睡眠状态，直到主库发送信号量通知其有新的事件产生时才会被唤醒，备库 IO 线程会将接收到的事件记录到中继日志中。

备库的 SQL 线程执行最后一步，该线程从中继日志中读取事件并在备库执行，从而实现备库数据的更新。当 SQL 线程追赶上 IO 线程时，中继日志通常已经在系统缓存中，所以中继日志的开销很低。SQL 线程执行的时间也可以通过配置选项来决定是否写入其自己的二进制日志中。

这种复制架构实现了获取事件和重放事件的解耦，允许这两个过程异步进行，也就是说 IO 线程能够独立于 SQL 线程工作。但这种架构也限制了复制的过程，在主库上并发允许的查询在备库只能串行化执行，因为只有一个 SQL 线程来重放中继日志中的事件。













