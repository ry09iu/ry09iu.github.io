---
layout: article
title: 'Node 及 npm 命令异常提示 Killed 9'
date: 2020.04.08 23:22:05
key: web_20200408_1
category:
  - Web
  - note
tags:
  - 前端开发
  - NodeJS
---

当 MySQL 数据库表中时间字段类型为 datetime 时，在 Nodejs 后台服务中查询到的数据，时间会比实际相差8小时，并且显示的是 ISO 时间格式，解决方案很简单。

<!-- more -->

在 MySQL 配置文件中加入 timezone: '08:00'

```javascript
mysql: {
  host: 'localhost',
  user: 'root',
  password: '12345',
  database: 'tb_name',
  port: '3306',
  timezone: '08:00'   // 新增该配置
}
```