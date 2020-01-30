---
layout: article
title: '[JavaScript 实例] 导航栏几种常见样式的实现方法'
date: 2018.12.26 14:29:29
key: jsdemo_20181226_2
category: 
- Web 
- Jsdemo
tags:
- JS 实例
---

这里记录下导航栏几种常见的样式，仅供大家参考。

<!-- more -->

### 导航栏1（原生 JS 代码）

比较常见也比较简单的导航栏 ，用的 JavaScript 原生代码，大致思路是通过改变 li 元素的 className 实现鼠标移动切换 tab 时改变其背景色及文字颜色，同时切换显示 tab 对应内容。

 ![nav-1.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/jsdemo/nav-1.gif)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" charset="text/html;charset=utf-8" />
    <title>导航栏1-JS原生版</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      body {
        font: 12px/2 "Courier New";
      }
      #header {
        width: 550px;
        height: 200px;
        margin: 20px auto;
        border: 1px solid #e1e1e1;
      }
      #nav {
        list-style-type: none;
      }
      #nav li {
        width: 110px;
        height: 35px;
        background: #fff;
        float: left;
        color: #000;
        text-align: center;
        line-height: 35px;
        cursor: pointer;
        border-bottom: 1px solid #e1e1e1;
        font-weight: bold;
      }
      #nav li.active {
        background: #11ac63;
        color: #fff;
      }
      #content {
        padding: 50px 30px;
      }
      #content ul {
        display: none;
      }
      #content .list {
        display: block;
      }
    </style>
    <script>
      window.onload = function() {
        var aLi = document.getElementById("nav").getElementsByTagName("li");
        var oUl = document.getElementById("content").getElementsByTagName("ul");
        var i = 0;
        for (i = 0; i < aLi.length; i++) {
          aLi[i].index = i;
          aLi[i].onmouseover = function() {
            for (var x in aLi) {
              aLi[x].className = "";
              oUl[x].className = "";
            }
            this.className = "active";
            oUl[this.index].className = "list";
          };
          aLi[i].onmouseout = function() {
            this.className = "";
            aLi[this.index].className = "active";
          };
        }
      };
    </script>
  </head>
  <body>
    <div id="header">
      <ul id="nav">
        <li class="active">华为</li>
        <li>苹果</li>
        <li>小米</li>
        <li>三星</li>
        <li>一加</li>
      </ul>
      <div id="content">
        <ul class="list">
          <li>华为 Mate20</li>
          <li>华为 P20</li>
          <li>华为荣耀 10</li>
        </ul>
        <ul>
          <li>iPhone XS</li>
          <li>iPhone XS MAX</li>
          <li>iPhone 8</li>
        </ul>
        <ul>
          <li>小米 8</li>
          <li>小米 8 SE</li>
          <li>小米 Play</li>
        </ul>
        <ul>
          <li>三星 Note 9</li>
          <li>三星 S9 +</li>
          <li>三星 A8s</li>
        </ul>
        <ul>
          <li>一加手机 6</li>
          <li>一加手机 6T</li>
          <li>一加手机 5T</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```
### 导航栏1（jQuery 代码）

还是第一种导航栏样式，通过 jQuery 实现，思路不变。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" charset="text/html;charset=utf-8" />
    <title>导航栏2</title>
    <script src="../../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      body {
        font: 12px/2 "Courier New";
      }
      #header {
        width: 550px;
        height: 200px;
        margin: 20px auto;
        border: 1px solid #e1e1e1;
      }
      #nav {
        list-style-type: none;
      }
      #nav li {
        width: 110px;
        height: 35px;
        background: #fff;
        float: left;
        color: #000;
        text-align: center;
        line-height: 35px;
        cursor: pointer;
        border-bottom: 1px solid #e1e1e1;
        font-weight: bold;
        transition: 0.5s;
      }
      #nav li.active {
        background: #fff;
        color: #11ac63;
      }
      #content {
        padding: 50px 30px;
      }
      #content ul {
        display: none;
      }
      #content .list {
        display: block;
      }
    </style>
    <script>
      $(function() {
        $("#nav li").each(function(index) {
          $(this).mouseover(function() {
            $("#nav .active").removeClass("active");
            $("#content .list").removeClass("list");
            $(this).addClass("active");
            $("#content ul:eq(" + index + ")").addClass("list");
          });
        });
      });
    </script>
  </head>
  <body>
    <div id="header">
      <ul id="nav">
        <li class="active">华为</li>
        <li>苹果</li>
        <li>小米</li>
        <li>三星</li>
        <li>一加</li>
      </ul>
      <div id="content">
        <ul class="list">
          <li>华为 Mate20</li>
          <li>华为 P20</li>
          <li>华为荣耀 10</li>
        </ul>
        <ul>
          <li>iPhone XS</li>
          <li>iPhone XS MAX</li>
          <li>iPhone 8</li>
        </ul>
        <ul>
          <li>小米 8</li>
          <li>小米 8 SE</li>
          <li>小米 Play</li>
        </ul>
        <ul>
          <li>三星 Note 9</li>
          <li>三星 S9 +</li>
          <li>三星 A8s</li>
        </ul>
        <ul>
          <li>一加手机 6</li>
          <li>一加手机 6T</li>
          <li>一加手机 5T</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```
