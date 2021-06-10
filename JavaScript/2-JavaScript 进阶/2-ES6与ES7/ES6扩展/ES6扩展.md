# ES6扩展

## 1.Number的方法扩展

1.Number.isFinite() : 判断是否是有限大的数

2.Number.isNaN() : 判断是否是NaN

3.Number.isInteger() : 判断是否是整数

4.Number.parseInt() : 将字符串转换为对应的数值

**注:** window.parseInt 与 Number.parseInt 功能一样

**实例:**

```js
console.log(Number.isFinite(123)); // true
console.log(Number.isFinite(NaN)); // false
console.log(Number.isFinite(Infinity)); // false
console.log(Number.isNaN(NaN)); //true
console.log(Number.isInteger(22)); //true
console.log(Number.parseInt('123abc')); //123

// 笔试题
let str1 = '23.23';
let str2 = '34.34';
console.log(parseInt(str1) + parseInt(str2)); // 57
console.log(parseInt(str1 + str2)); // 23
```

## 2.对象方法扩展

1.Object.is(v1, v2)

  * 判断2个数据是否完全相等(一模一样)

2.Object.assign(target, source1, source2..) (重点！)

  * 将一个或多个源对象的属性复制到目标对象上，目标对象相同属性名的值会被覆盖。此方法会修改目标对象。

3.直接操作 __ proto __ 属性（ES6）

```js
let obj2 = {};
obj2.__proto__ = obj1;
```

**实例：**

```js
//特殊值比较
console.log(0 === -0); // true
console.log(NaN === NaN); // false

//Object.is(v1, v2)  看2个值是否完全相等(一模一样)
console.log(Object.is(0, -0)); // false
console.log(Object.is(NaN, NaN)); // true

//Object.assign(target, source1, source2..)
let target = {}; // target
let sources1  = {name: 'kobe'};
let sources2 = {age: 43};
// 克隆/拷贝
let result = Object.assign(target, sources1, sources2);
console.log(result);
console.log(target);

//直接操作 __proto__ 属性
let obj3 = {};
obj3.__proto__ = target;
console.log(obj3);
```

## 3.Set 和 Map

**1.Set**

  1) ES6提供了新的数据结构 Set（集合）,无序不可重复的多个值的集合体。

  2) Set的属性和方法：

​	1- Set() / Set(array)    构造函数

​	2- size    返回集合的元素个数

​	3- add()    增加一个新元素，返回当前集合

​	4- delete()    删除元素，返回 boolean 值

​	5- has()    检测集合中是否包含某个元素，返回 boolean 值

​	6- clear()    清空集合，返回 undefined

  3) Set实现了iterator 接口，所以可以使用 扩展运算符... 和 for...of 进行遍历

**实例：**

```js
//声明 set
let s = new Set();
console.log(s, typeof s);
//传入数组声明 set
let s2 = new Set(['大事儿','小事儿','好事儿','坏事儿','小事儿']);
//元素个数
console.log(s2.size);
//添加新的元素
s2.add('喜事儿');
//删除元素
s2.delete('坏事儿');
//检测
console.log(s2.has('糟心事'));
//清空
s2.clear();
console.log(s2);

// ES6中for...of 来替代 for...in 和 forEach()
for(let v of s2){
    console.log(v);
}
```

**2.Map**

  1) ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合。

​	但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

  2) Map也实现了iterator 接口，所以可以使用 扩展运算符... 和 for...of 进行遍历

  3) Map的属性和方法:

​	  1) Map() / Map(二维数组)    构造函数

​      2) size    返回 Map 的元素个数

​      3) set(key, value)    增加一个新元素，返回当前 Map

​      4) get()    返回键名对象的键值

​      5) has()    检测 Map 中是否包含某个元素，返回 boolean 值

​      6) clear()    清空集合，返回 undefined

**实例：**

```js
// 声明 Map
let m = new Map();
// 传入二维数组声明 Map
let map = new Map([[1, 'abc'], ['age', 123]]);
console.log(map);
// 添加元素
m.set('name','尚硅谷');
m.set('change', function(){
    console.log("我们可以改变你!!");
});
let key = {
    school : 'ATGUIGU'
};
m.set(key, ['北京','上海','深圳']);
// size
console.log(m.size);
// 删除
m.delete('name');
// 获取
console.log(m.get('change'));
console.log(m.get(key));
// 清空
m.clear();

// 遍历
for(let v of m){
    console.log(v); // v是一个数组，第一个元素是key，第二个是value
}
```

