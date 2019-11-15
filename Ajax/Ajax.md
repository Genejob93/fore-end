[TOC]

# Ajax

# 1. 原生JS 的 AJAX

## 1.1 Ajax学习

> AJAX 全称为Asynchronous Javascript And XML，就是异步的 JS 和 XML。
>
> 通过AJAX可以在浏览器中向服务器发送异步请求。 最大的优势就是: 无刷新页面获取数据
>
> AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

### 1.1.1XML 简介

> XML 可扩展标记语言。
>
> XML 被设计用来传输和存储数据。
>
> XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来表示一些数据。

### 1.1.2 AJAX 的工作原理

> ​	Ajax的工作原理相当于在用户和服务器之间加了一个**中间层(Ajax引擎)**，使用户操作与服务器响应异步化。

### 1.1.3 Ajax 特点

#### 1.1.3.1 Ajax 的优点

> 1)       可以无需刷新页面而与服务器端进行通信。
>
> 2)       允许你根据用户事件来更新部分页面内容。

#### 1.1.3.2Ajax 缺点

> 1)       没有浏览历史，不能回退
>
> 2)       存在跨域问题
>
> 3)       SEO不友好

## 1.2 Ajax 的使用

### 1.2.1 核心对象

> **XMLHttpRequest**，AJAX的所有操作都是通过该对象进行的。

### 1.2.2 使用步骤

> 1. 创建 XMLHttpRequest 对象
> 2.  给该对象绑定一个 onreadystatechange 事件监听
> 3. 设置请求的:   请求方式,   地址, 参数  open
> 4. 发送请求   send

1. 创建 XMLHttpRequest 对象

   `let xhr = new XMLHttpRequest();`

2. 给该对象绑定一个事件监听, 名称为 onreadystatechange

   ```js
   xhr.onreadystatechange = function () {
     //
   }
   ```

3.  指定发送请求的:  方式,  地址,  参数

   `xhr.open('GET',"http://localhost:3000/xxx?key=value");`

4. 发送请求

   `xhr.send()`

1. **原生的 JS 写 Ajax GET请求代码**

> ```js
> <h2>该页面是测试使用原生JS 发送Ajax_GET 请求</h2>
> <button id="btn">点我使用原生JS 发送Ajax_GET请求</button>
> 
> <script type="text/javascript">
>     /**
>      * 1. 实例化一个 XMLHttpRequest 对象
>      * 2. 给该对象绑定一个事件监听, 名称为 onreadystatechange
>      * 3. 指定发送请求的: 方式, 地址, 参数
>      * 4. 发送请求
>      */
>      let btn = document.getElementById("btn");
>      btn.onclick = function () {
>          
>        //1. 实例化一个 XMLHTTPRequest 对象
>          let xhr = new XMLHttpRequest();
>          //2. 给该对象绑定一个事件监听
>          xhr.onreadystatechange = () => {
>             if (xhr.readyState === 4 && xhr.status === 200 ){
>                 console.log(xhr.response);
>             }
>          };
>          //3. 指定发送请求的方式, 地址 参数
>          xhr.open('GET','http://localhost:3000/test_get?name=Gene&age=23');
>          //4. 发送请求
>          xhr.send();
>      }
> </script>
> ```

2. **原生的 JS 写 Ajax POST 请求的代码** 

> ```js
> <h2>该页面是测试使用原生JS 发送Ajax_GET 请求</h2>
> <button id="btn">点我使用原生JS 发送Ajax_GET请求</button>
> 
> <script type="text/javascript">
>     /**
>      * 1. 实例化一个 XMLHttpRequest 对象
>      * 2.  给该对象绑定一个事件监听, 名称为 onreadystatechange
>      * 3. 指定发送请求的: 方式, 地址, 参数
>      * 4. 发送请求
>      */
>      let btn = document.getElementById("btn");
>      btn.onclick = function () {
>          //1. 创建一个 XMLHttpRequest 实例
>          let xhr = new XMLHttpRequest();
>          //2. 给该对象绑定一个事件监听 onreadystatechange
>          xhr.onreadystatechange = () => {
>              if (xhr.readyState === 4 && xhr.status === 200) {
>                  console.log(xhr.response);
>              }
>          };
>          //3. 指定发送请求的: 方式,  地址,   参数
>          xhr.open('POST',"http://localhost:3000/test_post");
> 
>          // POST请求方式, 一定要在发送请求之前设置 请求所特有的请求 头 !!!!!!!!
>          xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');
>          //4. 发送请求
>          xhr.send("name=Gene&age=23");
>      }
> </script>
> ```

