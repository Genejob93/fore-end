[TOC]

# JS 常用方法总结

## Object(对象)常用方法

### Object.keys(obj)

> **Object.keys(obj)** 
>
> ​	 该方法传入一个对象, 返回对象的所有属性所组成的数组.   数组中属性名的排列顺序和使用 `for...in` 循环遍历该对象时, 返回的顺序一致. (两者的主要区别是 一个 for..in 循环会枚举其原型链上的属性).

> **Object.keys(obj)** 
>
> 1. 该方法遍历数组对象时, 返回的是数组的索引下标组成的数组.
>
> ```js
> let arr = ['a','b','c','d'];
> console.log(Object.keys(arr)); // 打印结果 [ '0', '1', '2', '3' ]
> ```
>
> 2. 该方法 遍历普通对象时,  返回的是 对象的可迭代属性`keys`组成的数组
>
> ```js
> let obj = {
>     name:"Gene",
>     age:23,
>     sex:"male",
>     address:"河北邯郸"
> }
> let keys = Object.keys(obj);
> console.log(keys);  // 打印结果: [ 'name', 'age', 'sex', 'address' ]
> ```
>
> 

# Array(数组) 常用方法

## reduce方法

```js
/*
  * 【数组的reduce方法】：
  *   1.reduce方法接收一个函数作为累加器（“连续操作器”）。
  *   2.数组中的每个值（从左到右）开始合并（不一定是相加！），最终合为一个值。
  *   3.reduce为数组中的每一个元素【依次执行】回调函数，但不包括数组中被删除或从未被赋值的元素。
  *   4.reduce方法最终返回的是最后一次调用累加器("连续操作器")的结果。
  *   5.累加器函数接受四个参数：preValue, nowValue, nowIndex, arr
  *       --preValue：
  *         --第一次调用时是初始值，如果初始值没有指定，就是数组中第一个元素的值，同时nowValue变为数组中的第二个值。
  *         --以后调用时是上一次该回调函数的返回值；
  *       --nowValue：当前元素值；
  *       --nowIndex：当前元素索引；
  *       --arr：调用 reduce方法的当前数组；
  *
  * 参数说明：
  *   array.reduce(function(preValue, nowValue, nowIndex, arr){}, initialValue)
  *
  * 注意：
  *     1.如果initialValue在调用时被提供，那么第一次的preValue就等于initialValue，nowValue等于数组中的第一个值；
  *     2.如果initialValue未被提供，那么preValue等于数组中的第一个值，nowValue自动等于数组中的第二个值。
  *
  * */

    let arr = [2,4,6,8];
    let result = arr.reduce((preValue,nowValue,nowIndex,arr) => {
        console.log(preValue,nowValue,nowIndex,arr);
        return preValue + nowValue;
    },100);  // 回调函数后面的值 100 为 preValue 的初始值
    console.log(result);
```





## 数组方法

```
find   filter
```





c



















































































































































