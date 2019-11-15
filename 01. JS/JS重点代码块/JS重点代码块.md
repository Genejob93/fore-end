[TOC]

# JS中重点代码块总结

## 1.0 数组去重

### 1.1.1 JS原生方式去重

```js
/**
	数组的 indexof() 方法,会查找当前数组中是否有某个元素, 如果没有返回 -1. 可以作为判断条件使用
*/
let arr = [1,2,3,4,5,4,3,2,1,5,6,3,2,1];
let arr2 = [];
for (let i = 0; i < arr.length; i++) {
    let item = arr[i];
    if (arr2.indexOf(item) === -1) {
        arr2.push(arr[i])
    }
}
console.log(arr2);
```

### 1.1.2 利用ES6语法实现去重

```js
let arr = [1,2,3,4,5,4,3,2,1,5,6,3,2,1];
function uniqArr(arr) {
    let set = new Set(arr);
    let result = [];
    for(let i of set) {
        result.push(i)
    }
    return result;
}
let result = uniqArr(arr);
console.log(result);
```

### 1.1.3 一行代码实现数组去重

> **精简ES6去重:** 
>
> ```js
> console.log("==================精简ES6的set去重 ======================");
> let arr = [1,2,3,4,5,4,3,2,1,5,6,3,2,1];
> // function uniqArr(arr) {
>     // let set = new Set(arr);
>     // let result = [];
>     // for(let i of set) {
>     //     result.push(i)
>     // }
> 
>     // 1. 迭代步骤 1
>     // 注释部分可以简写为 如下:
>     // let result = [...new Set(arr)];
>     // return result;
> 
>     //2. 迭代步骤 2
>     // 然后直接返回,去掉声明 let result 部分
>     // return [...new Set(arr)];
> // }
> 
>     // 3. 迭代步骤 3 把整个函数简写, 执行结果直接返回
>     let uniqArr2 = arr1 => [...new Set(arr1)];
>     console.log(uniqArr2(arr));
> 
> ```



































































































































