---
title: Notes_02|Mysql Related Concept
date: 2020-05-14 19:06:00
abbrlink: 9977
tags:
  - Studying
  - Notebook
  - Data analysis
categories:
  - MySQL
---

黑马授课视频笔记
<!-- more -->
### 数据库特点

- 持久化存储
- 读写速度极高
- 保证数据的有效性
- 对程序支持性非常好，容易扩展

## 关系型数据库的核心  

- 数据行（记录）
- 数据列（字段）
- 数据表（数据行的集合）
- 数据库（数据表的集合）

__举例：__ Excel文件(数据库) ---->  Sheet(数据表) ---> 行、列(数据行、列)  

## MySQL、redis、MongoDB常用范围  

- MySQL常用于建设网站
- redis存贮缓存
- MongoDB用于存贮不相关的数据（例如不同网站信息的爬取）  

## SQL语句、MySQL（数据库管理工具+数据库）

### SQL语句主要分为

- __DQL：数据查询语句，用于对数据进行查询，如select__  
- __DML：数据操作语句，对数据进行增加、删除、修改，如insert、update、delete__  
- TPL：事务处理语言，对事物进行处理，包括begin transaction、commit、rollback  
- DCL：数据控制语言，进行授权与权限回收，如grant、revoke  
- DDL：数据定义语句，进行数据库、表的管理等，如create、drop  
- CCL：指针控制语句，通过控制指针完成表的操作，如declare cursor  

对于web程序员来说，重点是数据的crud(增删改查)，必须熟练编辑DQL、DML，能够编写DDL完成数据库、表的操作，其他语言如TPL、DCL、CCL了解即可  

不区分大小写