## 4.深拷贝与浅拷贝（重难点！）

1、数据类型：

* 数据分为基本的数据类型(String, Number, boolean, Null, Undefined)和对象数据类型

    - 基本数据类型

      特点: 存储的是该对象的实际数据

    - 对象数据类型

      特点: 存储的是该对象在堆中的引用，真实的数据存放在堆内存里

2、复制数据

- 基本数据类型存放的就是实际的数据，可直接复制

```js
let number2 = 2;
let number1 = number2;
```

- 克隆(拷贝)数据: 对象/数组

​	1- 区别:  浅拷贝/深度拷贝

​		判断:  拷贝是否产生了新的数据，还是拷贝的是数据的引用

​		知识点: 对象数据存放的是对象在堆内存的引用，直接复制的是对象的引用

```js
let obj = {username: 'kobe'}
let obj1 = obj;  // obj1 复制了obj在堆内存的引用
```

​		**注:** 判断深拷贝还是浅拷贝: 修改拷贝之后的数据(引用数据)会不会影响原数据！

​	2- 常用的拷贝技术

​		1). arr.concat():  不传参, 数组浅拷贝

​		2). arr.slice():  传参0, 数组浅拷贝

​		3). JSON.parse(JSON.stringify(arr/obj)):  数组或对象深拷贝, 但不能处理函数数据

​		4). Object.assign(target, source1, source2..):  浅拷贝

​		5). 扩展运算符是浅拷贝

​	**注：**

​		1. 浅拷贝包含函数数据的对象/数组

​		2. 深拷贝包含函数数据的对象/数组

**实例：**

1.实现检测数据类型的方法

2.实现深拷贝: 对象，数组 - 递归**（重点！）**

```js
let obj = {username: 'kobe'};
console.log(obj.toString()); // [object Object]
let arr = [1,2,3];
// 数组中重写了Object原型的toString方法
console.log(arr.toString()); // 1,2,3

console.log(Object.prototype.toString.call(arr)); // [object Array]
// 可用于检测数据类型
console.log(Object.prototype.toString.call(null).slice(8, -1)); //截取类型
// 实现检测数据类型的方法（重点！）
function checkoutType(target) {
    return Object.prototype.toString.call(target).slice(8, -1);
}

// for...in
let obj = {username: 'kobe',  age: 43};
for(let item in obj){
    console.log(item);
}
// for in 遍历数组可以获取数组的下标
for(let item in arr){
    console.log(item);
}

// 深度拷贝引入
// obj保存的是对象的内存地址
let obj = {username: 'kobe',  age: 43};
let obj2 = obj;  // obj保存的内存地址给了obj2， 引用传递
obj2.username = 'wade'
console.log(obj.username);

// 基本数据类型保存的是值本身
let num = 123;
let num2 = num;  // 值传递，修改新的数据不会影响原数据
num2 = 234;
console.log(num);


//拷贝: 深拷贝/浅拷贝, 判断深拷贝还是浅拷贝: 修改拷贝之后的数据(引用数据)会不会影响原数据
let obj = {username: 'kobe',  age: 43};
let obj2 = {};
obj2.username = obj.username;
obj2.age = obj.age;
obj2.username = 'wade';
console.log(obj.username);

// arr.concat(): 数组浅拷贝
let arr = [1, 2, 3, {username: 'kobe'}];
let testArr = [4,5];
let arr2 = [];
// arr2 = arr.concat(arr2);
console.log(arr2);

arr2 = arr.concat();
console.log(arr2);
arr2[3].username = 'wade';
console.log(arr, arr2);

// arr.slice(): 数组浅拷贝
let arr3 = arr.slice(0);
arr3[3].username = 'duncan';
console.log(arr3, arr);


// JSON.parse(JSON.stringify(arr/obj)) 将json数据 和 原生的js对象/数组相互转换
//json数据： json对象，json数组
let obj = {username: 'kobe',  age: 43, sex: {option1: '男', option2: '女'}};  // 不能处理函数的拷贝
let obj2 = JSON.parse(JSON.stringify(obj));
console.log(obj2);
obj2.username = 'wade';
obj2.sex.option1  = '混合';
console.log(obj2, obj);


// Object.assign(target, source1, source2..)  浅拷贝
let obj = {username: 'kobe',  age: 43, sex: {option1: '男', option2: '女'}};
let obj2 = {};
Object.assign(obj2, obj);
obj2.username = 'wade';
obj2.sex.option1  = '混合';
console.log(obj2, obj);


// 扩展运算符是浅拷贝
//构造字面量对象时可以使用扩展运算符...
let person = {name:'tom', age:18, ss:{s:11}}
let person2 = {...person}
//console.log(...person); //报错，展开运算符不能展开对象
person2.name = 'jerry';
person2.ss.s = 22;
console.log(person2);
console.log(person);


//实现深拷贝: 对象，数组 (重点!!!)-使用了递归
function checkoutType(target) {
    return Object.prototype.toString.call(target).slice(8, -1);
}
function clone(target) {
    let result; // 最终加工拷贝完的数据
    //  检测数据类型 - 判断拷贝的数据是对象 || 数组 || 其他(基本数据类型，函数)
    let targetType = checkoutType(target);
    if(targetType === 'Array'){
        result = [];
    }else if(targetType === 'Object'){
        result = {};
    }else {
        return target;
    }
    // 拷贝
    // arr = [1,2,3] ====> []arr2
    // obj = {username: 'kobe'} ===> {}obj2
    for(let item in target){
        // item: 对象(key), 数组(index)
        // target[item] 都可以获取对应的value
        let value = target[item];
        // arr2[item] = arr[item]
        // 判断是否是引用数据类型
        if(checkoutType(value) === 'Object' || 'Array'){
            result[item] = clone(value);
        }else {
            result[item] = value;
        }
    }
    return result;
}

let obj = {username: 'kobe', age: 43, sex: ['男', '女']};
let obj2 = clone(obj);
obj.username = 'wade';
console.log(obj, obj2);
obj2.sex[0] = '混合';
console.log(obj, obj2);
```

