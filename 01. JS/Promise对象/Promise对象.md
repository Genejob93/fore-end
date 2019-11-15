[TOC]

# Promise对象

> 1. **Promise 是什么?** 
> 2. **为什么要用 Promise? **
> 3. **怎么使用 Promise/Promise的语法?**  
> 4.  **自定义 Promise**   

> **目标: 自定义Promise** 
>
> ==Promise 是 ES 的基本语法==: MDN 搜索 promise 可以查看 promise的文档

## 1.0 Promise 是什么?

> ==**回调函数的分类**==  
>
> 1. **同步执行的:**  数组遍历相关的回调函数  / Promise 的 excutor 函数
> 2. **异步执行的:** 定时器回调 / ajax 回调 / Promise的成功 | 失败的回调
>
> **Promise 到底是什么?** 
>
> ```js
> #1. 抽象表达:
> 	  promise 中, 实现异步的新的解决方案 (以前没有promise之前, js中是用回调函数解决异步的请求)
> #2. 具体表达:
> 		1. promise对象用于表示一个异步操作的最终状态(完成或失败). 以及该操作的结果值. 
>     2. Promise对象是一个代理对象(代理一个值). 被代理的值在 Promise对象创建时可能是未知的,它允许你为异步操作的成功和失败分别绑定相应的处理方法(handlers).这让异步方法可以像操作同步方法那样返回值. 但并不是立即返回最终执行结果, 而是一个能代表未来出现结果的 promise对象
> ```
>
> **Promise 有三种状态的值** 
>
> ```js
> pending	: 初始状态, 既不是成功, 也不是失败状态
> fulfilled:  意味着操作成功完成
> rejected:		意味着操作失败
> 
> # .then() 方法执行之后, 又返回一个新的 promise() 实例对象. 所以可以实现链式调用
> # then() 返回的是一个新的 promise对象, 链式调用时, 后面的 .then() 方法的结果 由 之前的then()  指定的回调函数的结果来决定.
> ```

## 1.2 为什么用 Promise

> 
>
> ```js
> /* 
> 1). 指定回调函数的方式更加灵活: 
>   旧的: 必须在启动异步任务前指定
>   promise: 启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数(甚至可以在异步任务结束后指定)
> 2). 支持链式调用, 可以解决回调地狱问题
>   什么是回调地狱? 回调函数嵌套调用, 外部回调函数异步执行的结果是嵌套的回调函数执行的条件
>   回调地狱的缺点?  不便于阅读 / 不便于异常处理
>   promise链式调用解决
>   async/await终极解决方案
>  */
> 
> # ----------------------------------------------------------
>   /* 
>       1). Promise构造函数: Promise (excutor) {} // 在执行器函数里面执行异步任务
>       2). excutor函数: 同步执行的, 接收两个参数  (resolve, reject) => {} // 执行器函数我们定义的
>       3). resolve函数: 内部定义成功时我们调用的函数 value => {}   // 由引擎内部定义, 我们来调用的
>       4). reject函数: 内部定义失败时我们调用的函数 reason => {}
>       5). Promise.prototype.then方法: (onResolved, onRejected) => {} // 参数又是两个函数
>       6). onResolved函数: 成功的回调函数  (value) => {}
>       7). onRejected函数: 失败的回调函数 (reason) => {}
>       8). Promise.prototype.catch方法: (onRejected) => {}
> 
>       // Promise() 函数对象的方法.  ----------------  
>       9). Promise.resolve方法: (value) => {}
>       10). Promise.reject方法: (reason) => {}
>       11). Promise.all方法: (promises) => {}  // 都要成功才会执行
>       12). Promise.race方法: (promises) => {}  // 有一个成功就会执行
>      */
> 
> ```

## 1.3 自定义Promise的整体结构

> 自定义 Promise的整体结构
>
> ```js
> /*
> 自定义Promise 的整体结构
>  */
> (function (window) {
>   /*
>   Promise构造函数
>   excutor: 内部同步执行的函数  (resolve, reject) => {}
>    */
>   function Promise(excutor) {
> 
>   }
>   /*
>   为promise指定成功/失败的回调函数
>   函数的返回值是一个新的promise对象
>    */
>   Promise.prototype.then = function (onResolved, onRejected) {
> 
>   }
>   /*
>   为promise指定失败的回调函数
>   是then(null, onRejected)的语法糖
>    */
>   Promise.prototype.catch = function (onRejected) {
> 
>   }
>   /*
>   返回一个指定了成功value的promise对象
>    */
>   Promise.resolve = function (value) {
> 
>   }
>   /*
>   返回一个指定了失败reason的promise对象
>    */
>   Promise.reject = function (reason) {
> 
>   }
>   /*
>   返回一个promise, 只有promises中所有promise都成功时, 才最终成功, 只要有一个失败就直接失败
>    */
>   Promise.all = function (promises) {
> 
>   }
>   /*
>   返回一个 promise， 一旦某个promise解决或拒绝， 返回的 promise就会解决或拒绝。
>   */
>   Promise.race = function (promises) {
>     
>   }
>   // 暴露构造函数
>   window.Promise = Promise
> })(window)
> ```