3. **服务器代码实现**  

```js
//1.引入express
let express = require('express')

//2.创建app服务对象
let app = express()
//暴露静态资源
app.use(express.static('public'))
//解析POST请求请求体中以urlencoded形式编码参数
app.use(express.urlencoded({extended:true}))


app.get('/test_get',(request,response)=>{
  console.log('一个GET请求来了',request.query)
  response.send('我是服务器响应GET请求的信息')
})

app.post('/test_post',(request,response)=>{
  console.log(request.body)
  console.log('一个POST请求来了')
  response.send('我是服务器响应POST请求的信息')
})

//4.绑定端口监听
app.listen(3000,(err)=>{
  if (!err) {
    console.log('【不要使用webstorm打开html页面，会存在跨域问题！！用以下地址】')
    console.log('测试原生js发送Ajax-GET请求的地址是：http://localhost:3000/ajax_get.html')
    console.log('测试原生js发送Ajax-POST请求的地址是：http://localhost:3000/ajax_post.html')
  }
  else console.log(err)
})
```

### 1.2.3 解决 该死的 IE 缓存问题

> ​	问题：在一些浏览器中(IE),由于缓存机制的存在，ajax只会发送的第一次请求，剩余多次请求不会在发送给浏览器而是直接加载缓存中的数据。
>
> ​	解决方式：浏览器的缓存是根据url地址来记录的，所以我们只需要修改url地址即可避免缓存问题

```js
xhr.open("get","/testAJAX?t="+Date.now());
```

### 1.2.4 Ajax 的请求状态

> **了解**

```js
xhr.readyState 可以用来查看请求当前的状态
0: 对应常量UNSENT，表示XMLHttpRequest实例已经生成，但是open()方法还没有被调用。
1: 对应常量OPENED，表示send()方法还没有被调用，仍然可以使用setRequestHeader()，设定HTTP请求的头信息。
2: 对应常量HEADERS_RECEIVED，表示send()方法已经执行，并且头信息和状态码已经收到。
3: 对应常量LOADING，表示正在接收服务器传来的body部分的数据，如果responseType属性是text或者空字符串，responseText就会包含已经收到的部分信息。
4: 对应常量DONE，表示服务器数据已经完全接收，或者本次接收已经失败了

```

## 1.3 使用 promise 封装原生的 Ajax

> **封装的Ajax 实现代码**

```js
 let btn1 = document.getElementById('btn1')
  let btn2 = document.getElementById('btn2')
  
  btn1.onclick = function () {
    sendAjax('http://localhost:3000/test_get','GET',{m:1,n:2}).then((data)=>{
      console.log(data)
    }).catch((err)=>{
      console.log(err)
    })
  }
  btn2.onclick = function () {
    sendAjax('http://localhost:3000/test_post','POST',{m:3,n:4}).then((data)=>{
      console.log(data)
    }).catch((err)=>{
      console.log(err)
    })
  }
  
  /*;(async()=>{
    let {data} = await sendAjax('http://localhost:3000/test_get','GET',{m:1,n:2})
    let {data2} = await sendAjax('http://localhost:3000/test_get','GET',{data})
    let {data3} = await sendAjax('http://localhost:3000/test_get','GET',{data2})
  })()*/
  
  function sendAjax(url,method,data) {
    return new Promise((resolve,reject)=>{
      //1.创建xhr对象
      let xhr = new XMLHttpRequest()
      //2.绑定监听
      xhr.onreadystatechange = function () {
        if(xhr.readyState !== 4){
          return
        }
        if(xhr.readyState === 4 && (xhr.status >= 200 && xhr.status<= 299)){
            const responseObj = {
              data:xhr.response,
              status:xhr.status,
              statusText:xhr.statusText
            }
            resolve(responseObj)
        }else{
            reject(new Error('请求出错了'))
        }
      }
      //3.设置请求的方式，地址，携带的参数
      let dataKeys = Object.keys(data)
      //4.将传递过来的数据对象加工成urlencoded形式的编码
      let str = dataKeys.reduce(function (pre,now) {
        return pre+=`${now}=${data[now]}&`
      },'')
      //5.发送请求
      if(method.toLowerCase() === 'get'){
        url += `?${str}`
        xhr.open(method,url)
        xhr.send()
      }else if (method.toLowerCase() === 'post'){
        xhr.open(method,url)
        xhr.setRequestHeader('content-type','application/x-www-form-urlencoded')
        xhr.send(str)
      }
    })
  }
```