### 导航栏2
使用 jQuery 代码，思路与第一种一样，通过改变 li 元素的 className 实现，只是不改变 tab 的背景色而只改变字体颜色，也是比较常见的导航栏样式之一。为了更好的显示效果，顺便给切换标签时字体颜色的改变加了渐变效果。

![nav-2.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/jsdemo/nav-2.gif)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" charset="text/html;charset=utf-8" />
    <title>导航栏2</title>
    <script src="../../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      body {
        font: 12px/2 "Courier New";
      }
      #header {
        width: 550px;
        height: 200px;
        margin: 20px auto;
        border: 1px solid #e1e1e1;
      }
      #nav {
        list-style-type: none;
      }
      #nav li {
        width: 110px;
        height: 35px;
        background: #fff;
        float: left;
        color: #000;
        text-align: center;
        line-height: 35px;
        cursor: pointer;
        border-bottom: 1px solid #e1e1e1;
        font-weight: bold;
        transition: 0.5s;
      }
      #nav li.active {
        color: #11ac63;
      }
      #content {
        padding: 50px 30px;
      }
      #content ul {
        display: none;
      }
      #content .list {
        display: block;
      }
    </style>
    <script>
      $(function() {
        $("#nav li").each(function(index) {
          $(this).mouseover(function() {
            $("#nav .active").removeClass("active");
            $("#content .list").removeClass("list");
            $(this).addClass("active");
            $("#content ul:eq(" + index + ")").addClass("list");
          });
        });
      });
    </script>
  </head>
  <body>
    <div id="header">
      <ul id="nav">
        <li class="active">华为</li>
        <li>苹果</li>
        <li>小米</li>
        <li>三星</li>
        <li>一加</li>
      </ul>
      <div id="content">
        <ul class="list">
          <li>华为 Mate20</li>
          <li>华为 P20</li>
          <li>华为荣耀 10</li>
        </ul>
        <ul>
          <li>iPhone XS</li>
          <li>iPhone XS MAX</li>
          <li>iPhone 8</li>
        </ul>
        <ul>
          <li>小米 8</li>
          <li>小米 8 SE</li>
          <li>小米 Play</li>
        </ul>
        <ul>
          <li>三星 Note 9</li>
          <li>三星 S9 +</li>
          <li>三星 A8s</li>
        </ul>
        <ul>
          <li>一加手机 6</li>
          <li>一加手机 6T</li>
          <li>一加手机 5T</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```
### 导航栏3
使用 jQuery 代码，鼠标移动切换标签时 tab 背景色会通过滑动的方式切换至目标位置。实现思路是在之前导航栏样式基础上多加了个 li 标签，指定该 li 元素的定位类型为 absolute，同时还需设置其父元素为 relative 定位。然后在 JS 代码中修改上述 li 标签的 left 属性值，并且指定其文本内容（可获取当前 tab 的文本内容）。当然，最重要的是给该 li 标签添加过渡属性，否则 tab 切换就没有滑动效果了。

![nav-3.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/jsdemo/nav-3.gif)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" charset="text/html;charset=utf-8" />
    <title>导航栏3</title>
    <script src="../../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      body {
        font: 12px/2 "Courier New";
      }
      #header {
        width: 550px;
        height: 200px;
        margin: 20px auto;
        border: 1px solid #e1e1e1;
      }
      #nav {
        list-style-type: none;
        position: relative;
      }
      #nav li {
        width: 110px;
        height: 35px;
        background: #fff;
        float: left;
        color: #000;
        text-align: center;
        line-height: 35px;
        cursor: pointer;
        border-bottom: 1px solid #e1e1e1;
        font-weight: bold;
      }
      #nav li.active {
        position: absolute;
        background: #11ac63;
        color: #fff;
        left: 0;
        transition: 0.5s;
      }
      #content {
        padding: 50px 30px;
      }
      #content ul {
        display: none;
      }
      #content .list {
        display: block;
      }
    </style>
    <script>
      $(function() {
        $(".tab").hover(function() {
          var index = $(".tab").index(this);
          $(".active").css("left", 110 * index + "px");
          setTimeout(function() {
            $(".active").html($(".tab:eq(" + index + ")").html());
          }, 250);
          $("#content .list").removeClass("list");
          $("#content ul:eq(" + index + ")").addClass("list");
        });
      });
    </script>
  </head>
  <body>
    <div id="header">
      <ul id="nav">
        <li class="active">华为</li>
        <li class="tab">华为</li>
        <li class="tab">苹果</li>
        <li class="tab">小米</li>
        <li class="tab">三星</li>
        <li class="tab">一加</li>
      </ul>
      <div id="content">
        <ul class="list">
          <li>华为 Mate20</li>
          <li>华为 P20</li>
          <li>华为荣耀 10</li>
        </ul>
        <ul>
          <li>iPhone XS</li>
          <li>iPhone XS MAX</li>
          <li>iPhone 8</li>
        </ul>
        <ul>
          <li>小米 8</li>
          <li>小米 8 SE</li>
          <li>小米 Play</li>
        </ul>
        <ul>
          <li>三星 Note 9</li>
          <li>三星 S9 +</li>
          <li>三星 A8s</li>
        </ul>
        <ul>
          <li>一加手机 6</li>
          <li>一加手机 6T</li>
          <li>一加手机 5T</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```