## 1.4 实现 Promise.prototype.then/catch()

> 





















## 

## 1.n  自定义的 Promise对象实现

```js
/*
自定义Promise构造函数模块
使用 IIFE定义模块. 
 */
(function (window) {
  /*
  Promise构造函数
  excutor: 同步执行回调函数: (resolve, reject) => {}
   */
  function Promise(excutor) {
    const self = this

    // 1. 初始化属性
    self.status = 'pending' // 状态属性, 初始值为pending, 后面会改变为: resolved/rejected
    self.data = undefined // 用来保存将来产生了成功数据(value)或/失败数据(reason)
    self.callbacks = [] // 用来存储包含待处理onResolved和onRejected回调函数方法的对象的数组

    // 2. 定义resolve和reject两个函数
    /*
    当异步处理成功后应该立即执行的函数
    value: 需要传递给onResolved函数的成功的值
    内部:
      1. 同步修改状态和保存数据
      2. 异步调用成功的回调函数
     */
    function resolve (value) {
      // 如果当前不是pending, 直接结束
      if(self.status!=='pending') { 
        return
      }

      // 1. 同步修改状态和保存数据
      self.status = 'resolved'
      self.data = value
      // 2. 异步调用成功的回调函数
      setTimeout(() => {
        self.callbacks.forEach(obj => {
          obj.onResolved(value)
        })
      })
    }

    /*
    当异步处理失败/异常时后应该立即执行的函数
    reason: 需要传递给onRejected函数的失败的值
    内部:
      1. 同步修改状态和保存数据
      2. 异步调用失败的回调函数
     */
    function reject (reason) { // 如果当前不是pending, 直接结束
      if(self.status!=='pending') {
        return
      }
      // 1. 同步修改状态和保存数据
      self.status = 'rejected'
      self.data = reason
      // 2. 异步调用失败的回调函数
      setTimeout(() => {
        self.callbacks.forEach(obj => {
          obj.onRejected(reason)
        })
      })
    }

    // 3. 执行excutor,并传入定义好的 resolve和 reject两个函数
    try {
      excutor(resolve, reject)
    } catch (error) { // 如果excutor函数中抛出异常, 当前promise失败
      reject(error)
    }
  }
  
  /*
  指定成功和失败后回调函数
  函数的返回值是一个新的promise
   */
  Promise.prototype.then = function (onResolved, onRejected) {
    const self = this

    // 如果onResolved/onRejected不是函数, 可它指定一个默认的函数
    onResolved = typeof onResolved==='function' ? onResolved : value => value  // 指定返回的promise为一个成功状态, 结果值为 value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {throw reason} // 指定返回的promise为一个失败状态, 结果值为reason

    /* 
    专门抽取的用来处理promise成功/失败结果的函数
    callback: 成功/失败的回调函数
    resolve: 新promise的resolve函数
    reject: 新promise的reject函数
    */
    function handle(callback, resolve, reject) {
      // 1. 抛出异常  ===> 返回的promise变为rejected
      try {
        const x = callback(self.data)
        // 2. 返回一个新的promise ===> 得到新的promise的结果值作为返回的promise的结果值
        if(x instanceof Promise) {
          x.then(resolve, reject) // 一旦x成功了, resolve(value), 一旦x失败了: reject(reason)
        } else {
          // 3. 返回一个一般值(undefined) ===> 将这个值作为返回的promise的成功值
          resolve(x)
        }
      } catch (error) {
        reject(error)
      }
    }

    // 返回一个新的promise对象
    return new Promise((resolve, reject) => {
      if (self.status === 'resolved') { // 当前promise已经成功了
        setTimeout(() => {
          handle(onResolved, resolve, reject)
        })
      } else if (self.status === 'rejected') { // 当前promise已经失败了
        setTimeout(() => {
          handle(onRejected, resolve, reject)
        })
      } else { // 当前promise还未确定 pending
        // 将onResolved和onRejected保存起来
        self.callbacks.push({
          onResolved(value) {
            handle(onResolved, resolve, reject)
          },
          onRejected(reason) {
            handle(onRejected, resolve, reject)
          }
        })
      }
    })
  }

  /*
  为promise指定成功/失败的回调函数
  函数的返回值是一个新的promise对象
   */
  Promise.prototype.then = function (onResolved, onRejected) {
    // 保存promise对象
    const self = this

    // 如果成功/失败的回调函数不是function, 指定一个默认实现的函数
    onResolved = typeof onResolved === 'function' ? onResolved : value => value // 向下传递成功的值
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {throw reason} // 向下抛出异常

    /*
    处理成功/失败结果的函数
     */
    function handle(callback, resolve, reject) {
      // 1. 抛出异常  ===> 返回的promise变为rejected
      try {
        const x = callback(self.data)
        // 2. 返回一个新的promise ===> 得到新的promise的结果值作为返回的promise的结果值
        if(x instanceof Promise) {
          x.then(resolve, reject) // 一旦x成功了, resolve(value), 一旦x失败了: reject(reason)
        } else {
          // 3. 返回一个一般值(可能是undefined) ===> 将这个值作为返回的promise的成功值
          resolve(x)
        }
      } catch (error) {
        reject(error)
      }
    }

    // 返回一个新的promsie对象
    return new Promise((resolve, reject) => {
      if (self.status === 'resolved') {
        // 异步执行onResolved
        setTimeout(() => {
          handle(onResolved, resolve, reject)
        })
      } else if (self.status === 'rejected') {
        // 异步执行onRejected
        setTimeout(() => {
          handle(onRejected, resolve, reject)
        })
      } else {
        // 保存待处理的回调函数
        self.callbacks.push({
          onResolved(value) {
            handle(onResolved, resolve, reject)
          },
          onRejected(value) {
            handle(onRejected, resolve, reject)
          }
        })
      }
    })
  }

  /*
  方法返回一个Promise，并且处理拒绝的情况。它的行为与调用Promise.prototype.then(undefined, onRejected) 相同
  then()的语法糖
   */
  Promise.prototype.catch = function (onRejected) {
    return this.then(null, onRejected)
  }

  /*
  返回一个以给定值解析后的Promise 对象
  value也可能是一个promise
   */
  Promise.resolve = function (value) {
    return new Promise((resolve, reject) => {
      if(value instanceof Promise) { // 如果value是一个promise, 取这个promise的结果值作为返回的promise的结果值
        value.then(resolve, reject) // 如果value成功, 调用resolve(val), 如果value失败了, 调用reject(reason)
      } else {
        resolve(value)
      }
    })
  }

  /*
  返回一个带有拒绝原因reason参数的Promise对象。
   */
  Promise.reject = function (reason) {
    return new Promise((resolve, reject) => {
      reject(reason)
    })
  }

  /*
    返回一个 Promise 实例
    只有当promises中所有的都成功了, 返回的promise才成功, 只要有一个失败, 返回的promise就失败了
   */
  Promise.all = function (promises) {
    return new Promise((resolve, reject) => {

      let resolvedCount = 0 // 用来保存已成功的个数
      const promisesLength = promises.length // 所有待处理promise个数
      const values = new Array(promisesLength) // 存储所有成功value的数组
      promises.forEach((p, index) => {
        (function (index) {
          // promises中元素可能不是promise对象, 需要用resolve()包装一下
          Promise.resolve(p).then(
            value => {
              values[index] = value // 保存到values中对应的下标
              resolvedCount++
              // 如果全部成功了, resolve(values)
              if(resolvedCount===promisesLength) {
                resolve(values)
              }
            },
            reason => {
              // 只要一个失败了, reject(reason)
              reject(reason)
            }
          )
        })(index)
      })
    })
  }

  /*
  返回一个 promise，一旦某个promise解决或拒绝， 返回的 promise就会解决或拒绝。
  */
  Promise.race = function (promises) {
    // 返回新的promise对象
    return new Promise((resolve, reject) => {
      // 遍历所有promise
      for (var i = 0; i < promises.length; i++) {
        Promise.resolve(promises[i]).then(
          (value) => {// 只要有一个成功了, 返回的promise就成功了
            resolve(value)
          },
          (reason) => { // 只要有一个失败了, 返回的结果就失败了
            reject(reason)
          }
        )
      }
    })
  }
  
  // 向外暴露Promise
  window.Promise = Promise
  
})(window)
```

































































