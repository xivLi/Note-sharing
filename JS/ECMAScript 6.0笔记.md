# ECMAScript 6.0笔记

[toc]


### 简介

&emsp;&emsp;`ECMAScript 6.0`，简称`ES6`，是`JavaScript`的下一个版本标准，2015.06发布。此版本增加了非常重要的东西：`let`、`const`、`class`、`modules`、 `arrow functions`、`template` `string`、`destructuring`、`default`、`rest`、`argument`、`binary data`、`promises`等等。

    注：
    `ECMA`：European computer manufactures association，欧洲计算机制造联合会
    `ECMAScript`：由`ECMA`发布的规定浏览器脚本语言的标准

### 基本语法

#### 1. let和const
> let声明的变量只在let命令所在的代码块内有效
> const声明一个只读变量，即一旦声明，值就不能改变
    
let使用举例：
+ 块级作用域
```js
let x = 10;
var y = 10;
// 这里输出 x 为 10，y为10
{ 
    let x = 2;
    var y = 2;
    // 这里输出 x 为 2，y为2
}
// 这里输出 x 为 10，y为2
```
    ：let 声明的变量只在 let 命令所在的代码块 {} 内有效，在 {} 之外不能访问。
+ 循环作用域
```js
let i = 5;
for (let i = 0; i < 10; i++) {
    // 一些代码...
}
// 这里输出 i 为 5

var j = 5;
for (var j = 0; j < 10; j++) {
    // 一些代码...
}
// 这里输出 j 为 10
```
    ：使用 let 关键字声明的变量作用域只在循环体内，循环体外的变量不受影响。
+ 循环作用域2
```js
for (var i = 0; i < 10; i++) {
    setTimeout(function(){
        console.log(i);
    })
}
// 输出十个 10
for (let j = 0; j < 10; j++) {
    setTimeout(function(){
        console.log(j);
    })
}
// 输出 0123456789
```
    ：变量 i 是用 var 声明的，在全局范围内有效，所以全局中只有一个变量 i, 每次循环时，setTimeout 定时器里面的 i 指的是全局变量 i ，而循环里的十个 setTimeout 是在循环结束后才执行，所以此时的 i 都是 10。
    变量 j 是用 let 声明的，当前的 j 只在本轮循环中有效，每次循环的 j 其实都是一个新的变量，所以 setTimeout 定时器里面的 j 其实是不同的变量，即最后输出 012345...。
+ 全局变量
```js
var carName1 = "Volvo";
// 可以使用 window.carName1 访问变量
let carName2 = "Volvo";
// 不能使用 window.carName2 访问变量
```
const使用举例：
+ 不可修改，声明时初始化
```js
const PI = 3.141592653589793;
PI = 3.14;      // 报错
PI = PI + 10;   // 报错
```
+ 常量对象
```js
// 创建常量对象
const car = {type:"Fiat", model:"500", color:"white"};

// 修改属性:
car.color = "red";

// 添加属性
car.owner = "Johnson";
```
    ：常量对象的属性可变
+ 常量数组
```js
// 创建常量数组
const cars = ["Saab", "Volvo", "BMW"];
 
// 修改元素
cars[0] = "Toyota";
 
// 添加元素
cars.push("Audi");
```
    ：常量数组元素可变
    <br />
#### 2. 解构赋值
> 是一种针对数组或者对象进行模式匹配，然后对其中的变量进行赋值
> 解构赋值表达式的右边部分是解构的`源`
> 解构赋值表达式的左边部分是解构的`目标`

数组举例：
```js
let [a, b, c] = [1, 2, 3];
// a = 1
// b = 2
// c = 3
```
```js
let [a, [[b], c]] = [1, [[2], 3]];
// a = 1
// b = 2
// c = 3
```
```js
let [a, , b] = [1, 2, 3];
// a = 1
// b = 3
```
```js
let [a, ...b] = [1, 2, 3];
//a = 1
//b = [2, 3]
```
```js
let [a, b, c, d, e] = 'hello';
// a = 'h'
// b = 'e'
// c = 'l'
// d = 'l'
// e = 'o'
```
&emsp;&emsp;：解构的目标若为可遍历对象，皆可进行解构赋值。可遍历对象即实现 Iterator 接口的数据。

对象举例：
```js
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
// foo = 'aaa'
// bar = 'bbb'

let { baz : foo } = { baz : 'ddd' };
// foo = 'ddd'
```
```js
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
// a = 10
// b = 20
// rest = {c: 30, d: 40}
```
#### 3. Map对象
> Map对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值

