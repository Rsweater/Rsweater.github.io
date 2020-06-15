---
title: Time Series Pandas
tags:
  - Time Series
  - Data Analysis
  - Pandas
categories:
  - Machine Learning
date: 2020-06-04 09:34:15
abbrlink: 38536
---

**摘要：**利用Python进行时间序列分析,  主要记录ARIMA模型相关知识。

<!---more--->



## 生成时间序列

1. pd.date_range()
2. truncate()过滤
3. pd.Timestamp()时间戳
4. pd.Period()时间周期
5. pd.datelta()间隔



## 差分法

​	改进数据平稳性

`.diff(num)`: num阶差分，一般一阶就可以了。



## ARIMA模型

​		**ARIMA模型**（英语：**A**uto**r**egressive **I**ntegrated **M**oving **A**verage model），差分整合移动平均自回归模型，又称整合移动平均自回归模型（移动也可称作滑动），是[时间序列](https://baike.baidu.com/item/时间序列)预测分析方法之一。

​		ARIMA(p，d，q)中，AR是“自回归模型”，p为自回归项数；MA为“滑动平均模型”，q为滑动平均项数，d为使之成为平稳序列所做的差分次数（阶数）。“差分”一词虽未出现在ARIMA的英文名称中，却是关键步骤。

### AR模型

- 描述当前值与历史值之间的关系，用变量自身的历史时间数据对自身进行预测

- p阶自回归过程的公式定义：$y_t = \mu + \sum^{p}_{i=1}{\gamma_iy_{t-i}} + \epsilon_t$, 

- 其中 $y_t$ 是当前值、$\mu$ 是常数项、p 是阶数、 $\gamma_i$ 是自相关系数、 $\epsilon_t$ 是误差

    

- 自回归模型是用自身的数据来进行预测

- 必须具有平稳性

- 必须具有自相关，如果自相关系数小于0.5，则不宜采用

- 自回归只适用于预测与自身前期相关的现象

### MA模型

- 移动平均模型关注的是自回归模型中误差项的累加
- q阶移动平均模型：$y_t = \mu + \epsilon_t + \sum^q_{i=1}{\theta_i\epsilon_{t-i}}$ 
- 移动平均模型能够消除预测中的随机波动



- `ARMA = AR + MA` 、`ARMA + 差分 = ARIMA`

### ARIMA模型原理

​		将非平稳时间序列转化为平稳时间序列，然后将因变量对它的滞后值以及随机误差项的现值和滞后值进行回归所建立的模型

### p、q值的确定

自相关系数ACF、偏相关系数PACF

| 模型       | ACF                               | PACF                              |
| ---------- | --------------------------------- | --------------------------------- |
| AR(p)      | 衰减趋于零（几何型或振荡型）      | p阶后截尾                         |
| MA(q)      | q阶后截尾                         | 衰减趋于零（几何型或振荡型）      |
| ARMA(p, q) | q阶后衰减趋于零（几何型或振荡型） | p阶后衰减趋于零（几何型或振荡型） |

- 截尾：落在置信区间内（95%的点都符合该规则）

- Python方法：

    ```python
    from statsmodels.graphocs.tsaplots import plot_acf, plot_pacf
    ```



## 数据重采样

1. `.resample(freq)` 例如：

```Python
rng = pd.date_range('1/1/2020', periods=90, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)

# 降采样
ts = resample('M').mean()
ts = resmaple('M').sum()
ts = resmaple('3D').sum()
```

2. 升采样插值
   - `.fill(位数)`，向后填充，空值取前面的值
   - `.bfill()`, 向前填充
   - `.interpolate()`, 线性填充



## 滑动窗口

在较长时间区间上，用来观测某个时间点、区间相关情况的方法。

使用`.rolling(windowSize)`



## 额外收获

[pandasdatareade](https://github.com/pydata/pandas-datareader): 直接获取网络上的数据到`dataframe`

[tsfresh](https://github.com/blue-yonder/tsfresh): 提取时间序列的特征



## 相关知识

1. 时间序列分类
2. EDA