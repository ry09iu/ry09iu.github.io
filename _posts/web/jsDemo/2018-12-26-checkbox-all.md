---
layout: article
title: '[JavaScript 实例] 复选框全选和取消的实现'
date: 2018.11.03 18:38:04
key: jsdemo_20181103_1
category: 
- Web 
- Jsdemo
tags:
- JS 实例
---

### 目标
通过勾选“全选”或“取消”，可控制复选框内所有选项的选中或取消

<!-- more -->

实现代码如下

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>复选框全选和取消的实现</title>
    <style>
        .list {
            list-style: none;
        }
    </style>
    <script>
        window.onload = function () {
            var subjects = document.getElementsByName("subject");
            var chk = document.getElementById("chk");
            var all = document.getElementById("all");
            chk.onclick = function () {
                if (this.checked == true) {
                    for (var i = 0; i < subjects.length; i++) {
                        subjects[i].checked = true;
                    }
                    all.innerHTML = "取消";
                } else {
                    for (var i = 0; i < subjects.length; i++) {
                        subjects[i].checked = false;
                    }
                    all.innerHTML = "全选";
                }
            }
        }
    </script>
</head>
<body>
    <div>
        <p>选择你喜欢的科目</p>
        <ul class="list">
            <li>
               <input type="checkbox" name="subject" />语文
            </li>
            <li>
                <input type="checkbox" name="subject" />数学
            </li>
            <li>
                <input type="checkbox" name="subject" />英语
            </li>
        </ul>
        <input type="checkbox" id="chk" />
        <span id="all">全选</p>
    </div>
</body>
</html>
```