[TOC]

# JS模块化

# 1.0  什么是模块 / 模块化

## 1.1 模块 

> **什么是模块?** 
>
> ```js
> 1. 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件),并进行组合在一起
> 
> 2. 这些拆分的文件就是模块, 模块内部数据 / 实现 是私有的, 只是向外暴露一些接口(方法)与外部其他模块通信
> ```
>
> **一个模块的组成** 
>
> ```js
> /**
>  * 数据 ---> 内部的属性
>  * 操作数据的行为 ---> 内部的函数
>  */
> ```

## 1.2 模块化

> **常用的模块化语法** 

> **CommonJs模块化** 
>
> **ES6模块化** 
>
> AMD
>
> CMD 

> **什么是模块化?** 
>
> ```js
> 当开发的项目是遵循模块化规范的项目, 那么这个项目就是模块化的项目
> ```

## 1.3 模块化进化史

> 1. **全局 function模式** 
>
>    > **说明:** 
>    >
>    > ```js
>    > 全局函数模式:  将不同的功能封装成不同的全局函数
>    > 问题: Global 被污染了, 很容易引起命名冲突
>    > ```
>
>    - module1.js
>
>      ```js
>      //数据
>      let data = 'atguigu.com'
>      
>      //操作数据的函数
>      function foo() {
>        console.log(`foo() ${data}`)
>      }
>      function bar() {
>        console.log(`bar() ${data}`)
>      }
>      ```
>
>    - module2.js
>
>      ```js
>      let data2 = 'other data';
>      
>      function foo() {  //这里与另一个模块中的函数冲突了
>        console.log(`foo() ${data2}`)
>      }
>      ```
>
>    - test.html
>
>      ```js
>      <script type="text/javascript" src="module1.js"></script>
>      <script type="text/javascript" src="module2.js"></script>
>      <script type="text/javascript">
>        let data = "我是修改后的数据"
>        foo()
>        bar()
>      </script>
>      ```
>
> 2. **namespace 模式** 
>
>    > - module1.js
>    >
>    >   ```js
>    >   let myModule = {
>    >     data: 'module1 atguigu.com',
>    >     foo() {
>    >       console.log(`foo() ${this.data}`)
>    >     },
>    >     bar() {
>    >       console.log(`bar() ${this.data}`)
>    >     }
>    >   }
>    >   ```
>    >
>    > - module2.js
>    >
>    >   ```js
>    >   let myModule2 = {
>    >     data: 'module2 atguigu.com',
>    >     foo() {
>    >       console.log(`foo() ${this.data}`)
>    >     },
>    >     bar() {
>    >       console.log(`bar() ${this.data}`)
>    >     }
>    >   }
>    >   ```
>    >
>    > - test.html
>    >
>    >   ```js
>    >   <script type="text/javascript" src="module2.js"></script>
>    >   <script type="text/javascript" src="module22.js"></script>
>    >   <script type="text/javascript">
>    >     myModule.foo()
>    >     myModule.bar()
>    >   
>    >     myModule2.foo()
>    >     myModule2.bar()
>    >   
>    >     //可以直接修改模块内部的数据
>    >     myModule.data = 'other data' 
>    >     myModule.foo()
>    >   </script>
>    >   ```
>    >
>    > **说明:** 
>    >
>    > 1. namespace模式:  简单的对象封装
>    > 2. 作用:  减少了全局变量
>    > 3. 问题: 依然可以修改内部代码, 不安全
>
> 3. **IIFE模式**
>
>    > - module1.js
>    >
>    >   ```js
>    >   (function (window) {
>    >     //数据
>    >     let data = 'atguigu.com'
>    >   
>    >     //操作数据的函数
>    >     function foo() { //向外暴露的内部私有函数
>    >       console.log(`foo() ${data}`)
>    >     }
>    >   
>    >     function bar() {//向外暴露的内部私有函数
>    >       console.log(`bar() ${data}`)
>    >       otherFun() //内部调用
>    >     }
>    >   
>    >     function otherFun() { //未暴露的内部私有函数
>    >       console.log('otherFun()')
>    >     }
>    >   
>    >     //暴露行为
>    >     window.myModule = {foo, bar}
>    >   })(window)
>    >   ```
>    >
>    > - test.html
>    >
>    >   ```js
>    >   <script type="text/javascript" src="module3.js"></script>
>    >   <script type="text/javascript">
>    >     myModule.foo()
>    >     myModule.bar()
>    >     //myModule.otherFun()  //报错：myModule.otherFun is not a function
>    >     console.log(myModule.data) //undefined 不能访问模块内部数据
>    >     myModule.data = 'xxxx' //并不是修改的模块内部的data
>    >     myModule.foo() //未受影响
>    >   
>    >   </script>
>    >   ```
>    >
>    > **说明:** 
>    >
>    > 1. IIFE模式:  匿名函数自调用(闭包)
>    > 2. IIFE: immediately-invoked function expression(立即调用函数表达式)
>    > 3. 作用: 数据是私有的, 外部只能通过暴露的方法操作
>    > 4. **问题:**  如果当前这个模块依赖另一个模块比较麻烦
>
> 4. **IIFE 模式增强**
>
>    > - 引入 jquery 到项目中
>    >
>    > - module4.js
>    >
>    >   ```js
>    >   (function (window, $) {
>    >     //数据
>    >     let data = 'atguigu.com'
>    >   
>    >     //操作数据的函数
>    >     function foo() { //用于暴露有函数
>    >       console.log(`foo() ${data}`)
>    >       $('body').css('background', 'red')
>    >     }
>    >   
>    >     function bar() {//用于暴露有函数
>    >       console.log(`bar() ${data}`)
>    >       otherFun() //内部调用
>    >     }
>    >   
>    >     function otherFun() { //内部私有的函数
>    >       console.log('otherFun()')
>    >     }
>    >   
>    >     //暴露行为
>    >     window.myModule = {foo, bar}
>    >   })(window, jQuery)
>    >   ```
>    >
>    > - test4.html
>    >
>    >   ```js
>    >   <script type="text/javascript" src="jquery-1.10.1.js"></script>
>    >   <script type="text/javascript" src="module4.js"></script>
>    >   <script type="text/javascript">
>    >     myModule.foo()
>    >   </script>
>    >   ```
>    >
>    > **说明:** 
>    >
>    > 1. IIFE模式增强: 引入依赖
>    > 2. 这就是现代模块化的基石
>
> 5. **页面加载多个 JS 的问题** 
>
>    > - **页面** 
>    >
>    >   ```js
>    >   <script type="text/javascript" src="module1.js"></script>
>    >   <script type="text/javascript" src="module2.js"></script>
>    >   <script type="text/javascript" src="module3.js"></script>
>    >   <script type="text/javascript" src="module4.js"></script>
>    >   <script type="text/javascript" src="module5.js"></script>
>    >   <script type="text/javascript" src="module6.js"></script>
>    >   <script type="text/javascript" src="module7.js"></script>
>    >   <script type="text/javascript" src="module8.js"></script>
>    >   <script type="text/javascript" src="module9.js"></script>
>    >   <script type="text/javascript" src="module10.js"></script>
>    >   <script type="text/javascript" src="module11.js"></script>
>    >   <script type="text/javascript" src="module12.js"></script>
>    >   ```
>    >
>    > **说明:** 
>    >
>    > 1. 一个页面需要引入多个 js 文件
>    > 2. **问题:** 
>    >    - 请求过多
>    >    - 依赖模糊
>    >    - 难以维护
>    > 3. 这些问题可以通过现代模块化编码和项目构建来解决
>
> 

