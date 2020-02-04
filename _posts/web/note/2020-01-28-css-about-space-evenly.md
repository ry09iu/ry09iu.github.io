---
layout: article
title: 'CSS 属性值 space-evenly 无效的解决方案'
date: 2020.01.28 10:23:01
key: web_20190128_1
category: 
- Web 
- note
tags:
- 前端开发
- HTML/CSS
---

本文讨论下部分浏览器使用CSS 属性`space-evenly`无效的解决方案

<!-- more -->

我们可以设置`display: flex`属性来实现弹性布局，其中`justify-content`有2个属性值值得我们关注

```css
space-between: 均匀排列每个元素，首个元素放置于起点末尾元素放置于终点
space-evenly: 均匀排列每个元素，每个元素之间的间隔相等
```

但是，在部分浏览器中使用`space-evenly`却没有效果，解决方案是设置元素属性为`space-between`，为该元素添加宽度为0的伪类 (n-1+2)，即等于用了`space-evenly`实现了布局 (n+1)

```css
container{
      display: flex;
      justify-content: space-between;
      /* justify-content: space-evenly; */
}
container::before,
container::after{
    content: '';
    display: block;
}
```