## 5.ES7 - 指数运算符(幂)

1.指数运算符(幂): **

- 等同于Math.pow()

**实例：**

```js
console.log(3 ** 3); // 27
```

## 6.面试题

```js
//1
var obj = {n: 1};
var obj2 = obj;
obj2.n = 2;
console.log(obj.n); //2

//2
function fn1(a) {
	a.n = 3;
}
fn1(obj);
console.log(obj.n); //3

//3
function fn2(a) {
    a = {n: 4}
}
fn2(obj);
console.log(obj.n); //3


//4
var a = {n: 1};
var b = a;
a.x = a = {n: 2};
console.log(a.n, b.n); //2  1
console.log(a.x, b.x); //undefined  {n: 2}

注: 操作对象属性的优先级高于给对象本身赋值！
    a.x = a = {n: 2};
    //上面的分解动作
    a.x = {n: 2};
    a = {n: 2};


//5
var x = 10;
function fn() {
    console.log(x);
}
function show(f) {
    var x = 20;
    f();
}
show(fn); //10


//6
var fn = function () {
    console.log(fn);
}
fn(); //function


//7
var obj = {
    fn2: function () {
        console.log(this.fn2); //function
        console.log(fn2); //报错
}
obj.fn2();


//8
var a = 2;
function fn() {
    console.log(a);
    var a = 3;
}
fn(); //undefined

function fn2() {
    console.log(a)
    a = 3;
fn2() //2

    
//9
function b() {}
var b;
console.log(typeof b); //function

    
//10
var c = 1;
function c(c) {
    console.log(c);
    var c = 3;
}
console.log(c); //1
c(2); //报错

    
//11
var name = 'wor1d!'
;(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log(name); //Jack
    } else {
        console.log(name)
    }
})()

    
//12
var a = 6;
setTimeout(function () {
    console.log(0);
    alert(a);
    a = 666;
}, 0);
console.log(1);
a = 66;
// 1  0  66

    
//13
function fn1() {
	var a = 2;
    function fn2 () {
        a++;
        console.log(a);
    }
    return fn2;
}
var f = fn1();
f(); //3
f(); //4
```

