---
abbrlink: 10772
title: QG实验2-经典统计分析1
date: 2020-05-12 18:38:07
tags:
  - Excel
  - Python
  - 地理空间分析
  - 数据分析
categories:
  - 计量地理学
---
用Python实现经典统计分析中的相关分析，本文是ipynb转化生成
<!-- more -->
__内容概述:__

1. 相关分析Spearman、Pearson系数的计算；
2. 一元线性回归；
3. 多元线性回归。

# 1 相关分析——Spearman、Pearson系数


```python
try:
    import pandas as pd
    import numpy as np
except:
    !pip3 install pandas numpy matplotlib
```


```python
Data = {
    '平均气温x': [3.80, 4.00, 5.80, 8.00, 11.30, 14.40, 16.50, 16.20, 13.80, 10.80, 6.70, 4.70],
    '降雨量y': [77.70, 51.20, 60.10, 54.10, 55.40, 56.80, 45.00, 55.30, 67.50, 73.30, 76.60, 79.60]
}
data = pd.DataFrame(Data)
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>平均气温x</th>
      <th>降雨量y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.8</td>
      <td>77.7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.0</td>
      <td>51.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.8</td>
      <td>60.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.0</td>
      <td>54.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11.3</td>
      <td>55.4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14.4</td>
      <td>56.8</td>
    </tr>
    <tr>
      <th>6</th>
      <td>16.5</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>16.2</td>
      <td>55.3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13.8</td>
      <td>67.5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10.8</td>
      <td>73.3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6.7</td>
      <td>76.6</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4.7</td>
      <td>79.6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pearson相关系数
data.corr('pearson')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>平均气温x</th>
      <th>降雨量y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>平均气温x</th>
      <td>1.000000</td>
      <td>-0.489495</td>
    </tr>
    <tr>
      <th>降雨量y</th>
      <td>-0.489495</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



__Spearman相关系数看下面 2 pandas--read_excel最后。__  
前面的部分是Python打开Excel常用的操作

# 2 pandas——read_excel

```Python3
pandas.read_excel(io, sheet_name=0, header=0, names=None, index_col=None, usecols=None, squeeze=False, dtype=None, engine=None, converters=None, true_values=None, false_values=None, skiprows=None, nrows=None, na_values=None, keep_default_na=True, verbose=False, parse_dates=False, date_parser=None, thousands=None, comment=None, skipfooter=0, convert_float=True, mangle_dupe_cols=True, **kwds)
```
用途：Read an Excel file into a pandas DataFrame  
支持格式：xls、xlsx、xlsm、xlsb和odf，可以是来自本地，也可以来自网络UR。
支持读入单个或多个工作表。   

API参考：https://pandas.pydata.org/docs/reference/api/pandas.read_excel.html#pandas.read_excel

## 2.1 数据准备  
    定位到工作表

__内容：__  
1. 路径io：接受任何的字符串路径，不论是本地的file还是其他的ftp、http、s3等等。
2. 工作表sheet_name：接受 str、int、list，or None, defult 0
    1. 字符串对应工作表名称；
    2. 整型对应工作表索引；
    3. 包含字符串或者整型的列表对应多个工作表；
    4. None 表示解析所有工作表；

    

 注：如果使用解析多个工作表，将以字典的形式输出


```python
# sheet_name表明需要解析那张表格，默认为0（第一张）
data1 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=2)
data1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>省市</th>
      <th>GDP(x)(亿元)</th>
      <th>GDP位次R1</th>
      <th>总人口(Y)(万人)</th>
      <th>总人口位次R2</th>
      <th>位次差的平方</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>n</th>
      <th>显著水平α</th>
      <th>Unnamed: 12</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>广东</td>
      <td>13625.866128</td>
      <td>1.0</td>
      <td>7954.22</td>
      <td>4.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>秩相关系数</td>
      <td>0.784677</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.050</td>
      <td>0.010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>江苏</td>
      <td>12460.830000</td>
      <td>2.0</td>
      <td>7405.82</td>
      <td>5.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>山东</td>
      <td>12435.930000</td>
      <td>3.0</td>
      <td>9125.00</td>
      <td>2.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>0.900</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>浙江</td>
      <td>9395.000000</td>
      <td>4.0</td>
      <td>4679.55</td>
      <td>11.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>0.829</td>
      <td>0.943</td>
    </tr>
    <tr>
      <th>4</th>
      <td>河北</td>
      <td>7098.560000</td>
      <td>5.0</td>
      <td>6769.44</td>
      <td>6.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>0.714</td>
      <td>0.893</td>
    </tr>
    <tr>
      <th>5</th>
      <td>河南</td>
      <td>7048.590000</td>
      <td>6.0</td>
      <td>9667.00</td>
      <td>1.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>0.643</td>
      <td>0.833</td>
    </tr>
    <tr>
      <th>6</th>
      <td>上海</td>
      <td>6250.810000</td>
      <td>7.0</td>
      <td>1711.00</td>
      <td>25.0</td>
      <td>324</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>0.600</td>
      <td>0.783</td>
    </tr>
    <tr>
      <th>7</th>
      <td>辽宁</td>
      <td>6002.540000</td>
      <td>8.0</td>
      <td>4210.00</td>
      <td>14.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>0.564</td>
      <td>0.746</td>
    </tr>
    <tr>
      <th>8</th>
      <td>四川</td>
      <td>5456.320000</td>
      <td>9.0</td>
      <td>8700.40</td>
      <td>3.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>0.456</td>
      <td>0.712</td>
    </tr>
    <tr>
      <th>9</th>
      <td>湖北</td>
      <td>5401.710000</td>
      <td>10.0</td>
      <td>6001.70</td>
      <td>9.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.0</td>
      <td>0.456</td>
      <td>0.645</td>
    </tr>
    <tr>
      <th>10</th>
      <td>福建</td>
      <td>5232.170000</td>
      <td>11.0</td>
      <td>3488.00</td>
      <td>18.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>16.0</td>
      <td>0.425</td>
      <td>0.601</td>
    </tr>
    <tr>
      <th>11</th>
      <td>湖南</td>
      <td>4638.730000</td>
      <td>12.0</td>
      <td>6662.80</td>
      <td>7.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.0</td>
      <td>0.399</td>
      <td>0.564</td>
    </tr>
    <tr>
      <th>12</th>
      <td>黑龙江</td>
      <td>4430.000000</td>
      <td>13.0</td>
      <td>3815.00</td>
      <td>16.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>0.377</td>
      <td>0.534</td>
    </tr>
    <tr>
      <th>13</th>
      <td>安徽</td>
      <td>3972.380000</td>
      <td>14.0</td>
      <td>6410.00</td>
      <td>8.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>0.359</td>
      <td>0.508</td>
    </tr>
    <tr>
      <th>14</th>
      <td>北京</td>
      <td>3663.100000</td>
      <td>15.0</td>
      <td>1456.40</td>
      <td>26.0</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.0</td>
      <td>0.343</td>
      <td>0.485</td>
    </tr>
    <tr>
      <th>15</th>
      <td>江西</td>
      <td>2830.460000</td>
      <td>16.0</td>
      <td>4254.23</td>
      <td>13.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.0</td>
      <td>0.329</td>
      <td>0.465</td>
    </tr>
    <tr>
      <th>16</th>
      <td>广西</td>
      <td>2735.130000</td>
      <td>17.0</td>
      <td>4857.00</td>
      <td>10.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.0</td>
      <td>0.317</td>
      <td>0.448</td>
    </tr>
    <tr>
      <th>17</th>
      <td>吉林</td>
      <td>2522.620000</td>
      <td>18.0</td>
      <td>2703.70</td>
      <td>21.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>0.306</td>
      <td>0.432</td>
    </tr>
    <tr>
      <th>18</th>
      <td>云南</td>
      <td>2465.290000</td>
      <td>19.0</td>
      <td>4375.60</td>
      <td>12.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>山西</td>
      <td>2456.590000</td>
      <td>20.0</td>
      <td>3314.29</td>
      <td>19.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>天津</td>
      <td>2447.660000</td>
      <td>21.0</td>
      <td>1011.30</td>
      <td>27.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>陕西</td>
      <td>2398.580000</td>
      <td>22.0</td>
      <td>3689.50</td>
      <td>17.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>重庆</td>
      <td>2250.560000</td>
      <td>23.0</td>
      <td>3130.00</td>
      <td>20.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>内蒙古</td>
      <td>2150.414897</td>
      <td>24.0</td>
      <td>2379.61</td>
      <td>23.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>新疆</td>
      <td>1877.610000</td>
      <td>25.0</td>
      <td>1933.95</td>
      <td>24.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>贵州</td>
      <td>1356.110000</td>
      <td>26.0</td>
      <td>3869.66</td>
      <td>15.0</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>甘肃</td>
      <td>1304.600000</td>
      <td>27.0</td>
      <td>2603.34</td>
      <td>22.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>海南</td>
      <td>670.930000</td>
      <td>28.0</td>
      <td>810.52</td>
      <td>28.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>青海</td>
      <td>390.210000</td>
      <td>29.0</td>
      <td>533.80</td>
      <td>30.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>宁夏</td>
      <td>385.340000</td>
      <td>30.0</td>
      <td>580.30</td>
      <td>29.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>西藏</td>
      <td>184.500000</td>
      <td>31.0</td>
      <td>270.17</td>
      <td>31.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1068</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



