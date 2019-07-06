---
layout: post
title: es6常用语法
date: '2019-05-18'
---

## ECMAScript和JavaScript的关系
---
ECMAScript是JavaScript的规范，JavaScript是ECMAScript的实现；
2009年，ECMAScript5发布，开始成为主流标准；
2015年，ECMAScript6发布，此后发布的标准通常成为ES6；
目前主流浏览器对ES6的支持程度可参考 
https://kangax.github.io/compat-table/es6/  
如果要在低端浏览器支持ES6新语法，需要使用babel进行转换，babel只转换新语法，新增加的API需要通过babel-polyfill进行方法填补，babel-polyfill对新API的实现主要依赖core-js库 
https://github.com/zloirock/core-js

## ES6常用新语法和新特性
---
### 块级作用域
ES6之前不存在块级作用域，内部变量可能覆盖外部变量，同时使用var定义的变量会具备变量提升效果，例如：
```
function test() {
  console.log(num); // 这里的num在下面定义了，变量提升了，不报错
  var num;
}
```
ES6增加了**let**，其声明的变量受到块级作用域限定，不具备变量提升的效果，例如：
```
function test() {
  console.log(num); // 报错
  let num;
}
```
ES6同时增加了**const**用来声明只读的常量，声明后不能被改变；

### 变量的解构赋值
```
let [a, b] = [1, 2]; // 数组解构赋值
let [a, b = 2] = [1]; // 数组解构赋值并带有默认值
let { name, age } = { name: 'xiaoming', age: 20 }; // 对象解构赋值
let { name, age } = { age: 20 }; // 对象解构赋值，name解构不到等于undefined
let { name = 'xiaoming', age } = { age: 20 }; // 对象解构赋值并带有默认值
let [a, b, c, d, e] = 'hello'; // 字符串解构赋值

/* 函数参数的解构赋值 */
function test1([x, y]){
  return x + y;
}
test1([1, 2]);

function test2({x, y}){
  return x + y;
}
test2({x: 1, y: 2});
```

### for-of遍历
遍历数组可以使用for-in语法，但是for-in返回的是数组的key，ES6新增的for-of可以直接返回数据的value，同时可以结合break、continue、return配合使用
```
let myArray = ['aaa', 'bbb'];
// 输出0,1
for (let key in myArray) {
  console.log(key);
}
// 输出aaa,bbb
for (let value of myArray) {
  console.log(value);
}
// 跳出循环，输出aaa
for (let value of myArray) {
  if (value === 'bbb') {
    break;
  }
  console.log(value);
}
```

### 模板字符串
ES6使用反引号包裹的字符串来作为模板字符串，其中可以使用${}插入变量
```
let num = 10;
let name = 'xiaoming';
console.log(`name:${name}, num:${num}`);
```

### 字符串新增的常用方法
- startsWith() —— 返回布尔值，表示参数字符串是否在原字符串的头部
- endsWith() —— 返回布尔值，表示参数字符串是否在原字符串的尾部
- includes() —— 返回布尔值，表示是否找到了参数字符串
- repeat(n) —— 返回一个新字符串，表示将原字符串重复n次
- padStart() —— 字符串头部补全
- padEnd() —— 字符串尾部补全
- trimStart() —— 去掉头部空格
- trimEnd() —— 去掉尾部空格
```
let s = 'Hello world!';
s.startsWith('Hello'); // true
s.endsWith('!'); // true
s.includes('o'); // true
```
```
'x'.repeat(3); // "xxx"
'x'.padStart(4, '0'); // '000x'
'x'.padEnd(4, '0'); // 'x000'
```
```
let s = '  abc  ';
s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

### Number.EPSILON
ES6增加一个Number.EPSILON，代表一个极小的常量（js的最小精度），众所周知浮点数的计算是不准确的（由于计算机二进制的存储导致精度丢失），我们无法对浮点数进行等号对比
```
let a = 0.1 + 0.2;
let b = 0.3;
if (a === b) {
  console.log('a === b'); // 进不来
}
```
通过新增的这个最小精度常量，我们可以去比较两个浮点数，如果差值绝对值小于这个最小精度，可以当做结果一致
```
let a = 0.1 + 0.2;
let b = 0.3;
if (Math.abs(a - b) < Number.EPSILON) {
  console.log('a === b'); // 可以进来
}
```

### 函数扩展
```
// 函数参数默认值
function test(x, y = 0) {
  console.log(x, y);
}