+ Map对象基本使用：
```js
let myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
myMap.set("stringKey", "a string");
myMap.size;  //3
```
+ Map对象内容遍历for...of：
```js
for (let [key, value] of myMap) {
  console.log(key + " = " + value);
}
for (let [key, value] of myMap.entries()) {
  console.log(key + " = " + value);
}
for (let value of myMap.keys()) {
  console.log(value);
}
for (let value of myMap.values()) {
  console.log(value);
}
//entries 方法、keys 方法和 values 方法均返回一个新的 Iterator 对象
```
+ Map对象内容遍历forEach()：
```js
myMap.forEach(function(value, key, map) {
  console.log(key + " = " + value);
  console.log(key + " = " + map.get(key));
  console.log(key + " = " + this.get(key));
}, myMap);
```
&emsp;&emsp;或简单写成：
```js
myMap.forEach(function(value) {
  console.log(value);
});
```
+ Map对象与Array对象的转换
```js
let kvArray = [["key1", "value1"], ["key2", "value2"]];
 
// Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
let myMap = new Map(kvArray);
 
// 使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组
let outArray = Array.from(myMap);
```
+ Map对象的克隆
```js
let myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);
let myMap2 = new Map(myMap1);

console.log(myMap1 === myMap2);
// 打印 false。 Map 对象构造函数生成实例，迭代出新的对象。
```
+ Map对象的合并
```js
var first = new Map([[1, 'one'], [2, 'two'], [3, 'three'],]);
var second = new Map([[1, 'uno'], [2, 'dos']]);
 
// 合并两个 Map 对象时，如果有重复的键值，则后面的会覆盖前面的，对应值即 uno，dos
var merged = new Map([...first, ...second]);
```
#### 4. Set对象
> Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用
+ 特殊值：
    
&emsp;&emsp;+0 与 -0 在存储判断唯一性的时候是恒等的，所以不能重复；
&emsp;&emsp;undefined 与 undefined 是恒等的，所以不能重复；
&emsp;&emsp;NaN 与 NaN 是不恒等的，但是在 Set 中只能存一个，所以不能重复。
+ Set对象基本使用
```js
let mySet = new Set();

mySet.add(1); // Set(1) {1}
mySet.add(5); // Set(2) {1, 5}
mySet.add(5); // Set(2) {1, 5} 这里体现了值的唯一性
mySet.add("some text"); // Set(3) {1, 5, "some text"} 这里体现了类型的多样性

let o = {a: 1, b: 2}; 
let p = {a: 1, b: 2}; 
mySet.add(o); // Set(4) {1, 5, "some text", {…}}
mySet.add(p); // Set(5) {1, 5, "some text", {…}, {…}}
// 这里体现了对象之间引用不同不恒等，即使值相同，Set 也能存储
```
+ Set对象与Array对象的转换
```js
let setArray = ["value1", "value2", "value3"];
 
// Set 构造函数可以将一个数组转换成一个 Set 对象
let mySet = new Set(setArray);
 
// 使用 [...mySet] 可以将一个 Set 对象转换成一个数组
let outArray = [...mySet];
```
+ Set对象用于数组去重
```js
let myArray = [1, 2, 3, 4, 4];
let mySet = new Set(myArray);
let newArray = [...mySet];
```
+ Set对象合并
```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);
let union = new Set([...a, ...b]); // {1, 2, 3, 4}
// 相当于两个集合的并集
```
#### 5. 字符串新方法
+ 子串的识别
```js
let string = "apple,banana,orange";
//includes()方法，返回布尔值，判断是否找包含参数字符串
string.includes("banana");     // true
//startsWith()方法，返回布尔值，判断是否以参数字符串为开头
string.startsWith("apple");    // true
//endsWith()方法，返回布尔值，判断是否以参数字符串为结尾
string.endsWith("apple");      // false
//startsWith()方法，返回布尔值，判断是否以参数字符串为开头，6是搜索起始位置索引
string.startsWith("banana",6)  // true
```
+ 字符串重复
```js
//repeat(n)，返回原字符串重复n次的新字符串
console.log("Hello,".repeat(2));  // "Hello,Hello,"

//若参数是字符串，则会先将字符串转化为数字
console.log("Hello,".repeat("2"));  // "Hello,Hello,"
```
+ 字符串补全
```js
//padStart()方法和padEnd方法接受两个参数，第一个参数是指定生成的字符串的最小长度，第二个参数是用来补全的字符串。第二个参数默认值是空格，即" "
console.log("h".padStart(5,"o"));  // "ooooh"
console.log("h".padEnd(5,"o"));    // "hoooo"
console.log("h".padStart(5));      // "    h"
```
+ 模板字符串