__内容：__ 
   3. 列标签header: 默认defult 0，可以接受一个整数或者一个整数列表，整数所在的行作为列标签，整数列表则是表示多重标签。如果不需要列名，使用None。
   4. 自定义列名names: 
      1. 基于header的基础上，接收列表，定义列名；
      2. 不能与header=None同时使用；
      3. names的长度必须和Excel列长度一致。
   5. 行标签index_col: 与header类似。
   6. 强制规定列数据类型converters，传入字典{列：类型}，dtype类似。


```python
# 列标签header
data2 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', 2, header=None)
data2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>省市</td>
      <td>GDP(x)(亿元)</td>
      <td>GDP位次R1</td>
      <td>总人口(Y)(万人)</td>
      <td>总人口位次R2</td>
      <td>位次差的平方</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>n</td>
      <td>显著水平α</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>广东</td>
      <td>13625.9</td>
      <td>1</td>
      <td>7954.22</td>
      <td>4</td>
      <td>9</td>
      <td>NaN</td>
      <td>秩相关系数</td>
      <td>0.784677</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.05</td>
      <td>0.010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>江苏</td>
      <td>12460.8</td>
      <td>2</td>
      <td>7405.82</td>
      <td>5</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>山东</td>
      <td>12435.9</td>
      <td>3</td>
      <td>9125</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>0.9</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>浙江</td>
      <td>9395</td>
      <td>4</td>
      <td>4679.55</td>
      <td>11</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>0.829</td>
      <td>0.943</td>
    </tr>
    <tr>
      <th>5</th>
      <td>河北</td>
      <td>7098.56</td>
      <td>5</td>
      <td>6769.44</td>
      <td>6</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>0.714</td>
      <td>0.893</td>
    </tr>
    <tr>
      <th>6</th>
      <td>河南</td>
      <td>7048.59</td>
      <td>6</td>
      <td>9667</td>
      <td>1</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>0.643</td>
      <td>0.833</td>
    </tr>
    <tr>
      <th>7</th>
      <td>上海</td>
      <td>6250.81</td>
      <td>7</td>
      <td>1711</td>
      <td>25</td>
      <td>324</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9</td>
      <td>0.6</td>
      <td>0.783</td>
    </tr>
    <tr>
      <th>8</th>
      <td>辽宁</td>
      <td>6002.54</td>
      <td>8</td>
      <td>4210</td>
      <td>14</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10</td>
      <td>0.564</td>
      <td>0.746</td>
    </tr>
    <tr>
      <th>9</th>
      <td>四川</td>
      <td>5456.32</td>
      <td>9</td>
      <td>8700.4</td>
      <td>3</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12</td>
      <td>0.456</td>
      <td>0.712</td>
    </tr>
    <tr>
      <th>10</th>
      <td>湖北</td>
      <td>5401.71</td>
      <td>10</td>
      <td>6001.7</td>
      <td>9</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>0.456</td>
      <td>0.645</td>
    </tr>
    <tr>
      <th>11</th>
      <td>福建</td>
      <td>5232.17</td>
      <td>11</td>
      <td>3488</td>
      <td>18</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>16</td>
      <td>0.425</td>
      <td>0.601</td>
    </tr>
    <tr>
      <th>12</th>
      <td>湖南</td>
      <td>4638.73</td>
      <td>12</td>
      <td>6662.8</td>
      <td>7</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18</td>
      <td>0.399</td>
      <td>0.564</td>
    </tr>
    <tr>
      <th>13</th>
      <td>黑龙江</td>
      <td>4430</td>
      <td>13</td>
      <td>3815</td>
      <td>16</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20</td>
      <td>0.377</td>
      <td>0.534</td>
    </tr>
    <tr>
      <th>14</th>
      <td>安徽</td>
      <td>3972.38</td>
      <td>14</td>
      <td>6410</td>
      <td>8</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22</td>
      <td>0.359</td>
      <td>0.508</td>
    </tr>
    <tr>
      <th>15</th>
      <td>北京</td>
      <td>3663.1</td>
      <td>15</td>
      <td>1456.4</td>
      <td>26</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24</td>
      <td>0.343</td>
      <td>0.485</td>
    </tr>
    <tr>
      <th>16</th>
      <td>江西</td>
      <td>2830.46</td>
      <td>16</td>
      <td>4254.23</td>
      <td>13</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26</td>
      <td>0.329</td>
      <td>0.465</td>
    </tr>
    <tr>
      <th>17</th>
      <td>广西</td>
      <td>2735.13</td>
      <td>17</td>
      <td>4857</td>
      <td>10</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28</td>
      <td>0.317</td>
      <td>0.448</td>
    </tr>
    <tr>
      <th>18</th>
      <td>吉林</td>
      <td>2522.62</td>
      <td>18</td>
      <td>2703.7</td>
      <td>21</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30</td>
      <td>0.306</td>
      <td>0.432</td>
    </tr>
    <tr>
      <th>19</th>
      <td>云南</td>
      <td>2465.29</td>
      <td>19</td>
      <td>4375.6</td>
      <td>12</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>山西</td>
      <td>2456.59</td>
      <td>20</td>
      <td>3314.29</td>
      <td>19</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>天津</td>
      <td>2447.66</td>
      <td>21</td>
      <td>1011.3</td>
      <td>27</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>陕西</td>
      <td>2398.58</td>
      <td>22</td>
      <td>3689.5</td>
      <td>17</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>重庆</td>
      <td>2250.56</td>
      <td>23</td>
      <td>3130</td>
      <td>20</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>内蒙古</td>
      <td>2150.41</td>
      <td>24</td>
      <td>2379.61</td>
      <td>23</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>新疆</td>
      <td>1877.61</td>
      <td>25</td>
      <td>1933.95</td>
      <td>24</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>贵州</td>
      <td>1356.11</td>
      <td>26</td>
      <td>3869.66</td>
      <td>15</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>甘肃</td>
      <td>1304.6</td>
      <td>27</td>
      <td>2603.34</td>
      <td>22</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>海南</td>
      <td>670.93</td>
      <td>28</td>
      <td>810.52</td>
      <td>28</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>青海</td>
      <td>390.21</td>
      <td>29</td>
      <td>533.8</td>
      <td>30</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>宁夏</td>
      <td>385.34</td>
      <td>30</td>
      <td>580.3</td>
      <td>29</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>西藏</td>
      <td>184.5</td>
      <td>31</td>
      <td>270.17</td>
      <td>31</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1068</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 自定义列名names
data3 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', 2, names=[1,2,3,4,5,6,7,8,9,10,11,12,13])
data3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>广东</td>
      <td>13625.866128</td>
      <td>1.0</td>
      <td>7954.22</td>
      <td>4.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>秩相关系数</td>
      <td>0.784677</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.050</td>
      <td>0.010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>江苏</td>
      <td>12460.830000</td>
      <td>2.0</td>
      <td>7405.82</td>
      <td>5.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>山东</td>
      <td>12435.930000</td>
      <td>3.0</td>
      <td>9125.00</td>
      <td>2.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>0.900</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>浙江</td>
      <td>9395.000000</td>
      <td>4.0</td>
      <td>4679.55</td>
      <td>11.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>0.829</td>
      <td>0.943</td>
    </tr>
    <tr>
      <th>4</th>
      <td>河北</td>
      <td>7098.560000</td>
      <td>5.0</td>
      <td>6769.44</td>
      <td>6.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>0.714</td>
      <td>0.893</td>
    </tr>
    <tr>
      <th>5</th>
      <td>河南</td>
      <td>7048.590000</td>
      <td>6.0</td>
      <td>9667.00</td>
      <td>1.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>0.643</td>
      <td>0.833</td>
    </tr>
    <tr>
      <th>6</th>
      <td>上海</td>
      <td>6250.810000</td>
      <td>7.0</td>
      <td>1711.00</td>
      <td>25.0</td>
      <td>324</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>0.600</td>
      <td>0.783</td>
    </tr>
    <tr>
      <th>7</th>
      <td>辽宁</td>
      <td>6002.540000</td>
      <td>8.0</td>
      <td>4210.00</td>
      <td>14.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>0.564</td>
      <td>0.746</td>
    </tr>
    <tr>
      <th>8</th>
      <td>四川</td>
      <td>5456.320000</td>
      <td>9.0</td>
      <td>8700.40</td>
      <td>3.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>0.456</td>
      <td>0.712</td>
    </tr>
    <tr>
      <th>9</th>
      <td>湖北</td>
      <td>5401.710000</td>
      <td>10.0</td>
      <td>6001.70</td>
      <td>9.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.0</td>
      <td>0.456</td>
      <td>0.645</td>
    </tr>
    <tr>
      <th>10</th>
      <td>福建</td>
      <td>5232.170000</td>
      <td>11.0</td>
      <td>3488.00</td>
      <td>18.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>16.0</td>
      <td>0.425</td>
      <td>0.601</td>
    </tr>
    <tr>
      <th>11</th>
      <td>湖南</td>
      <td>4638.730000</td>
      <td>12.0</td>
      <td>6662.80</td>
      <td>7.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.0</td>
      <td>0.399</td>
      <td>0.564</td>
    </tr>
    <tr>
      <th>12</th>
      <td>黑龙江</td>
      <td>4430.000000</td>
      <td>13.0</td>
      <td>3815.00</td>
      <td>16.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>0.377</td>
      <td>0.534</td>
    </tr>
    <tr>
      <th>13</th>
      <td>安徽</td>
      <td>3972.380000</td>
      <td>14.0</td>
      <td>6410.00</td>
      <td>8.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>0.359</td>
      <td>0.508</td>
    </tr>
    <tr>
      <th>14</th>
      <td>北京</td>
      <td>3663.100000</td>
      <td>15.0</td>
      <td>1456.40</td>
      <td>26.0</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.0</td>
      <td>0.343</td>
      <td>0.485</td>
    </tr>
    <tr>
      <th>15</th>
      <td>江西</td>
      <td>2830.460000</td>
      <td>16.0</td>
      <td>4254.23</td>
      <td>13.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.0</td>
      <td>0.329</td>
      <td>0.465</td>
    </tr>
    <tr>
      <th>16</th>
      <td>广西</td>
      <td>2735.130000</td>
      <td>17.0</td>
      <td>4857.00</td>
      <td>10.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.0</td>
      <td>0.317</td>
      <td>0.448</td>
    </tr>
    <tr>
      <th>17</th>
      <td>吉林</td>
      <td>2522.620000</td>
      <td>18.0</td>
      <td>2703.70</td>
      <td>21.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>0.306</td>
      <td>0.432</td>
    </tr>
    <tr>
      <th>18</th>
      <td>云南</td>
      <td>2465.290000</td>
      <td>19.0</td>
      <td>4375.60</td>
      <td>12.0</td>
      <td>49</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>山西</td>
      <td>2456.590000</td>
      <td>20.0</td>
      <td>3314.29</td>
      <td>19.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>天津</td>
      <td>2447.660000</td>
      <td>21.0</td>
      <td>1011.30</td>
      <td>27.0</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>陕西</td>
      <td>2398.580000</td>
      <td>22.0</td>
      <td>3689.50</td>
      <td>17.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>重庆</td>
      <td>2250.560000</td>
      <td>23.0</td>
      <td>3130.00</td>
      <td>20.0</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>内蒙古</td>
      <td>2150.414897</td>
      <td>24.0</td>
      <td>2379.61</td>
      <td>23.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>新疆</td>
      <td>1877.610000</td>
      <td>25.0</td>
      <td>1933.95</td>
      <td>24.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>贵州</td>
      <td>1356.110000</td>
      <td>26.0</td>
      <td>3869.66</td>
      <td>15.0</td>
      <td>121</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>甘肃</td>
      <td>1304.600000</td>
      <td>27.0</td>
      <td>2603.34</td>
      <td>22.0</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>海南</td>
      <td>670.930000</td>
      <td>28.0</td>
      <td>810.52</td>
      <td>28.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>青海</td>
      <td>390.210000</td>
      <td>29.0</td>
      <td>533.80</td>
      <td>30.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>宁夏</td>
      <td>385.340000</td>
      <td>30.0</td>
      <td>580.30</td>
      <td>29.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>西藏</td>
      <td>184.500000</td>
      <td>31.0</td>
      <td>270.17</td>
      <td>31.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1068</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## 2.2 数据筛选  
    定位到某一区域
### 2.2.1 解析特定列usecols

可传入 str、list

其中，
1. 如果是str，表示Excel列字母和列范围的列表（如："A:E" 或 "A,C,E:F")；
2. 列表可以是字符串或整型，字符串表示列名称，整型表示列索引。


注：解析特定行用nrows参数。


```python
data4 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=2, usecols=['GDP(x)(亿元)', '总人口(Y)(万人)'])
data4 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>GDP(x)(亿元)</th>
      <th>总人口(Y)(万人)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>13625.866128</td>
      <td>7954.22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12460.830000</td>
      <td>7405.82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12435.930000</td>
      <td>9125.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9395.000000</td>
      <td>4679.55</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7098.560000</td>
      <td>6769.44</td>
    </tr>
    <tr>
      <th>5</th>
      <td>7048.590000</td>
      <td>9667.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6250.810000</td>
      <td>1711.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>6002.540000</td>
      <td>4210.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5456.320000</td>
      <td>8700.40</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5401.710000</td>
      <td>6001.70</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5232.170000</td>
      <td>3488.00</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4638.730000</td>
      <td>6662.80</td>
    </tr>
    <tr>
      <th>12</th>
      <td>4430.000000</td>
      <td>3815.00</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3972.380000</td>
      <td>6410.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3663.100000</td>
      <td>1456.40</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2830.460000</td>
      <td>4254.23</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2735.130000</td>
      <td>4857.00</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2522.620000</td>
      <td>2703.70</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2465.290000</td>
      <td>4375.60</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2456.590000</td>
      <td>3314.29</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2447.660000</td>
      <td>1011.30</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2398.580000</td>
      <td>3689.50</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2250.560000</td>
      <td>3130.00</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2150.414897</td>
      <td>2379.61</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1877.610000</td>
      <td>1933.95</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1356.110000</td>
      <td>3869.66</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1304.600000</td>
      <td>2603.34</td>
    </tr>
    <tr>
      <th>27</th>
      <td>670.930000</td>
      <td>810.52</td>
    </tr>
    <tr>
      <th>28</th>
      <td>390.210000</td>
      <td>533.80</td>
    </tr>
    <tr>
      <th>29</th>
      <td>385.340000</td>
      <td>580.30</td>
    </tr>
    <tr>
      <th>30</th>
      <td>184.500000</td>
      <td>270.17</td>
    </tr>
    <tr>
      <th>31</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 2.2.2 跳过开头结尾的的行
  以哪行开始、以哪行结束  