// rest参数（剩余参数），必须处于最后一个
function test(...items) {
  items.forEach(item => {
    console.log(item);
  });
}
test(1); // 1
test(1, 2, 3); // 1 2 3

// 箭头函数，注意箭头函数内部的this绑定了定义时的作用域，而非运行时的作用域
var f = () => 5; // 等同于 var f = function () { return 5 };
```

### 数组扩展
```
// 扩展运算符...，类似rest参数逆运算
console.log(...[1, 2, 3]) // 1 2 3

// Array.from —— 将类数组对象转为数组
function foo() {
  var args = [].slice.call(arguments); // ES5写法
  var args = Array.from(arguments); // ES6写法
}

// Array.of —— 将一组值，转换为数组
Array.of(3, 11, 8) // [3,11,8]
```

### 对象扩展
```
// Object.assign —— 对象合并
Object.assign({}, {a: 1}, {b: 1}); // {a: 1, b: 1}

let a = {num: 1};
let b = Object.assign({}, a); // 拷贝对象，使用的是浅拷贝，如果a对象属性是对象，则拷贝到a对象属性的引用

// Object.keys() —— 返回对象key数组
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj) // ["foo", "baz"]

// Object.values() —— 返回对象value数组
var obj = { foo: 'bar', baz: 42 };
Object.values(obj) // ["bar", 42]

// Object.entries() —— 返回对象key-value对数组
var obj = { foo: 'bar', baz: 42 };
Object.entries(obj) // [["foo", "bar"], ["baz", 42]]
```

### Symbol
ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）;
```
let mySymbol = Symbol();
```

### Set结构
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值，添加到Set里的数值会自动去重；
```
new Set([1, 2, 2, 3, 4]); // Set(4) {1, 2, 3, 4}
```
数组去重可以使用以下方法：
```
[...new Set([1, 2, 2, 3, 4])]; // [1, 2, 3, 4]
```
Set 内部判断两个值是否不同，它类似于精确相等运算符（===），不过会认为NaN等于自身，同时两个对象总是不相等的。
```
new Set([1, "1"]); // Set(2) {1, "1"}
new Set([NaN, NaN]); // Set(1) {NaN}
new Set([{name: "1"}, {name: "1"}]);
```
Set实例的常用属性和方法：
- Set.prototype.size：返回Set实例的成员总数。
- add(value)：添加某个值，返回 Set 结构本身。
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- has(value)：返回一个布尔值，表示该值是否为Set的成员。
- clear()：清除所有成员，没有返回值。

### Map结构
JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是只能用字符串当作键；
ES6 提供了 Map 数据结构，它类似于对象，也是键值对的集合，但是“键”可以是各种类型的值（包括对象）；
```
const m = new Map();
const o = {p: 'Hello World'};
m.set(o, 'content')
m.get(o) // "content"
```
Map通过set方法设置属性，通过get方法获取属性，直接取属性无法获取
```
const m = new Map();
m.set('name', 'content')
m.get('name') // "content"
m.name // undefined
```

### Proxy
Proxy用于拦截某个对象，对外界的访问进行某些过滤和改写；

let p = new Proxy(target, handler);
- target：用 Proxy 包装的目标对象（可以是数组对象，函数，或者另一个代理）；
- handler：一个对象，拦截过滤代理操作的函数。
```
var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function (target, key) {
    if (key in target) {
      return target[key]
    } else {
      return "对象没有此属性";
    }
  }
});

