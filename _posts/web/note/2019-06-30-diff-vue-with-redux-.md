---
layout: article
title: 'React 和 Vue 两大框架之间的区别'
date: 2019.06.30 17:28:04
key: web_20190630_1
category: 
- Web 
- note
tags:
- 前端开发
---

React 和 Vue 两大框架之间的相爱相杀，他们之间到底有哪些区别？

<!-- more -->

### 什么是 MVVM？比之 MVC 有什么区别？
- 在 MVVM 架构中，引入了 ViewModel 的概念。ViewModel 只关心数据和业务的处理，不关心 View 如何处理数据，在这种情况下，View 和 Model 都可以独立出来，任何一方改变了也不一定需要改变另一方，并且可以将一些可复用的逻辑放在一个 ViewModel 中，让多个 View 复用这个 ViewModel。
- 对于 MVVM 来说，其实最重要的并不是通过双向绑定或者其他的方式将 View 与 ViewModel 绑定起来，而是通过 ViewModel 将视图中的状态和用户的行为分离出一个抽象，这才是 MVVM 的精髓。

### 什么是 Virtual DOM？为什么 Virtual DOM 比原生 DOM 快？
- Virtual DOM 即 虚拟DOM
- 用 JS 模拟 DOM 结构
- DOM 变化的对比，放在 JS 层来做
- 提高重绘性能

优势：
- 将 Virtual DOM 作为一个兼容层，让我们还能对接非 Web 端的系统，实现跨端开发。
- 同样的，通过 Virtual DOM 我们可以渲染到其他的平台，比如实现 SSR、同构渲染等等。
- 实现组件的高度抽象化

### 前端路由原理？两种实现方式有什么区别？
前端路由实现起来其实很简单，本质就是监听 URL 的变化，然后匹配路由规则，显示相应的页面，并且无须刷新页面。
- Hash 模式
  www.test.com/#/ 就是 Hash URL，当 # 后面的哈希值发生变化时，可以通过 hashchange 事件来监听到 URL 的变化，从而进行跳转页面，并且无论哈希值如何变化，服务端接收到的 URL 请求永远是 www.test.com。
- History 模式
  History 模式是 HTML5 新推出的功能，主要使用 history.pushState 和 history.replaceState改变 URL。
  通过 History 模式改变 URL 同样不会引起页面的刷新，只会更新浏览器的历史记录。
  当用户做出浏览器动作时，比如点击后退按钮时会触发 popState 事件

两种方式区别：
- Hash 模式只可以更改 # 后面的内容，History 模式可以通过 API 设置任意的同源 URL
- History 模式可以通过 API 添加任意类型的数据到历史记录中，Hash 模式只能更改哈希值，也就是字符串
- Hash 模式无需后端配置，并且兼容性好。History 模式在用户手动输入地址或者刷新页面的时候会发起 URL 请求，后端需要配置 index.html 页面用于匹配不到静态资源的时候

### Vue 和 React 之间的区别
- Vue 的表单可使用 v-model 支持双向绑定，比 React 方便，但 v-model 实质是语法糖，二者实质一样
- 改变数据方式不一样，React 需要使用 setState 来改变状态，Vue 比较简单
- Vue 底层使用了依赖追踪，页面更新渲染已经是最优的，React 还需要手动进行优化
- React 使用 JSX，有一定上手成本，且需要工具链支持，但完全可以通过 JS 来控制页面
  Vue 使用模板语言，没有 JSX 灵活，但可以脱离工具链，直接编辑 render 函数即可运行在浏览器