> **服务器代码** 

```js
//1.引入express
let express = require('express')

//2.创建app服务对象
let app = express()
//暴露静态资源
app.use(express.static('public'))
//解析POST请求请求体中以urlencoded形式编码参数
app.use(express.urlencoded({extended:true}))


app.get('/test_get',(request,response)=>{
  console.log('一个GET请求来了',request.query)
  response.send('我是服务器响应GET请求的信息')
})

app.post('/test_post',(request,response)=>{
  console.log(request.body)
  console.log('一个POST请求来了')
  response.send('我是服务器响应POST请求的信息')
})

//4.绑定端口监听
app.listen(3000,(err)=>{
  if (!err) {
    console.log('【不要使用webstorm打开html页面，会存在跨域问题！！用以下地址】')
    console.log('测试原生js发送Ajax-GET请求的地址是：http://localhost:3000/ajax_get.html')
    console.log('测试原生js发送Ajax-POST请求的地址是：http://localhost:3000/ajax_post.html')
    console.log('测试自己封装的Ajax请求的地址是：http://localhost:3000/ajax_with_promise.html')
  }
  else console.log(err)
})
```

# 2. jQquery 中的 Ajax

## 2.1 Get 请求

>  $.get(url, [data], [callback], [type])
>
> url:请求的URL地址。
>
> data:请求携带的参数。
>
> callback:载入成功时回调函数。
>
> type:设置返回内容格式，xml, html, script, json, text, _default。

1.  **使用 jQuery 发送 Ajax-GET请求(标准写法)** 

   ```js
     /*
       * 使用jQuery发送Ajax-GET请求几个常用的参数
       *   -method:发送请求的方式
       *   -data:要传递的数据
       *   -success:成功的回调
       *   -error:失败的回调
       * */
       
       //使用jQuery发送Ajax-GET请求（标准写法）
       $.ajax('http://localhost:3000/test_get',{
         method:'GET',
         data:{name:'kobe',age:18},
         success:function (result) {
           console.log(result)
         },
         error:function (err) {
           console.log(err)
         }
       })
   ```

2. **使用 jQuery 发送 Ajax-GET请求(简写)**

   ```js
   $.get('http://localhost:3000/test_get',{name:'kobe',age:18},(data)=>{
           console.log(data)
       })
   ```

## 2.2 Post 请求

> $.post(url, [data], [callback], [type])
>
> url:请求的URL地址。
>
> data:请求携带的参数。
>
> callback:载入成功时回调函数。
>
> type:设置返回内容格式，xml, html, script, json, text, _default。

1. **使用jQuery 发送Ajax-POST请求(标准写法)** 

   ```js
   $.ajax('http://localhost:3000/test_post',{
         method:'POST',
         data:{name:'kobe',age:18},
         success:function (result) {
           console.log(result)
         },
         error:function (err) {
           console.log(err)
         }
       })
   ```

2. **使用jQuery 发送 Ajax-POST请求(简写)** 

   ```js
    $.post('http://localhost:3000/test_post',{name:'kobe',age:18},(data)=>{
         console.log(data)
    })
   ```



# 3.0 axios

# 4.0 fetch

# 5.0 fly



































































































































