### 导航栏4
使用 jQuery 代码，与上一种导航栏样式实现思路大致相同，只是把新添加的 li 更换为 div，同样设置好该 div 及其父元素的定位类型，并根据 tab 的高度自行调整 div 的高度及 top 属性值。除了该 div 无需设置文本内容外，还加入了 tab 切换时改变字体颜色，其他部分则上一种样式基本一致。

![nav-4.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/jsdemo/nav-4.gif)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" charset="text/html;charset=utf-8" />
    <title>导航栏4</title>
    <script src="../../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      body {
        font: 12px/2 "Courier New";
      }
      #header {
        width: 550px;
        height: 200px;
        margin: 20px auto;
        border: 1px solid #e1e1e1;
      }
      #nav {
        list-style-type: none;
        position: relative;
      }
      #nav li {
        width: 110px;
        height: 35px;
        background: #fff;
        float: left;
        color: #000;
        text-align: center;
        line-height: 35px;
        cursor: pointer;
        border-bottom: 1px solid #e1e1e1;
        font-weight: bold;
      }
      #nav .active {
        position: absolute;
        background: #11ac63;
        color: #fff;
        top: 32px;
        left: 0;
        transition: 0.5s;
        width: 110px;
        height: 3px;
        text-align: center;
      }
      #content {
        padding: 50px 30px;
      }
      #content ul {
        display: none;
      }
      #content .list {
        display: block;
      }
      #nav .tab {
        color: #11ac63;
        transition: 0.5s;
      }
    </style>
    <script>
      $(function() {
        $("#nav li").hover(function() {
          var index = $("#nav li").index(this);
          $(".active").css("left", 110 * index + "px");
          $("#nav .tab").removeClass("tab");
          $(this).addClass("tab");
          $("#content .list").removeClass("list");
          $("#content ul:eq(" + index + ")").addClass("list");
        });
      });
    </script>
  </head>
  <body>
    <div id="header">
      <ul id="nav">
        <div class="active"></div>
        <li class="tab">华为</li>
        <li>苹果</li>
        <li>小米</li>
        <li>三星</li>
        <li>一加</li>
      </ul>
      <div id="content">
        <ul class="list">
          <li>华为 Mate20</li>
          <li>华为 P20</li>
          <li>华为荣耀 10</li>
        </ul>
        <ul>
          <li>iPhone XS</li>
          <li>iPhone XS MAX</li>
          <li>iPhone 8</li>
        </ul>
        <ul>
          <li>小米 8</li>
          <li>小米 8 SE</li>
          <li>小米 Play</li>
        </ul>
        <ul>
          <li>三星 Note 9</li>
          <li>三星 S9 +</li>
          <li>三星 A8s</li>
        </ul>
        <ul>
          <li>一加手机 6</li>
          <li>一加手机 6T</li>
          <li>一加手机 5T</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```