&emsp;&emsp;相当于加强版的字符串
```js
let string = `Hello'\n' world`;
console.log(string); 
// "Hello'
// ' world"
//相当于
let string = `Hello'
' world`;
console.log(string); 
```
```js
//可以在字符串中加入变量和表达式
let name = "Mike";
let age = 27;
let info = `My Name is ${name}, I am ${age+1} years old next year.`
console.log(info);
// My Name is Mike, I am 28 years old next year.
```
#### 6. 数值新特性
+ 二进制与八进制
```js
// 二进制前缀0b或0B
console.log(0b11 === 3); // true
console.log(0B11 === 3); // true
// 八进制前缀0o或0O
console.log(0o11 === 9); // true
console.log(0O11 === 9); // true
```
+ 一个特殊常量
```js
console.log(Number.EPSILON); // 2.220446049250313e-16
//用于测试数值是否在误差范围内：
0.1 + 0.2 === 0.3; // false
// 在误差范围内即视为相等
equal = (Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON); // true
```
+ Number.isFinite()
```js
//判断一个数值是否为有限的
console.log( Number.isFinite(1));   // true ：1是有限数
console.log( Number.isFinite(NaN)); // false ：NaN不是有限的
console.log( Number.isFinite(Infinity));  // false ：Infinity不是有限的
```
+ Number.parseInt() 与 Number.parseFloat()
```js
//将给定字符串参数转化为相应整数，默认为10进制
Number.parseInt('12.34'); // 12
// 指定进制
Number.parseInt('0011',2); // 3
//包含特殊字符
Number.parseInt('122h32.34i'); // 122
//以特殊字符开头
Number.parseInt('w122h32.34i'); // NaN

// 与全局的 parseInt() 函数是同一个函数
Number.parseInt === parseInt; // true
```
```js
//用于把一个字符串解析成浮点数，无法被解析成浮点数，则返回 NaN
Number.parseFloat('123.45')    // 123.45
Number.parseFloat('123.45abc') // 123.45
Number.parseFloat('abc') // NaN

// 与全局的 parseFloat() 方法是同一个方法
Number.parseFloat === parseFloat // true
```
+ Number.isInteger()
```js
Number.isInteger(1);   // true
Number.isInteger(1.0); // true
 
Number.isInteger(1.1);     // false
Number.isInteger(Math.PI); // false
 
Number.isInteger(NaN);       // false
Number.isInteger(Infinity);  // false

Number.isInteger("10");  // false
Number.isInteger(true);  // false
Number.isInteger([1]);   // false
```
#### 7. 对象新特性
+ 属性的简洁表示
```js
const age = 12;
const name = "Amy";
const person = {age, name};
//等同于
const person = {age: age, name: name}
```
+ 方法名简写
```js
const person = {
  sayHi(){
    console.log("Hi");
  }
}
//等同于
const person = {
  sayHi:function(){
    console.log("Hi");
  }
}
```
&emsp;&emsp;如果是Generator 函数，则要在前面加一个星号:
```js
const obj = {
  * myGenerator() {
    yield 'hello world';
  }
};
//等同于
const obj = {
  myGenerator: function* () {
    yield 'hello world';
  }
};
```
+ 拓展运算符`...`
```js
//用于取出参数对象所有可遍历属性然后拷贝到当前对象：
let person = {name: "Amy", age: 15};
let someone = { ...person };
someone;  //{name: "Amy", age: 15}

//可以有多个参数对象：
let age = {age: 15};
let name = {name: "Amy"};
let person = {...age, ...name};
person;  //{age: 15, name: "Amy"}
```
+ 新方法assign()
```js
let target = {a: 1};
let object2 = {b: 2};
let object3 = {c: 3};
Object.assign(target,object2,object3);  
// 第一个参数是目标对象，后面的参数是源对象
target;  // {a: 1, b: 2, c: 3

//该拷贝为浅拷贝
target.b = 4;
object2.b; // 4
```
+ 新方法is()
```js
//用来比较两个值是否严格相等，与（===）基本类似
Object.is("q","q");      // true
Object.is(1,1);          // true
Object.is([1],[1]);      // false
Object.is({q:1},{q:1});  // false

