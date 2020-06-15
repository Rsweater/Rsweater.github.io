---
title: QG实验1——地理数据统计预处理
author: 一线毛衣
abbrlink: 60550
tags:
  - Excel
  - Python
  - 地理空间分析
  - 数据分析
categories:
  - 计量地理学
date: 2020-04-18 14:35:00
---

## 实验目的

1. 掌握用Excel、Python进行__数据统计描述__的方法；
2. 掌握用Excel、Python进行__数据标准化__的基本方法；
3. 掌握用Excel、Python进行__常用数据可视化__的方法；
4. 掌握用Excel、Python进行__数据连接__和__数据透视__的方法。
<!-- more -->

---

## 实验内容

> ### １.目录
>
> 1. 统计数据描述；  
> 2. 统计数据标准化；
> 3. 统计数据可视化；  
> 4. 数据连接(VLOOKUP)；  
> 5. 数据透视(Pivot)。  
>
---

### 2. 统计数据描述

1>  集中趋势:  

| 特征 | Excel | Python |
| :-------:| :---: | :----: |
| 平均数 | Average函数 | np.mean(array) |
| 中位数 | Median函数 | np.median(array) |
| 众数 | Mode函数 | stats.mode(array) |
2>  离中趋势：  

| 特征 | Excel | Python |
| :-------:| :---: | :----: |
| 极差 | Max-Min | np.ptp(array) |
| 方差和标准差 | Var和Stdev函数 | np.std(array)和 np.var(array)|
| 变异系数 | Stdev/Average |std(array) / mean(array)|
| 四分位差 | Quartile3- Quartile1 | np.percentile(array,75)-np.percentile(array,25)) |

3> 数据分布特征：  

| 特征 | Excel | Python |
| :-------:| :---: | :----: |
| 偏度系数 | Skew函数 | st.skew(array) |
| 峰度系数 | Kurt函数 | st.kurtosis(array) |

### 3. 统计数据标准化

1> 归一化法  

<img src="https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/normalization1.png" width="80%"/>  

2> 线性比例法/相对值法

<img src="https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/normalization2.png" width="85%"/>
3> 

3> 极差法

![Max-Min1](E:/bw_ch/Pictures/Saved Pictures/计量地理学/实验1/Max-Min1.png)

 ![](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/Max-Min2.png)

4> 均值法  

![mean](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/mean.png)

5> 标准差法  

![std1](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/std1.png)

![std2](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/std2.png)

6> 功效系数法

![ec](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/ec.png)
__Python实现：__  
以极差标准化为例：

```Python
import numpy as np


# 数据准备
arr = np.random.randint(0,100,size=100)

# 数据处理
arr_nor = (arr-np.min(arr))/(np.max(arr)-np.min(arr))

# 数据展示
print("原始array：{}\n".format(arr))
print("归一化array：{}\n".format(arr_1))
```

### 4. 统计数据可视化  

1. 描述数据__分布__：直方图、箱线图；
2. 描述数据__构成__：饼状图、雷达图；
3. 描述数据__联系__：散点图、相关系数矩阵；
4. 描述数据__比较__：柱状图/条形图、折线图。

#### Excel操作

![Excel](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/Excel.png)

#### Python操作

以直方图为例：

```Python
import numpy as np
import matplotlib.pyplot as plt

# 数据准备
np.random.seed(19680801)

mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

# 设置直方图参数以及展示
n, bins, patches = plt.hist(x, 50, density=True, facecolor='g', alpha=0.75)

plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')    # 添加文本到（60，0.025）
plt.axis([40, 160, 0, 0.03])
plt.grid(True)   # 配置网格线
plt.show()


# 箱线图：
seaborn.boxplot() 或 plot.box()

# 饼状图：
matplotlib.pyplot.pie()
# 雷达图：
matplotlib.pyplot.polar()

# 散点图：
matplotlib.pyplot.scatter()
# 热力图：
seaborn.heatmap()

# 条形图：
matplotlib.pyplot.bar()   # 水平
matplotlib.pyplot.barh()  # 垂直
# 折线图：
matplotlib.pyplot.plot()
```

### 5.数据连接（vlookup)

#### Excel  操作

直接使用 = VLOOKUP （你想要查找的内容，要查找的位置，包含要返回的值的区域中的列号，返回近似或精确匹配-表示为 1/TRUE 或 0/假）  
__使用方式：__
1、选中要连接单元格；  
2、点击 公式-查找和引用-VLOOKUP 或者 直接 键入=VLOOKUP( ；  
3、点选要查找的值、包含要返回的值的区域中的列号、返回近似或精确匹配-表示为 1/TRUE 或 0/假。

#### Python 操作

这个方法就很多了。  
举几个思路：  

1. 借助字典方法 dict.get()

```Python
a = {'A':1, 'D':4, 'E':5}
b = {'A', 'B', 'C', 'D', 'E'}

c = {k:a.get(k) for k in b}

输出：
{'A': 1, 'E': 5, 'C': None, 'D': 4, 'B': None}
```

2. 使用pandas空值填充dataframe.fillna()  

```Python
import numpy as np
import pandas as pd

# 数据准备
frame1 = pd.DataFrame([[1, 5.1],[2, 5.7],[3, np.nan]],columns=['id','num'])
frame2 = pd.DataFrame([[1, 5.1],[2, 5.7],[4, 9.9],[3,8.8]],columns=['id','num'])

# 操作准备
frame1 = frame1.set_index('id')
frame2 = frame2.set_index('id')

# 数据连接
frame3 = frame1.fillna(frame2)
```

这里需要注意操作准备中设置索引是功能实现的关键，fillna是按索引来填充的。没有的话，对比下面6，7，8行输出：

```Python
In [1]: import pandas as pd
In [2]: import numpy as np

In [3]: frame1 = pd.DataFrame([[1, 5.1],[2, 5.7],[3, np.nan]],columns=['id','num'])
In [4]: frame2 = pd.DataFrame([[1, 5.1],[2, 5.7],[4, 9.9],[3,8.8]],columns=['id','num'])
In [5]: frame3 = frame1.fillna(frame2)

In [6]: frame1
Out[6]:
   id  num
0   1  5.1
1   2  5.7
2   3  NaN
In [7]: frame2
Out[7]:
   id  num
0   1  5.1
1   2  5.7
2   4  9.9
3   3  8.8
In [8]: frame3
Out[8]:
   id  num
0   1  5.1
1   2  5.7
2   3  9.9

In [9]: frame1 = frame1.set_index('id')
In [10]: frame2 = frame2.set_index('id')
In [11]: frame3 = frame1.fillna(frame2)

In [12]: frame3
Out[12]:
id   num
1    5.1
2    5.7
3    8.8
```

### 6.数据透视（pivot)

借助pandas中的pivot_table(dataframe)