skiprows：list-like  
  + Rows to skip at the beginning (0-indexed).  
    

skipfooterint, default 0  
  + Rows at the end to skip (0-indexed).


```python
data5 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=2, usecols=['GDP(x)(亿元)', '总人口(Y)(万人)'], skipfooter=1)
data5 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>GDP(x)(亿元)</th>
      <th>总人口(Y)(万人)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>13625.866128</td>
      <td>7954.22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12460.830000</td>
      <td>7405.82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12435.930000</td>
      <td>9125.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9395.000000</td>
      <td>4679.55</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7098.560000</td>
      <td>6769.44</td>
    </tr>
    <tr>
      <th>5</th>
      <td>7048.590000</td>
      <td>9667.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6250.810000</td>
      <td>1711.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>6002.540000</td>
      <td>4210.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5456.320000</td>
      <td>8700.40</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5401.710000</td>
      <td>6001.70</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5232.170000</td>
      <td>3488.00</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4638.730000</td>
      <td>6662.80</td>
    </tr>
    <tr>
      <th>12</th>
      <td>4430.000000</td>
      <td>3815.00</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3972.380000</td>
      <td>6410.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3663.100000</td>
      <td>1456.40</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2830.460000</td>
      <td>4254.23</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2735.130000</td>
      <td>4857.00</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2522.620000</td>
      <td>2703.70</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2465.290000</td>
      <td>4375.60</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2456.590000</td>
      <td>3314.29</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2447.660000</td>
      <td>1011.30</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2398.580000</td>
      <td>3689.50</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2250.560000</td>
      <td>3130.00</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2150.414897</td>
      <td>2379.61</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1877.610000</td>
      <td>1933.95</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1356.110000</td>
      <td>3869.66</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1304.600000</td>
      <td>2603.34</td>
    </tr>
    <tr>
      <th>27</th>
      <td>670.930000</td>
      <td>810.52</td>
    </tr>
    <tr>
      <th>28</th>
      <td>390.210000</td>
      <td>533.80</td>
    </tr>
    <tr>
      <th>29</th>
      <td>385.340000</td>
      <td>580.30</td>
    </tr>
    <tr>
      <th>30</th>
      <td>184.500000</td>
      <td>270.17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 计算 spearman 相关系数
data5.corr('spearman') 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>GDP(x)(亿元)</th>
      <th>总人口(Y)(万人)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>GDP(x)(亿元)</th>
      <td>1.000000</td>
      <td>0.784677</td>
    </tr>
    <tr>
      <th>总人口(Y)(万人)</th>
      <td>0.784677</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



# 3 回归分析   
## 3.1 一元线性回归  
__建立模型：__  
   1. __选取__ 一元线性回归模型的 __变量__ ；
   2. 绘制计算表和拟合散点图;
   3. 计算变量间的回归系数及其相关的显著性；
   4. 回归分析结果的应用。

__模型的检验__:  
   1. 经济意义检验：就是根据模型中各个参数的经济含义，分析各参数的值是否与分析对象的经济含义相符；
   2. 回归标准差检验；
   3. 拟合优度检验；
   4. 回归系数的显著性检验。


```python
from scipy import stats
import matplotlib.pyplot as plt
%matplotlib inline
%config InlineBackend.figure_format = 'svg'  # 转化成矢量图，提高清晰度
```


```python
# 完整sheet在pandas中查看
data6 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=1)
data6
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>台站</th>
      <th>经度x/度</th>
      <th>纬度y/度</th>
      <th>海拔a/m</th>
      <th>年降水量p/mm</th>
      <th>年蒸发量v/mm</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>相关系数</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
      <th>Unnamed: 11</th>
      <th>Unnamed: 12</th>
      <th>Unnamed: 13</th>
      <th>Unnamed: 14</th>
      <th>Unnamed: 15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>安西</td>
      <td>95.92</td>
      <td>40.50</td>
      <td>1170.8</td>
      <td>48.25</td>
      <td>2835.57</td>
      <td>NaN</td>
      <td>py</td>
      <td>-0.903529</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>白银</td>
      <td>104.53</td>
      <td>36.60</td>
      <td>1707.2</td>
      <td>193.72</td>
      <td>1947.97</td>
      <td>NaN</td>
      <td>vy</td>
      <td>0.880732</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>定西</td>
      <td>104.63</td>
      <td>35.53</td>
      <td>1908.8</td>
      <td>413.94</td>
      <td>1538.10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>古浪</td>
      <td>102.90</td>
      <td>37.48</td>
      <td>2072.4</td>
      <td>358.60</td>
      <td>1756.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>和政</td>
      <td>103.35</td>
      <td>35.43</td>
      <td>2136.4</td>
      <td>615.04</td>
      <td>1317.64</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>57</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>58</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Coefficients</td>
      <td>标准误差</td>
      <td>t Stat</td>
      <td>P-value</td>
      <td>Lower 95%</td>
      <td>Upper 95%</td>
      <td>下限 95.0%</td>
      <td>上限 95.0%</td>
    </tr>
    <tr>
      <th>59</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Intercept</td>
      <td>3295.13</td>
      <td>205.455</td>
      <td>16.0382</td>
      <td>2.37198e-21</td>
      <td>2882.46</td>
      <td>3707.8</td>
      <td>2882.46</td>
      <td>3707.8</td>
    </tr>
    <tr>
      <th>60</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>X Variable 1</td>
      <td>-81.1737</td>
      <td>5.38605</td>
      <td>-15.0711</td>
      <td>3.15159e-20</td>
      <td>-91.9919</td>
      <td>-70.3555</td>
      <td>-91.9919</td>
      <td>-70.3555</td>
    </tr>
    <tr>
      <th>61</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>X Variable 2</td>
      <td>0.036214</td>
      <td>0.0208951</td>
      <td>1.73313</td>
      <td>0.0892358</td>
      <td>-0.00575503</td>
      <td>0.078183</td>
      <td>-0.00575503</td>
      <td>0.078183</td>
    </tr>
  </tbody>
