---
layout: article
title: '[JavaScript 实例] 隔行变色、复选框操作及节点删除等练习'
key: web_20181226_1
category: Web
tags:
- 前端开发
- JS 实例
---

## 简介

本次练习主要涉及如下几个主要知识点的应用：

1. 复选框的勾选与取消
2. 鼠标移入移出操作
3. className 的设置
4. 父节点的获取及子节点的删除

<!--more-->

![练习1.gif](https://raw.githubusercontent.com/ry09iu/ry09iu.github.io/master/assets/images/web/js3_1.gif)

## 完整代码

JavaScript 原生代码

```
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
    <title>JavaScript实例练习-JS 原生代码</title>
    <style>
      table {
        height: 50px;
        margin: 0 auto;
      }
      th {
        font-size: 14px;
        font-weight: 700;
        width: 150px;
        height: 40px;
      }
      td {
        width: 150px;
        text-align: center;
      }
      .even {
        background: #b4eeb4;
      }
      .odd {
        background: #cae1ff;
      }
      .active {
        background: #ff6347;
      }
      .lastline {
        background: #fff;
        height: 40px;
      }
      #cbk {
        font-size: 12px;
      }
    </style>
    <script>
      window.onload = function() {
        setRowsBgColor();
        var oInput = document.getElementsByTagName("input");
        var btnDel = document.getElementById("del");
        var i = 0;
        // 全选按钮的点击操作
        oInput[0].onclick = function() {
          // 循环 input 对象，排除前3个及最后1个
          for (i = 3; i < oInput.length - 1; i++) {
            oInput[i].checked = true; // 全部勾选
          }
        };
        // 全不选按钮的点击操作
        oInput[1].onclick = function() {
          for (i = 3; i < oInput.length - 1; i++) {
            oInput[i].checked = false; // 全部不勾选
          }
        };
        // 反选按钮的点击操作
        oInput[2].onclick = function() {
          for (i = 3; i < oInput.length - 1; i++) {
            oInput[i].checked = !oInput[i].checked; // 反选
          }
        };
        // 删除按钮
        btnDel.onclick = function() {
          if (!confirm("您确定删除所选用户信息吗？")) return; // 提示用户是否删除，取消则返回不操作
          var userNodes = document.getElementsByName("user"); // 获取 tbody 下所有 input 对象
          for (var i = 0; i < userNodes.length; i++) {
            // 当复选框被勾选时
            if (userNodes[i].checked) {
              var trNodes = userNodes[i].parentNode.parentNode; // 获取复选框元素父节点的父节点
              trNodes.parentNode.removeChild(trNodes); // 移除上述节点即 tr
              i--;
            }
          }
          setRowsBgColor();
        };
      };
      // 设置奇数行、偶数行及最后一行的背景色，并实现鼠标移入后行背景色
      function setRowsBgColor() {
        var tbNode = document.getElementById("tabInfo"); // 获取 tbody 对象
        var rowsNode = tbNode.rows; // 获取 tbody 下所有行对象
        var name;
        // 循环 tbody 下所有航元素
        for (var i = 0; i < rowsNode.length; i++) {
          if (i % 2 == 1) {
            rowsNode[i].className = "odd"; // 奇数行
          } else {
            rowsNode[i].className = "even"; // 偶数行
          }
          rowsNode[i].onmouseover = function() {
            name = this.className; // 保存鼠标移入之前的 className
            this.className = "active";
          };
          rowsNode[i].onmouseout = function() {
            this.className = name; // 设置之前保存的 className
          };
        }
      }
    </script>
  </head>
  <body>
    <table border="1" id="tbl">
      <thead>
        <tr>
          <th id="cbk">
            <input id="checkAll" type="button" name="" value="全选" />
            <input id="uncheckAll" type="button" name="" value="全不选" />
            <input id="inverse" type="button" name="" value="反选" />
          </th>
          <th>编号</th>
          <th>姓名</th>
          <th>年龄</th>
        </tr>
      </thead>
      <tbody id="tabInfo">
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>1</td>
          <td>张三</td>
          <td>22</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>2</td>
          <td>李四</td>
          <td>25</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>3</td>
          <td>王五</td>
          <td>27</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>4</td>
          <td>赵六</td>
          <td>29</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>5</td>
          <td>田七</td>
          <td>30</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>6</td>
          <td>汾九</td>
          <td>20</td>
        </tr>
      </tbody>
      <tfoot>
        <tr class="lastline">
          <td colspan="4"><input id="del" type="button" value="删除" /></td>
        </tr>
      </tfoot>
    </table>
  </body>
</html>
```

## jQuery 代码

```
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
    <title>JavaScript实例练习-JQ 代码</title>
    <script src="../../scripts/lib/jquery-3.3.1.js"></script>
    <style>
      table {
        height: 50px;
        margin: 0 auto;
      }
      th {
        font-size: 14px;
        font-weight: 700;
        width: 150px;
        height: 40px;
      }
      td {
        width: 150px;
        text-align: center;
      }
      .even {
        background: #b4eeb4;
      }
      .odd {
        background: #cae1ff;
      }
      .active {
        background: #ff6347;
      }
      .lastline {
        background: #fff;
        height: 40px;
      }
      #cbk {
        font-size: 12px;
      }
    </style>
    <script>
      $(function() {
        setRowColor();
        // 设置奇数行、偶数行及最后一行的背景色，并实现鼠标移入后行背景色
        function setRowColor() {
          $("tbody tr").removeClass("even odd"); // 移除 tbody tr 之前的 className
          $("tbody tr:even").addClass("even"); // 偶数行
          $("tbody tr:odd").addClass("odd"); // 奇数行
        }
        var name;
        // 实现移入移出 tr 的 className
        $("tbody tr").hover(
          function() {
            name = this.className; // 保存移入前的 className
            $(this).addClass("active");
          },
          function() {
            $(this).removeClass("active");
            $(this).addClass(name); // 设置为之前保存的 className
          }
        );
        // 全选按钮的点击操作
        $("#checkAll").click(function() {
          $("tbody input").prop("checked", true); // 全部勾选
        });
        // 全不选按钮的点击操作
        $("#uncheckAll").click(function() {
          $("tbody input").prop("checked", false); // 全部不勾选
        });
        // 反选按钮的点击操作
        $("#inverse").click(function() {
          // 循环 tbody 下所有 input 元素
          $("tbody input").each(function() {
            $(this).prop("checked", !$(this).prop("checked")); // 反选
          });
        });
        // 删除按钮的点击操作
        $("#del").click(function() {
          if (!confirm("确定删除选中用户信息吗？")) return; // 提示用户是否删除，取消则返回不操作
          $("tbody input").each(function() {
            if ($(this).prop("checked")) {
              $(this)
                .parent()
                .parent()
                .remove(); // 获取父节点的父节点即 tr 节点
            }
            setRowColor(); // 重新设置 tbody 下 tr 的 className
          });
        });
      });
    </script>
  </head>
  <body>
    <table border="1" id="tbl">
      <thead>
        <tr>
          <th id="cbk">
            <input id="checkAll" type="button" name="" value="全选" />
            <input id="uncheckAll" type="button" name="" value="取消" />
            <input id="inverse" type="button" name="" value="反选" />
          </th>
          <th>编号</th>
          <th>姓名</th>
          <th>年龄</th>
        </tr>
      </thead>
      <tbody id="info">
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>1</td>
          <td>张三</td>
          <td>22</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>2</td>
          <td>李四</td>
          <td>25</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>3</td>
          <td>王五</td>
          <td>27</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>4</td>
          <td>赵六</td>
          <td>29</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>5</td>
          <td>田七</td>
          <td>30</td>
        </tr>
        <tr>
          <td><input type="checkbox" name="user" /></td>
          <td>6</td>
          <td>汾九</td>
          <td>20</td>
        </tr>
      </tbody>
      <tfoot>
        <tr class="lastline">
          <td colspan="4"><input id="del" type="button" value="删除" /></td>
        </tr>
      </tfoot>
    </table>
  </body>
</html>
```