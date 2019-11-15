[TOC]

# JS中this关键字的理解

> 本质上,  任何函数在执行时, 都是通过某个对象来调用的. 比如我们常用的定时器函数.
>
> setInterval();   实际上时通过 window.setInterval  来调用的.

## 1.1 首先 this 是什么?

> **this本质:**  **this本质是一个关键字,  变量.** 

> **this分类:** 
>
> 1. 全局this:     this ===  window
> 2. 局部this:      this  === 调用其的对象
>
> 自调用函数的 this 是 window

```js
this 是一个关键字, 一个内置的引用变量,   在函数中都可以直接使用 this.  
this 代表调用该函数的当前对象
在定义函数时, this 还没有确定,  只有在执行时才动态确定(绑定)的.
```

## 1.2 函数中 this 的指向问题

> **通过 call,  apply 方法执行的函数, 可以强制绑定 this指向**  

```js
       /**
        * 函数中的this的指向问题 ???
        *
        * 普通函数 的 this 是谁?     ----->  window
        * 定时器方法中的 this是谁     -----> window
        *
        * 对象.方法中的 this 是谁?    ----> 	  当前的实例对象
        *构造函数中的this是谁?        ------>     实例对象
        * 原型对象方法中的this是谁?   ------>  	 实例对象
        */
       /**
        * BOD: 中顶级对象是 window, 浏览器所有的东西都是 window的
        */

       // 普通函数中  this    ----> window
       // function f1() {
       //     console.log(this);
       // }
       // f1();
       
       
       // 定时器中 this  ----> window
       // setInterval(function () {
       //     console.log(this);
       // },1000)

       function Person() {
           console.log(this);
           this.sayHello1 = function () {
               console.log(this);
           }
       }
       Person.prototype.eat = function () {
           console.log(this);
       };
       var per = new Person();
       per.sayHello1();
       console.log(per);
       per.eat();
```

## 1.3 严格模式中 this 指向问题

```js
 // 严格模式
    "use strict";  // 开启严格模式
    function f1() {
        console.log(this);  // 普通模式 window
    }
    f1();    //  严格模式下,此时 this 就是  undefined
    window.f1(); // ---> window
```









































































