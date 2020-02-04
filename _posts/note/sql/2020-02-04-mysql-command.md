---
layout: article
title: 'MySQL 常用简单语句整理'
date: 2020-02-04 22:15
key: sql_20200204_1
category: 
- note 
- sql
tags:
- 学习笔记
- MySQL
---

### 前言

记录下一些简单又常用的 MySQL 语句

<!-- more -->

### 正文

添加字段
`alter table user add dirName varchar(255) default null`

修改字段类型
`alter table stat modify updateTime int(20)`

按条件查询
`select * from stat where status=1`

分页查询
`select * from stat limit (pageIndex-1)*pageSize,pageSize`

按时间段查询
`select * from stat where uploadTime between 1569081600 and 1569254400`

排序
`select * from stat order by uploadTime desc`

多条件查询

`select * from stat where uploadTime between 1569081600 and 1569254400 order by uploadTime desc limit 0,20`

`select * from stat where status=1 and (uploadTime between 1569081600 and 1569254400)`

删除字段
`alter table car_evidence drop column unit_name`

修改字段名称
`alter table env_boqing change ver version varchar(50) default null`