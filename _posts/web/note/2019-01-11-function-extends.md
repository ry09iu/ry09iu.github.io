---
layout: article
title: 'JavaScript 函数的继承'
date: 2019.01.11 12:23:01
key: web_20190111_1
category: 
- Web 
- note
tags:
- 前端开发
- JavaScript
---

关于 JS 函数的继承总结

<!-- more -->

### 函数的继承

- 每个构造函数都有一个`prototype`属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承
- 构造函数的属性

    ```javascript
    funcion A(name) {
        this.name = name; // 实例基本属性 (该属性，强调私有，不共享)
        this.arr = [1]; // 实例引用属性 (该属性，强调私用，不共享)
        this.say = function() { // 实例引用属性 (该属性，强调复用，需要共享)
            console.log('hello')
        }
    }
    
    注意：数组和方法都属于‘实例引用属性’，但是数组强调私有、不共享的。方法需要复用、共享。

    注意：在构造函数中，一般很少有数组形式的引用属性，大部分情况都是：基本属性 + 方法。
    ```

- 原型对象  
    用途是为每个实例对象存储共享的方法和属性，故原型对象只是一个普通的对象而已。原型对象仅有一份，实例可以有很多份。为了属性（实例基本属性）的私有性、以及方法（实例引用属性）的复用、共享，提倡：

      1）将属性封装在构造函数中
      2）将方法定义在原型对象中
    
- 继承的分类

      1、构造继承
      2、原型继承
      3、实例继承
      4、拷贝继承
    
    [js继承、构造函数继承、原型链继承、组合继承、组合继承优化、寄生组合继承](https://segmentfault.com/a/1190000015216289) 
    
    [一篇文章理解JS继承——原型链/构造函数/组合/原型式/寄生式/寄生组合/Class extends](https://segmentfault.com/a/1190000015727237)
- 构造函数的继承

    * 构造函数绑定  
    
    ```javascript
        function Animal() {
          this.species = "动物";
        }
        function Cat(name, color) {
          Animal.apply(this, arguments);
          this.name = name;
          this.color = color;
        }
        var cat1 = new Cat("大毛", "黄色");
        console.log(cat1.species);
    ```

    * Prototype 模式

    ```javascript
        function Animal() {
          this.species = "动物";
        }
        function Cat(name, color) {
          this.name = name;
          this.color = color;
        }
        Cat.prototype = new Animal();
        Cat.prototype.constructor = Cat;
        var cat1 = new Cat("大毛", "黄色");
        console.log(cat1.species);
    ```
    * 直接继承 prototype
    ```javascript
        function Cat(name, color) {
          this.name = name;
          this.color = color;
        }
        function Animal() {};
        Animal.prototype.species = "动物";
        Cat.prototype = Animal.prototype;
        Cat.prototype.constructor = Cat;
        var cat1 = new Cat("大毛", "黄色");
        console.log(cat1.species);
    ```
    * 利用空对象作为中介
    ```javascript
        function Cat(name, color) {
          this.name = name;
          this.color = color;
        }
        function extend(Child, Parent) {
          var F = function() {};
          F.prototype = Parent.prototype;
          Child.prototype = new F();
          Child.prototype.constructor = Child;
          Child.uber = Parent.prototype; 
          // 为子对象设一个uber属性，这个属性直接指向父对象的prototype属性。
          //（uber是一个德语词，意思是"向上"、"上一层"。）
          // 这等于在子对象上打开一条通道，可以直接调用父对象的方法。
          // 这一行放在这里，只是为了实现继承的完备性，纯属备用性质。
        }
        function Animal() {};
        Animal.prototype.species = "动物";
        extend(Cat, Animal);
        var cat1 = new Cat("大毛", "黄色");
        console.log(cat1.species);
    ```
    * 拷贝继承
    ```javascript
        function Cat(name, color) {
          this.name = name;
          this.color = color;
        }
        function extend2(Child, Parent) {
          var p = Parent.prototype;
          var c = Child.prototype;
          for (var i in p) {
            c[i] = p[i];
          }
          c.uber = p;
        }
        function Animal() {}
        Animal.prototype.species = "动物";
        extend2(Cat, Animal);
        var cat1 = new Cat("大毛", "黄色");
        console.log(cat1.species);
    ```
- 非构造函数的继承
    * object() 方法
    ```javascript
        var Chinese = {
          nation: "中国"
        };
        function object(o) {
          function F() {}
          F.prototype = o;
          return new F();
        }
        var Doctor = object(Chinese);
        Doctor.career = "医生";
        console.log(Doctor.nation); // 医生
    ```
    * 浅拷贝
    ```javascript
        var Chinese = {
          nation: "中国"
        };
        function extendCopy(p) {
          var c = {};
          for (var i in p) {
            c[i] = p[i];
          }
          c.uber = p;
          return c;
        }
        var Doctor = extendCopy(Chinese);
        Doctor.career = "医生";
        console.log(Doctor.nation); // 医生
        
        // 这样的拷贝有一个问题
        // 那就是，如果父对象的属性等于数组或另一个对象
        // 那么实际上，子对象获得的只是一个内存地址，而不是真正拷贝
        // 因此存在父对象被篡改的可能
        
        
        Chinese.birthPlaces = ["北京", "上海", "香港"];
        var Doctor = extendCopy(Chinese);
        Doctor.birthPlaces.push("厦门");
        console.log(Doctor.birthPlaces); //北京, 上海, 香港, 厦门
        console.log(Chinese.birthPlaces); //北京, 上海, 香港, 厦门
        // 可见 Chinese 的出生地也被改变了
    ```
    * 深拷贝
    ```javascript
        var Chinese = {
          nation: "中国"
        };
        function deepCopy(p, c) {
          var c = c || {};
          for (var i in p) {
            if (typeof p[i] === "object") {
              c[i] = p[i].constructor === Array ? [] : {};
              deepCopy(p[i], c[i]);
            } else {
              c[i] = p[i];
            }
          }
          return c;
        }
        Chinese.birthPlaces = ["北京", "上海", "香港"];
        var Doctor = deepCopy(Chinese);
        Doctor.birthPlaces.push("厦门");
        console.log(Doctor.birthPlaces); //北京, 上海, 香港, 厦门
        console.log(Chinese.birthPlaces); //北京, 上海, 香港
    ```