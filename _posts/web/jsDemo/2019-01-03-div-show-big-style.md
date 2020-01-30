---
layout: article
title: '[JavaScript 实例] 从元素中心点逐渐放大的效果实现'
date: 2019.01.03 18:38:04
key: jsdemo_20190103_1
category: 
- Web 
- Jsdemo
tags:
- JS 实例
---

### 目标
实现从 DIV 元素中心点逐渐放大的效果

<!-- more -->

 ![div-big.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/jsdemo/div-big.gif)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="content-type" content="text/html, charset=utf-8" />
    <title>以元素中心点逐渐放大效果</title>
    <script src="../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      #box {
        width: 100px;
        height: 100px;
        position: absolute;
        top: 200px;
        left: 200px;
        border-radius: 10px;
        box-shadow: 0 0 20px #999;
        line-height: 100px;
        text-align: center;
        font-size: 18px;
        color: #fff;
        background: #5298c7;
      }
    </style>
  </head>
  <body>
    <div id="box">AAA</div>

    <script>
      $(function() {
        var width;
        var height;
        var top;
        var left;
        $("#box")
          .mouseenter(function() {
            var $this = $(this);
            width = $this.width();
            height = $this.height();
            top = $this.position().top;
            left = $this.position().left;
            var nWidth = 1.2 * width; // 原来基础上放大1.2倍
            var nHeight = 1.2 * height;
            // 计算放大后的 top 属性值，为 box 的高度*0.2再除以2
            var nTop = $this.position().top - (0.2 * $this.height()) / 2; 
            var nLeft = $this.position().left - (0.2 * $this.width()) / 2;
            $this.animate({
              width: nWidth,
              height: nHeight,
              top: nTop,
              left: nLeft,
              "font-size": 27,
              "line-height": nHeight
            });
          })
          .mouseout(function() {
            var $this = $(this);
            $this.animate(
              {
                width: width,
                height: height,
                top: top,
                left: left,
                "font-size": 18,
                "line-height": height
              },
              500
            );
          });
      });
    </script>
  </body>
</html>
```