# 2.0 CommonJS模块化

> **CommonJS 模块化是支持 ==服务器端== 和 ==浏览器端==的一种模块化语法**            

## 2.1 CommonJS 模块化(服务器端)

> [CommonJS模块化规范]( http://wiki.commonjs.org/wiki/Modules/1.1)       

1. **CommonJS 模块化 说明** 

   > 每一个 js 文件都可以当做一个模块
   >
   > **在服务器端**: 模块的加载时运行时同步加载的
   >
   > **在浏览器端:** 模块需要提前编译打包处理

2. **CommonJS 模块化基本语法**

   > | 暴露模块               | 引入模块                                    |
   > | ---------------------- | ------------------------------------------- |
   > | module.exports = value | 第三方模块:require(xxx),xxx为模块名         |
   > | exports.xxx = value    | 自定义模块:require(xxx),  xxx为模块文件路径 |

   > **暴露模块** 
   >
   > ```js
   > // 不能暴露多次, 多次暴露,以最后暴露的那个为准.(发生了覆盖重写)
   > 
   > /**
   >  * 注意: 在 commonJS 模块化中,存在这样一个关系: module.exports = exports = {}
   >   * 在一个模块中  当 module.exports = value 和 exports.xxx 同时暴露时, 以 module.exports 暴露的为准
   > */
   > module.exports = value 
   > 
   > exports.xxx = value
   > ```
   > 
   >**引入模块** 
   > 
   >```js
   > # require(xxx)   // commonJS 的引入方式
   > //1. 第三方模块:   xxx 为模块名
   > //2. 自定义模块:  xxx为模块文件路径
   > 
   > ```

## 2.2 CommonJS 服务器端教程

1. **安装 Node.js**

2. **创建项目结构** 

   ```js
     |-modules
       |-module1.js
       |-module2.js
       |-module3.js
     |-app.js
     |-package.json
       {
         "name": "test-0418",
         "version": "1.0.0"
       }
   ```

3. **模块化编码** 

   > - module1.js
   >
   > ```js
   > module.exports = {
   > data:'module1',
   > foo(){
   >  console.log('foo()------',this.data);
   > },
   > bar(){
   >  console.log('bar()------',this.data);
   > }
   > }
   > ```
   >
   > - module2.js
   >
   > ```js
   > # commonJS 中这种方式只能暴露一次, 一个文件内 用 两次 module.exports, 后面暴露的东西会把前面暴露的覆盖.
   > 
   > module.exports = function () {
   > console.log('module2');
   > }
   > ```
   >
   > - module3.js
   >
   > ```js
   > # commonJS 一个文件内 暴露多个模块 foo, bar ...
   > 
   > exports.foo = function () {
   > console.log('foo()  module3');
   > }
   > 
   > 
   > exports.bar = function () {
   > console.log('bar()  module3');
   > }
   > ```
   >
   > - 下载第三方模块 uniq:  打开左下角的Terminal，cd到指定 路径，输入命令：```npm install uniq --save```
   > - app.js
   >
   > ```js
   > let module1 = require('./modules/module1')
   > let module2 = require('./modules/module2')
   > let module3 = require('./modules/module3')
   > let a = require('uniq')
   > 
   > module1.foo()
   > module1.bar()
   > module2()
   > module3.foo()
   > module3.bar()
   > 
   > let arr = [1,11,2,2,2,5,5,5,3,4,6,6,9,7,8]
   > console.log(a(arr));
   > 
   > ```

4. **在node环境下运行app.js的两种方法(任选其一)：** 

   > - 第一种方法：用命令启动: ```node app.js```
   > - 第二种方法：用工具启动: 右键 --> Run 'xxxxx.js' 

## 2.3 CommonJS 模块化(浏览器端)

1. **创建项目结构** 

   > ```js
   >   |-js
   >     |-dist //生成编译完js的目录
   >     |-src //源码所在的目录（我们编写的、没经过工具处理的代码，叫做源码）
   >       |-module1.js
   >       |-module2.js
   >       |-module3.js
   >       |-app.js
   >   |-index.html
   >   |-package.json
   >     {
   >       "name": "test-0418",
   >       "version": "1.0.0"
   >     }
   > ```

2. **模块化编码**

   > - module1.js
   >
   > ```js
   > module.exports = {
   >   foo() {
   >     console.log('moudle1 foo()')
   >   }
   > }
   > ```
   >
   > -  module2.js
   >
   > ```js
   > module.exports = function () {
   >   console.log('module2()')
   > }
   > ```
   >
   > - module3.js
   >
   > ```js
   > exports.foo = function () {
   >   console.log('module3 foo()')
   > }
   > 
   > exports.bar = function () {
   >   console.log('module3 bar()')
   > }
   > ```
   >
   > - 下载第三方模块uniq：打开左下角的Terminal，cd到指定路径，输入命令：```npm install uniq --save``` 
   >
   > - app.js
   >
   > ```js
   > //引用模块
   > let module1 = require('./module1')
   > let module2 = require('./module2')
   > let module3 = require('./module3')
   > 
   > let uniq = require('uniq')
   > 
   > //使用模块
   > module1.foo()
   > module2()
   > module3.foo()
   > module3.bar()
   > 
   > console.log(uniq([1, 3, 1, 4, 3]))
   > ```

3.  **下载browserify(用于把CommonJS的模块化语法，翻译成浏览器认识的语法，一个“翻译官”)** 

   > - 第一步，执行全局安装命令: ```npm install browserify -g```（若此步骤报错，请使用管理员身份打开webstorm，再次执行即可）
   > - 第二步，执行局部安装命令: ```npm install browserify --save-dev```
   > - 备注：以上两步骤都要执行，缺一不可！

4. **执行处理命令** 

   > - 第一步，cd到指定文件夹（03_CommonJS-Browserify）
   > - 第二步，输入命令```browserify js/src/app.js -o js/dist/bundle.js``` 

5.  **页面中使用引入** 

   > ```js
   > <script type="text/javascript" src="js/dist/bundle.js"></script> 
   > ```


# 3.0 ES6 模块化

> [ES6模块化规范]( http://es6.ruanyifeng.com/#docs/module)   

> **基本语法** 
>
> 1. **导出模块: `export`**
> 2. **引入模块: `import`**  

> **实现:** 
>
> 1. 使用Babel将ES6编译为ES5代码：  babel 源文件路径 -d 编译后文件路径
> 2. 使用Browserify编译打包js    browserify 源文件路径/源文件 -o 编译后文件路径/文件

> **扩展阅读:** 
>
> [前端模块化开发那点历史](https://github.com/seajs/seajs/issues/588)    
>
> [CommonJS，AMD，CMD区别](http://zccst.iteye.com/blog/2215317)   
>
> [AMD和CMD 的区别](http://www.zhihu.com/question/20351507/answer/14859415)    
>
> [Javascript模块化编程](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)    

**==ES6模块化的 3 种暴露方式==**    

> 1. **分别暴露**  
> 2. **统一暴露**  
> 3. **默认暴露**    export default { }

## 3.1 ES6-Babel-Browserify模块化教程

1. **创建项目结构**

   > ```js
   >   |-js
   >     |-src
   >       |-module1.js
   >       |-module2.js
   >       |-module3.js
   >       |-app.js
   >   |-index.html
   >   |-package.json
   >     {
   >     "name" : "es6-modular-0418",
   >     "version" : "1.0.0"
   >   }
   > ```

2. **安装babel-cli, babel-preset-es2015和browserify**

   > 第一步，全局安装：```npm install babel-cli browserify -g ```
   > 第二步，局部安装：```npm install babel-preset-es2015 --save-dev```   

3.  **定义.babelrc文件(给babel指定具体的任务)，内容如下** 

   > ```js
   > {
   >    "presets": ["es2015"]
   > }
   > ```

4.  **编码** 

   > ==注意:==  
   >
   > ES6 模块化中.
   >
   > ```js
   > //1. 引入分别暴露的模块时, 必须使用 大括号的方式引入. (但是这里不是解构赋值)
   > 
   > //2. 引入统一暴露的模块, 也必须使用 大括号的方式引入. (但是这种方式不是解构赋值)
   > ```

   > - **js/src/module1.js**
   >
   >   ```js
   >   // 分别暴露
   >   export function() {
   >     console.log("module1 foo()")
   >   }
   >   export function(){
   >     console.log("module1 bar()")
   >   }
   >   export const DATA_ARR = [1,2,3,4,5]
   >   ```
   >
   > - **js/src/module2.js**   
   >
   >   ```js
   >   //统一暴露的方式 
   >   
   >   let data = "module2 data"
   >   
   >   function fun1(){
   >     console.log("module2 fun1" + data)
   >   }
   >   
   >   function fun2(){
   >     console.log("module2 fun2()" + data)
   >   }
   >   
   >   // 统一暴露 的暴露方式 (简写)
   >   export {fun1,fun2}
   >   
   >   // 完整写法 (统一暴露的方式完整写法)
   >   export {
   >   	fun1 as haha1,
   >     fun2 as haha2,
   >     data as data1 
   >   }
   >   ```
   >
   > - **js/src/module3.js**  
   >
   >   ```js
   >   // 默认暴露的方式 (默认暴露的方式只能暴露一次)
   >   
   >   export default {
   >     name: 'Tom',
   >     setName: function (name) {
   >       this.name = name
   >     }
   >   }
   >   ```
   >
   > -  **下载 jQuery 模块: `npm install jquery --save`**  
   >
   > - js/src/app.js
   >
   >   ```js
   >   // 分别暴露的 引入方式
   >   import {foo, bar} from './module1'
   >   import {DATA_ARR} from './module1'
   >   
   >   // 统一暴露的 引入方式
   >   import {fun1, fun2} from './module2'
   >   
   >   // 默认暴露的 引入方式
   >   import person from './module3'
   >   import $ from 'jquery'
   >   
   >   $('body').css('background', 'red')
   >   
   >   foo()
   >   bar()
   >   console.log(DATA_ARR);
   >   fun1()
   >   fun2()
   >   
   >   person.setName('JACK')
   >   console.log(person.name);
   >   ```

5. **编译源代码**  

   > - 第一步：使用Babel将ES6编译为ES5代码，命令为: ```babel js/src -d js/lib```
   > - 第二步：使用Browserify编译js上一步生成的js，命令为: ```browserify js/lib/app.js -o js/lib/bundle.js```
   > - 备注：第一步操作后Babel将es6的模块化语法，转换成了CommonJS模块化语法（浏览器不识别）， 所以需要第二步用Browserify再次编译成浏览器识别的语法。

6.  **引入页面中进行测试**  

   > ```js
   > <script type="text/javascript" src="js/lib/bundle.js"></script>
   > ```

## 3.2 ES6模块化注意事项

> ```js
> #//1. 引入分别暴露的方式 通常使用大括号引入
>   import {} from './module1'
> 
> // 但是也可以用 下面这种方式引入 as后面的 module1 作为一个对象, 存入所有引进来的 对象, 函数,数组等.
>  import * as module1 from './module1'
> 
> # -----------------------------------------------
> #//2. 统一暴露的方式 通常 也用 大括号方式引入
> 	import {} from './module1'
> 
> 	// 但是也可以使用 下面这种方式引入所有
> 	import * as module2 from './module2'
> 
> // 如果引入的模块是 分别暴露 和 统一暴露, 一般会直接用 import * 的方式引入.
> 
> ```

# 4.0 Amd 模块化规范

> **了解即可**

## require.js使用教程

1. 下载require.js

- 官网: http://www.requirejs.cn/
- github : https://github.com/requirejs/requirejs
- 将require.js导入项目: js/libs/require.js 

2. 创建项目结构

```
  |-js
    |-libs
      |-require.js
    |-modules
      |-loger.js
      |-dataService.js
    |-main.js
  |-index.html
```

3. 定义require.js的模块代码

- dataService.js

  ```
  define(function () {
    let msg = 'atguigu.com'
  
    function getMsg() {
      return msg.toUpperCase()
    }
  
    return {getMsg}
  })
  ```

- loger.js

  ```
  define(['dataService', 'jquery'], function (dataService, $) {
    let name = 'Tom2'
  
    function showMsg() {
      $('body').css('background', 'gray')
      console.log(dataService.getMsg() + ', ' + name)
    }
  
    return {showMsg}
  })
  ```

4. 应用主(入口)js: main.js

```
    requirejs.config({
      //模块标识名与模块路径映射
      paths: {
        "loger": "modules/loger",
        "dataService": "modules/dataService",
      }
    })
  
    //引入使用模块
    requirejs( ['loger'], function(loger) {
      loger.showMsg()
    })
    
```

5. 页面使用模块:

  ```<script data-main="js/main.js" src="js/libs/require.js"></script>```
    

6. 使用第三方基于require.js的框架(jquery)

- 将jquery的库文件导入到项目: 

  - js/libs/jquery-1.10.1.js

- 在main.js中配置jquery路径

  ```
  paths: {
            'jquery': 'libs/jquery-1.10.1'
        }
  ```

- 在loger.js中使用jquery

  ```
  define(['dataService','jquery'],function (dataService,$) {
    function showMsg() {
      console.log(dataService.getData());
      $('body').css('background','skyblue')
    }
    return {showMsg}
  })
  ```

- 备注：define中的要写jquery不可以写jQuery，因为jQuery源码已经对AMD模块化进行了适配，已经定义好了"jquery"

# 5.0 CMD-SealJS模块化教程

> **了解即可**  

## sea.js简单使用教程 

1. 下载sea.js, 并引入

- 官网: http://seajs.org/
- github : https://github.com/seajs/seajs
- 将sea.js导入项目: js/libs/sea.js 

2. 创建项目结构

```
  |-js
    |-libs
      |-sea.js
    |-modules
      |-module1.js
      |-module2.js
      |-module3.js
      |-module4.js
      |-main.js
  |-index.html
```

3. 定义sea.js的模块代码

- module1.js

  ```
  define(function (require,exports,module) {
    var name = 'module1';
  
    function fun() {
      console.log(name);
    }
  
    //暴露模块
    exports.showName = {fun}
  });
  ```

- module2.js

  ```
  define(function (require,exports,module) {
    var name = 'module2';
  
    function fun2() {
      console.log(name);
    }
  
    //暴露模块
    module.exports = fun2
  });
  ```

- module3.js

  ```
  define(function (require,exports,module) {
    var name = 'module3';
  
    function foo() {
      console.log(name);
    }
  
    //暴露模块
    module.exports = {foo}
  });
  ```

- module4.js

  ```
  //module4依赖于module2，module3
  define(function (require,exports,module) {
    var name = 'module4';
  
    function foo() {
      console.log(name);
    }
    //同步引入module2
    let module2 = require('./module2')
    module2()
    //异步引入module3
    require.async('./module3',function (m3) {
      m3.foo()
    })
    //暴露模块
    module.exports = {foo}
  });
  ```

- main.js : 主(入口)模块

  ```
  define(function (require) {
    var m1 = require('./module1')
    var m4 = require('./module4')
    m1.showName.fun()
    m4.foo()
  })
  ```

4. index.html:

```
  <!--
  使用seajs:
    1. 引入sea.js库
    2. 如何定义导出模块 :
      define()
      exports
      module.exports
    3. 如何依赖模块:
      require()
    4. 如何使用模块:
      seajs.use()
  -->
  <script type="text/javascript" src="js/libs/sea.js"></script>
  <script type="text/javascript">
    seajs.use('./js/modules/main')
  </script>
```

5.思考：为什么运行后输出结果如下？   

```
module2
module1
module4
module3
```



















