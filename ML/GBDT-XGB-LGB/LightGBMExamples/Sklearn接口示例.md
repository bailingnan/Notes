# Sklearn接口示例
```python
import numpy as np
import pandas as pd
import lightgbm as lgb

from sklearn.metrics import mean_squared_error
from sklearn.model_selection import GridSearchCV
```


## 加载或者创建自己的数据集
```python
print('Loading data...')
df_train = pd.read_csv('./data/regression/regression.train', header=None, sep='\t')
df_test = pd.read_csv('./data/regression/regression.test', header=None, sep='\t')

print(df_train.head())
y_train = df_train[0]
y_test = df_test[0]
X_train = df_train.drop(0, axis=1)
X_test = df_test.drop(0, axis=1)
```

## 训练
```python
print('Starting training...')
gbm = lgb.LGBMRegressor(num_leaves=31,
                        learning_rate=0.05,
                        n_estimators=20)
gbm.fit(X_train, y_train,
        eval_set=[(X_test, y_test)],#一个元素为(X,y) 的列表，或者None。 给出了验证集，用于早停。默认为None。
        eval_metric='l1',
        early_stopping_rounds=5)
```
## 预测
```python
print('Starting predicting...')

y_pred = gbm.predict(X_test, num_iteration=gbm.best_iteration_)#一个整数，指示在预测时，使用多少个子树。默认为0，表示使用所有的子树。
#gbm.best_iteration_:一个字典或者None。当early_stopping_round 参数设定时，它给出了训练完毕模型的最好的迭代步。
```
## 评估
```python
print('The rmse of prediction is:', mean_squared_error(y_test, y_pred) ** 0.5)
```
## 特征重要性
```python
print('Feature importances:', list(gbm.feature_importances_))#一个形状为(n_features,) 的 numpy array，给出了特征的重要性（值越大，则对于的特征越重要）。
```

## 自定义评估指标
`f(y_true: array, y_pred: array) -> name: string, eval_result: float, is_higher_better: bool`
```python
#Root Mean Squared Logarithmic Error (RMSLE)
def rmsle(y_true, y_pred):
    return 'RMSLE', np.sqrt(np.mean(np.power(np.log1p(y_pred) - np.log1p(y_true), 2))), False
```


## 训练
```python
print('Starting training with custom eval function...')
gbm.fit(X_train, y_train,
        eval_set=[(X_test, y_test)],
        eval_metric=rmsle,
        early_stopping_rounds=5)
```

## 另一个自定义评估指标
`f(y_true: array, y_pred: array) -> name: string, eval_result: float, is_higher_better: bool`
```python
#Relative Absolute Error (RAE)
def rae(y_true, y_pred):
    return 'RAE', np.sum(np.abs(y_pred - y_true)) / np.sum(np.abs(np.mean(y_true) - y_true)), False
```


## 训练
```python
print('Starting training with multiple custom eval functions...')
gbm.fit(X_train, y_train,
        eval_set=[(X_test, y_test)],
        eval_metric=lambda y_true, y_pred: [rmsle(y_true, y_pred), rae(y_true, y_pred)],
        early_stopping_rounds=5)
```

## 预测
```python
print('Starting predicting...')
y_pred = gbm.predict(X_test, num_iteration=gbm.best_iteration_)
# eval
print('The rmsle of prediction is:', rmsle(y_test, y_pred)[1])
print('The rae of prediction is:', rae(y_test, y_pred)[1])
```

## 超参数搜索
```python
estimator = lgb.LGBMRegressor(num_leaves=31)

param_grid = {
    'learning_rate': [0.01, 0.1, 1],
    'n_estimators': [20, 40]
}

gbm = GridSearchCV(estimator, param_grid, cv=3)
gbm.fit(X_train, y_train)

print('Best parameters found by grid search are:', gbm.best_params_)
```
