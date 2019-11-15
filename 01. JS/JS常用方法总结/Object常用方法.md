# Object 的常见方法

## Object.values(obj) 

```js
// Object.values() 方法, 传入一个对象参数
/**
	Object.values()返回一个数组，其元素是在对象上找到的可枚举属性值。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。
	   即:  返回的是 对象属性对应的属性值, 是一个数组, 这时候就可以调用数组的方法遍历
*/
```

> :triangular_flag_on_post:  **实例**

```js
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object  像数组对象  (即: 类数组对象 或 伪数组对象)
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']

// getFoo is property which isn't enumerable 可枚举的
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']

// non-object argument will be coerced(强迫,强制) to an object
console.log(Object.values('foo')); // ['f', 'o', 'o']
```