proxy.name; // "张三"
proxy.a; // "对象没有此属性"
```

### Reflect
Reflect为操作对象而提供的新API;
- 将Object对象的属于语言内部的方法放到Reflect对象上，即从Reflect对象上拿Object对象内部方法。
```
// 老写法
Object.defineProperty(target, property, attributes);
// 新写法
Reflect.defineProperty(target, property, attributes);
```
- 将用 老Object方法 报错的情况，改为返回false
```
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}
// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```
- 让Object操作变成函数行为
```
// 老写法
'name' in Object //true
// 新写法
Reflect.has(Object,'name') //true
```
- Reflect与Proxy是相辅相成的，在Proxy上有的方法，在Reflect就一定有
```
let proxy = new Proxy(target, {
  set(target, proName, proValue, receiver){
    // 确保对象的原生行为能够正常进行
    if (Reflect.set(target, proName, proValue, receiver)) {
      // 进行其他拦截操作
    }
  }
});
```

### Promise
Promise函数定义：
```
const promise = new Promise((resolve, reject) => {
  ...
});
```
常用实例方法：
- Promise.prototype.then()
- Promise.prototype.catch()
- Promise.prototype.finally()
```
promise.then(data => {
 // 成功回调
}, error => {
 // 失败回调
}).catch(error => {
  // 失败捕获
}).finally(() => {
  // 最终必然执行逻辑
});
```
- Promise.all()
```
// Promise.all方法用于将多个 Promise 实例包装成一个新的 Promise 实例，所有实例完成了才返回回调
const promise = Promise.all([p1, p2, p3]);
promise.then(...).catch(...); // 当p1，p2，p3都完成了才会进入resolve
```
- Promise.race()
```
// Promise.race方法也用于将多个 Promise 实例包装成一个新的 Promise 实例，其中某一个完成了立刻返回回调
const promise = Promise.race([p1, p2, p3]);
promise.then(...).catch(...); // 当p1，p2，p3某一个完成了，立刻会进入resolve
```

### async，await
async函数是对 Generator 函数的改进，是Generator 函数的语法糖；
async函数是将 Generator 函数的星号（*）替换成async，将yield替换成await，实现将异步操作串行化执行；
```
// Generator函数
function* () {
  yield a();
  yield b();
};
// async函数
async function() {
  await a();
  await b();
}
```
基础使用方法：
```
async function xxx() {
  // 多个异步操作串行化执行
  await a(); 
  await b(); 
  await c(); 
}
```
使用注意点：
- 注意错误捕获，await命令放在try...catch代码块中
```
async function xxx() {
  try {
    await a(); 
    await b(); 
    await c(); 
  } catch (error => {
    ...
  })
}
```
- 多个await命令不存在继发关系时进行并发处理
```
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```
- 遍历数据使用async方法
```
// forEach内部无法使用await，以下执行报错
async function() {
  array.forEach(() => {
    await xxx();
  });
}
// 正确方法是使用for循环
async function() {
  for (let d of array) {
    await xxx();
  }
}
// 或者使用Promise.all方法
async function() {
  let promises = array.map(() => xxx());
  await Promise.all(promises);
}
```

### Class
```
class A {
  // 构造器
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  方法() {
    ...
  }
}

const a = new A(1, 1); // 实例化
```
- 在“类”的内部可以使用get和set关键字，拦截某个属性的存取行为
```
class A {
  constructor() {
    ...
  }

  get name() {
    ...
  }

  set name(value) {
    ...
  }
}
```
- 类的静态方法
```
class A {
  static log() {
    ...
  }
}

A.log();
```
- 类的继承
```
class A {
  constructor() {
    ...
  }

  log() {

  }
}

// B继承A，也具备了log方法
class B extends A {
  constructor() {
    super(); // 调用父类的constructor方法
    ...
  }
}
```