</table>
<p>62 rows × 16 columns</p>
</div>




```python
# 数据筛选
data7 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=1, usecols=['纬度y/度', '年降水量p/mm'], skipfooter=9)
# 观察前后五行
print(data7.head(5))
print(data7.tail(5))
```

       纬度y/度  年降水量p/mm
    0  40.50     48.25
    1  36.60    193.72
    2  35.53    413.94
    3  37.48    358.60
    4  35.43    615.04
        纬度y/度  年降水量p/mm
    48  34.70    515.02
    49  35.00    545.72
    50  34.21    786.75
    51  35.43    584.89
    52  36.14    574.00



```python
# 散点图观察趋势
plt.scatter(
    data7['纬度y/度'],
    data7['年降水量p/mm'],
)
plt.xlabel('latitud')
plt.ylabel('amount of precipitation')
```




    Text(0, 0.5, 'amount of precipitation')




![svg](QG%E5%AE%9E%E9%AA%8C2-%E7%BB%8F%E5%85%B8%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%901%EF%BC%88Excel%E3%80%81Python%29_files/QG%E5%AE%9E%E9%AA%8C2-%E7%BB%8F%E5%85%B8%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%901%EF%BC%88Excel%E3%80%81Python%29_19_1.svg)



```python
# 计算参数
x = data7['纬度y/度'].values
y = data7['年降水量p/mm'].values

#############参数说明#############
# slope：斜率                    #
# intercept：截距                #
# r_value：相关系数              #
# p_value：假设检验P值           #
# sts_err：标准误差              #
##################################

slope, intercept, r_value, p_value, std_err = stats.linregress(x,y)
```


