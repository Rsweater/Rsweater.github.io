---
title: Cluster Analysis
tags:
  - K-Mean Clustering
  - Dbsacn Clustering
categories:
  - Machine Learning
abbrlink: 18403
date: 2020-06-05 17:14:52
---

**摘要：** K-Mean Clustering、DBSCAN Clustering、以及Python实现思路（包括空间聚类）。

<!---more--->

## Clustering

**聚类：** 将物理或抽象对象的集合分成由类似的对象组成的多个类的过程被称为聚类。

### Clustering 特点

- 非监督分类
- 聚类：相似的东西分一组
- 难点：如何评估，如何调参

**K-Mean基本概念**

## K-Mean Clustering

### 算法概述

- 要得到簇的个数，指定K值
- 质心：均值，即向量各维度取平均
- 距离的度量：常用欧式距离（先标准化）
- 优化目标：$\min\sum^K_{i=1}{\sum_{x\in{C_i}}{dist(c_i, x)^2}}$ ，其中 $c_i$ 表示质心。

### 工作流程

1. 随机确定k个质心，根据距离进行分类；
2. 对分类结果的所有点进行计算新的质心；
3. 再根据新的质心进行分类，一直到不能再更新为止。

### 优势

- 简单，快速，适合常规数据集

### 劣势

- K值难确定
- 复杂度与样本呈线性关系
- 很难发现任意形状点的簇

### 迭代可视化展示

**网址：**<https://www.naftaliharris.com/blog/visualizing-k-means-clustering/>

## DBSCAN Clustering

### 算法概论

- **英文全称**：Density-Based Spatial Clustering of Applications with Noise

- **核心对象：** 若某个点的密度达到算法设定的阈值，则认为是核心点

    （即 r 邻域内的点的数量不小于 **minPts**）

- **$\epsilon$-邻域的距离阈值**：设定的半径 **r**

- **直接密度可达：**若某点p在**核心点q**的r邻域内，则p-q直接密度可达。

- **密度可达：**若一个点的序列$q_0、q_1、...、q_k$，对任意$q_i-q_{i-1}$是直接密度可达的，则称从$q_0-q_k$密度可达，这实际上是直接密度可达的“传播”。

- **边界点：** 核心点的邻域范围内没有其他点了（除本身的核心点外）

- **噪声点：** 不属于任何一类簇的点。从任何一个核心点出发都是密度不可达的

### 工作流程

```
// 到核心点密度可达的点为一个簇

标记 所有对象为 unvisited；
Do 
随机选择 一个unvisited对象p；
标记 p 为visited；
If p的邻域至少有MinPts个对象
	创建一个新簇C，并把p添加到C；
	令 N为p的邻域中的对象集合
	For N中的每个点
		If p是unvisited；
		标记 p为visited；
		If p的邻域至少有MinPts个对象，把这些对象添加到N；
		如果 p 还不是任何簇的成员，把p添加到C；
	End for；
	输出 C；
Else 标记p为噪声；
Until 没有标记的为unvisited的对象；
```

- 参数选择：
    - 半径 r，可以根据 K距离 来设定：找突变点
    - MinPts：K-距离中k的值，一般取小一些

### 优势

- 不需要指定簇个数
- 可以发现任意形状的簇
- 擅长找到离群点（检测任务）
- 参数少（r、MinPts）

### 劣势

- 高维数据困难（可以降维）
- 参数难选择（参数对结果应该影响大）
- Sklearn中效率很慢（可以采用个别数据消减策略）

### 迭代可视化展示

**网址：**<https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/>

## Python实现

- pandas读数据
- sklearn.cluster进行聚类分析
- pandas添加新的Series类别
- groupby('cluster').mean()等可以看各类的均值等指标
- 可以取出中心点画图

### 空间聚类分析

​	可以借助ArcPy、Fiona、Shapely、**geopandas**等提取中心点之后，转化成了传统的聚类分析。