//与（===）不同的是
Object.is(+0,-0);  //false
+0 === -0  //true
Object.is(NaN,NaN); //true
NaN === NaN  //false
```
#### 8. 数组新特性
+ Array.of()
```js
//将参数中的所有值作为元素构成数组返回
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
console.log(Array.of(1, '2', true)); // [1, '2', true]
console.log(Array.of()); // []
```
+ Array.from()

&emsp;&emsp;形式：Array.from(arrayLike[, mapFn[, thisArg]])
&emsp;&emsp;功能：将类数组对象或可迭代对象转化为数组
&emsp;&emsp;参数说明：
&emsp;&emsp;(1). arrayLike 类数组对象或可迭代对象
&emsp;&emsp;(2). mapFn 可选 map 函数，用于对每个元素进行处理，放入数组的是处理后的元素
&emsp;&emsp;(3). thisArg 可选 用于指定 map 函数执行时的 this 对象
```js
//将类数组对象或可迭代对象转化为数组
let map = {
    do (n) {
        return n * 2;
    }
}；
let arrayLike = [1, 2, 3];
console.log(Array.from(arrayLike, function (n){
    return this.do(n);
}, map)); // [2, 4, 6]
//等价于
let arrayLike = [1, 2, 3];
console.log(Array.from(arrayLike, function (n){
    return n * 2;
})); // [2, 4, 6]
```

&emsp;&emsp;类数组对象
```js
//一个类数组对象必须含有 length 属性，且元素属性名必须是数值或者可转换为数值的字符
let arr = Array.from({
  0: '1',
  1: '2',
  2: 3,
  length: 3 //会寻找元素属性名为0, 1, 2的元素放入新数组，若缺失则新数组相应元素为undefined
});
console.log(array) // ['1', '2', 3]

let arr = Array.from({
  3: '1',
  2: '2',
  1: 3,
  length: 3
});
console.log(array) // [undefined, 3, '2']
```
&emsp;&emsp;可迭代对象
```js
//map
let map = new Map();
map.set('key0', 'value0');
map.set('key1', 'value1');
console.log(Array.from(map)); // [['key0', 'value0'],['key1', 'value1']]

//set
let arr = [1, 2, 3];
let set = new Set(arr);
console.log(Array.from(set)); // [1, 2, 3]

//字符串
let str = 'abc';
console.log(Array.from(str)); // ["a", "b", "c"]
```
#### 9. 函数新特性
+ 函数参数默认值

&emsp;&emsp;ES6允许为函数的参数设置默认值，比如：
```js
function log(x, y = 'World') {
  console.log(x, y);
}
log('Hello') // Hello World
```
&emsp;&emsp;再如，若定义了默认值的参数不是尾参数：
```js
function f(x = 1, y) {
  return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined]
f(, 1) // 报错
f(undefined, 1) // [1, 1]
//有默认值的参数不是尾参数时，无法只省略该参数，而不省略它后面的参数，除非显式输入undefined。
```
+ 函数的length属性

&emsp;&emsp;指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数
```js
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```
&emsp;&emsp;需要注意，如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了：
```js
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```
&emsp;&emsp;length属性不计rest参数：
```js
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```
+ rest 参数

&emsp;&emsp;ES6 引入 rest 参数，用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。
```js
function add(...values) {
  let sum = 0;

  for (let val of values) { //values是一个数组
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```
+ 函数的name属性


&emsp;&emsp;函数的name属性，返回该函数的函数名。
```js
function foo() {}
foo.name // "foo"
```
&emsp;&emsp;如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名。
&emsp;&emsp;如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的name属性都返回这个具名函数原本的名字。
```js
var f = function () {};
const bar = function baz() {};

// ES5
f.name // ""
bar.name // "baz"
// ES6
f.name // "f"
bar.name // "baz"
```
&emsp;&emsp;Function构造函数返回的函数实例，name属性的值为anonymous。
```js
(new Function).name // "anonymous"
```
&emsp;&emsp;bind返回的函数，name属性值会加上bound前缀。
```js
function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```
+ 箭头函数

&emsp;&emsp;ES6 允许使用“箭头”（=>）定义函数。
```js
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};
```
&emsp;&emsp;如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
```js
var f = () => 5;
// 等同于
var f = function () { return 5 };
```
&emsp;&emsp;如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
```js
var sum = (num1, num2) => { return num1 + num2; }
```
&emsp;&emsp;如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
```js
// 报错
let getTempItem = id => { id: id, name: "Temp" };

// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });
```
&emsp;&emsp;回调函数简化箭头函数
```js
// 正常函数写法
[1,2,3].map(function (x) {
  return x * x;
});

// 箭头函数写法
[1,2,3].map(x => x * x);
```
```js
// 正常函数写法
let result = [1,3,2].sort(function (a, b) {
  return a - b;
});

// 箭头函数写法
let result = [1,3,2].sort((a, b) => a - b);
```
#### 10. Promise对象
#### 11. Generator函数
#### 12. async函数