```python
# 曲线拟合
plt.scatter(
    data7['纬度y/度'],
    data7['年降水量p/mm'],
)

predictions = slope*data7['纬度y/度'] + intercept
plt.plot(
    data7['纬度y/度'],
    predictions,
    c='black',
    linewidth=2
)
plt.xlabel('纬度y/度')
plt.ylabel('年降水量p/mm')
```




    Text(0, 0.5, '年降水量p/mm')




![svg](QG%E5%AE%9E%E9%AA%8C2-%E7%BB%8F%E5%85%B8%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%901%EF%BC%88Excel%E3%80%81Python%29_files/QG%E5%AE%9E%E9%AA%8C2-%E7%BB%8F%E5%85%B8%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%901%EF%BC%88Excel%E3%80%81Python%29_21_1.svg)


__显著性检验参数有：__   
  1. 回归系数检验（t-检验）
  2. 拟合优度R<sup>2</sup>
  3. 模型检验(F检验）

在一元线性回归分析中，三者可以转化、检验效果基本一致。


```python
# 显著性检验 R²
print("The linear model is: y = {:.5}x + {:.5}".format(slope, intercept))
print("r-squared:", r_value**2)
```

    The linear model is: y = -82.188x + 3395.6
    r-squared: 0.8163651594029047


__补充：__  
Python实现一元线性回归的8种方法：
  1. Simple matrix inverse;
  2. Stats.linregress;
  3. Numpy.linalg.lstsq;
  4. Moore-Penrose inverse;
  5. sklearn.linear_model;
  6. Polyfit;
  7. Statsmodels.OLS;
  8. Optimize.curve_fit。

排名按速度快慢的顺序，其中Statsmodels.OLS()结果像R或Julia等统计语言一样丰富。所以你也可以搭配使用，你可以用sklearn。linalg_model来进行训练预测，用statsmodel.OLS来进行模型评估的。

参考文章：
https://blog.csdn.net/tMb8Z9Vdm66wH68VX1/article/details/79102425   
原文地址：  
https://medium.freecodecamp.org/data-science-with-python-8-ways-to-do-linear-regression-and-measure-their-speed-b5577d75f8b  

## 3.2 多元线性分析  

多元与一元基本一致，基本过程有选取变量、建模、检验。  

__Tip：__ 进行多元线性回归分析时就不能再用Stats.linregress了，它只能进行一元线性回归分析。进行多元线性回归以及非线性关系的线性化都可以用sklearn.linear_modle，[API参考](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.linear_model)


```python
from sklearn import linear_model


# 数据清洗
data8 = pd.read_excel('/home/Ubuntu/Documents/test.xlsx', sheet_name=1, usecols=['纬度y/度', '海拔a/m', '年降水量p/mm'], skipfooter=9)
# 清洗结果查看
print(data8.head())
print(data8.tail())
```

       纬度y/度   海拔a/m  年降水量p/mm
    0  40.50  1170.8     48.25
    1  36.60  1707.2    193.72
    2  35.53  1908.8    413.94
    3  37.48  2072.4    358.60
    4  35.43  2136.4    615.04
        纬度y/度   海拔a/m  年降水量p/mm
    48  34.70  2810.2    515.02
    49  35.00  2915.7    545.72
    50  34.21  3362.7    786.75
    51  35.43  1221.2    584.89
    52  36.14  1111.7    574.00



```python
# 选取变量
x = data8.drop(['年降水量p/mm'], axis=1)
y = data8.drop(['纬度y/度', '海拔a/m'], axis=1)
```


```python
# 创建线性回归对象
regr = linear_model.LinearRegression()

# 使用数据训练模型
regr.fit(x, y)

# 拟合模型
print("The linear model is: Y = {:.5} + {:.5}*维度 + {:.5}*海拔".format(regr.intercept_[0], regr.coef_[0][0], regr.coef_[0][1]))
```

    The linear model is: Y = 3295.1 + -81.174*维度 + 0.036214*海拔


之后，可以直接调用regr实例的Methods，获取想要的相关数据：
   1. get_params()   获取预测（计算模型用的）参数  
   2. predict()     获取预测值
   3. score()       可决系数R<sup>2<sup>

标准误差可以用sklearn.metrics.mean_squared_error()获取    


__再者:__ 如果需要更多参数可以使用statsmodel库，也一样几行代码完成回归计算。[statsmodel库API](https://www.statsmodels.org/stable/api.html)


```python
import statsmodels.api as sm


X2 = sm.add_constant(x)
regr1 = sm.OLS(y, X2).fit()
# 总结
regr1.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>        <td>年降水量p/mm</td>     <th>  R-squared:         </th> <td>   0.827</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.820</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   119.3</td>
</tr>
<tr>
  <th>Date:</th>             <td>Tue, 12 May 2020</td> <th>  Prob (F-statistic):</th> <td>9.24e-20</td>
</tr>
<tr>
  <th>Time:</th>                 <td>16:49:37</td>     <th>  Log-Likelihood:    </th> <td> -312.86</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>    53</td>      <th>  AIC:               </th> <td>   631.7</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>    50</td>      <th>  BIC:               </th> <td>   637.6</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     2</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
    <td></td>       <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th> <td> 3295.1279</td> <td>  205.455</td> <td>   16.038</td> <td> 0.000</td> <td> 2882.460</td> <td> 3707.796</td>
</tr>
<tr>
  <th>纬度y/度</th> <td>  -81.1737</td> <td>    5.386</td> <td>  -15.071</td> <td> 0.000</td> <td>  -91.992</td> <td>  -70.355</td>
</tr>
<tr>
  <th>海拔a/m</th> <td>    0.0362</td> <td>    0.021</td> <td>    1.733</td> <td> 0.089</td> <td>   -0.006</td> <td>    0.078</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 1.809</td> <th>  Durbin-Watson:     </th> <td>   1.347</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.405</td> <th>  Jarque-Bera (JB):  </th> <td>   1.677</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.330</td> <th>  Prob(JB):          </th> <td>   0.432</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.431</td> <th>  Cond. No.          </th> <td>3.03e+04</td>
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[2] The condition number is large, 3.03e+04. This might indicate that there are<br/>strong multicollinearity or other numerical problems.