# ES6常用

## 1.let关键字

1.作用: 与var类似, 用于声明一个变量

2.特点:

* 在块作用域内有效

* 不能重复声明

* 不存在变量提升, 即不能再初始化前使用变量。一定声明后再使用。

* 全局声明的变量不会挂载到window上，会放在script变量对象(暂存死区)上。

* 不影响作用域链

3.应用:

* 循环遍历添加监听

**实例:**

```js
//对比var
console.log(a); // undefined
var a = 123;
var a = 234;
console.log(a);

//不能重复声明
let star = '罗志祥';
let star = '小猪';

//不存在变量提升
// console.log(b);  //b is not defined
console.log(ausername);  //Cannot access 'ausername' before initialization
let ausername = 'wade';

//在块作用域内有效
//if else while for 
{
    let girl = '周扬青';
}
console.log(girl);


//应用-在块作用域内有效-循环遍历添加监听
//let解决来点击某个按钮, 提示"点击的是第n个按钮"时, 始终i=3的问题
let btns = document.getElementsByTagName('button');
for (let i = 0; i < btns.length; i++) {
    var btn = btns[i];
    btn.onclick = function () {
        alert(i);
    }
}
```

## 2.const关键字

1.作用: 定义一个常量

2.特点:

* 不能修改

* 声明时必须赋值

* 标识符一般为大写(潜规则)

* 在块作用域内有效

* 不能重复声明

* 不存在变量提升

3.应用: 一般声明对象类型使用 const，非对象类型声明选择 let

4.注意: 对象属性修改和数组元素变化不会触发 const 错误，因为对象名指向的地址没变！

**实例:**

```js
实例:
//声明常量
const SCHOOL = '尚硅谷';
//一定要赋初始值
const A; // 报错
//一般常量使用大写(潜规则)
const A = 100;
//常量的值不能修改
SCHOOL = 'ATGUIGU';
console.log(SCHOOL);
//对于数组和对象的元素修改, 不算做对常量的修改, 不会报错，对象名指向的地址没变！
const TEAM = ['UZI','MXLG','Ming','Letme'];
TEAM.push('Meiko');
```

## 3.变量的解构赋值（重点！）

1.理解:

* 从对象或数组中提取数据, 并赋值给变量(多个)

2.对象的解构赋值

let {n, a} = {n:'tom', a:12};

- 根据key索取value(按需索取)

3.数组的解构赋值

let [a,b] = [1, 'atguigu'];

- 根据index索取value

4.连续解构赋值+重命名

```js
let obj = {a:{b:1}};
const {a} = obj;  //传统解构赋值
const {a:{b}} = obj;  //连续解构赋值
const {a:{b:value}} = obj;  //连续解构赋值+重命名
```

5.应用

* 传入的参数为一个对象，利用函数的形参解构实参

**实例:**

```js
//对象解构赋值
let obj2 = {username:'kobe', age: 43};
let {username, age} = obj2;  // 解构赋值真正实现的是: 根据key索取value(按需索取)
console.log(username, age);

//数组解构赋值
let arr = [4,5,3];
let num1 = arr[2];
let num2 = arr[1];
console.log(num1, num2);

let [a,b] = arr;  // 根据index索取value
let [,,c] = arr;  // 用,占位取后面的值
console.log(a,b); //4  5
console.log(c);  //3


//应用-利用函数的形参解构实参
function fun({username, age}) {
    // let 形参变量 = 实参；
    // let {username, age} = {username:'kobe', age: 43};
    console.log(username);
    console.log(age);
}
fun(obj2);
```

## 4.模板字符串

1.理解: 简化字符串的拼接

* 模板字符串必须用 `` 包含

* 变量的部分使用${xxx}定义

**实例：**

```js
let obj = {username: 'kobe', age: 43};
let str = '我的名字是： ' + obj.username + '， 我的年龄是： ' + obj.age;
console.log(str);
let str1 = `我的名字是：${obj.username}, 我的年龄是：${obj.age}`;
console.log(str1);
```

## 5.对象的简化写法

1.省略同名的属性值

2.省略函数的function和冒号:

**实例:**

```js
//ES5
let obj = {
    username: username,
    age: age,
    showName: function () {
        console.log(this.username);
    }
};
console.log(obj);
obj.showName();

//ES6
let obj = {
    username,  // 同名的属性可省略 ---> key 和 value一样的时候
    age,
    showName() {  // 省略函数的function和冒号
        console.log(this.username);
    }
};
console.log(obj);
obj.showName();
```

## 6.箭头函数（重点！）

1.语法: let fun = 形参 => 函数体

2.形参:

* 没有参数: () => 函数体
	
* 一个参数-括号可省略: i => 函数体
	
* 大于一个参数: (i,j) => 函数体

3.重点: 函数体

- 只有一条语句或表达式的时候: {}可以省略
	
- 当有多条语句的时候: {}不能省略
	
- 当{}省略的时候会自动return当前语句或表达式的执行结果
	
- 只有一条语句且return对象时，省略括号{}会导致问题，因此需要在外面写小括号包裹对象：

  const fn = b => ({a:'22', c: b})

4.特点

- 箭头函数没有自己的this，不是调用的时候决定的，而是定义的时候决定
	
- 扩展理解:
	
	* 箭头函数定义的时候看外部是否有非箭头函数，如果有，当前箭头函数的this指向与外部函数的this是同一个对象
	
	* 如果外部没有，this指向的是window
	
- 箭头函数不能用作构造函数
	
- 不能使用 arguments

**实例:**

```js
//当箭头函数没有形参的时候, () 不能省略
let fun = () => console.log('fun()');
fun();

//当箭头函数只有一个形参的时候，() 可以省略，也可以不省略
let fun1 = a => console.log('只有一个形参的时候', a);
fun1(123);

//当箭头函数有多个形参的时候，()不能省略
let fun2 = (a, b) => console.log('有多个形参的时候', a, b);
fun2(1,2);

// 当箭头函数的函数体只有一条语句的时候{}可以省略， 当{}省略的时候会自动 return 当前语句或者表达式的结果, 当{}不省略的时候，需要手动指定返回的结果, 否则默认执行 return value: undefined;
let fun3 = () => console.log('函数体只有一条语句的时候'); // 语句的返回结果
console.log(fun3());
// let fun3 = () => 123 // 语句的返回结果
// console.log(fun3());

//当箭头函数的函数体有多条语句的时候，{}不能省略， 当{}不省略的时候，需要手动指定返回的结果， 否则默认执行 return value: undefined;
let fun4 = () => {
    let a = 1;
    console.log('函数体有多条语句的时候', a);
    // return a;
}
console.log(fun4());

//箭头函数的this指向问题 
btn1.onclick = function () {
    console.log(this); // btn1对象
};

btn2.onclick = () => console.log(this); // window

let obj = {
    username: 'kobe',
    test: function () { // this ---> obj
        btn2.onclick = () => console.log(this); // obj
    }
}

let obj = {
    username: 'kobe',
    test:  () => {
        btn2.onclick = () => console.log(this); // window
    }
};
obj.test();

//箭头函数不能用作构造函数
let Person = () => this.name = 'kobe';
let person1 = new Person();

//不能使用 arguments
let fn = () => {
    console.log(arguments);
}
fn(1,2,3);



//案例
//需求1: 点击 div 2s 后颜色变成『粉色』
//获取元素
let ad = document.getElementById('ad');
//绑定事件
ad.addEventListener("click", function(){
    // let _this = this;  //缓存this
    //定时器
    // setTimeout(function () {
    //     //修改背景颜色 this
    //     console.log(this);
    //     _this.style.background = 'pink';
    // }, 2000);
    
    setTimeout(() => {
        this.style.background = 'pink';
    }, 2000);
});


//需求2: 从数组中返回偶数的元素
const arr = [1,6,9,10,100,25];
// const result = arr.filter(function(item){
//     if(item % 2 === 0){
//         return true;
//     }else{
//         return false;
//     }
// });

const result = arr.filter(item => item % 2 === 0);
console.log(result);
```

**总结:**

- 箭头函数适合与 this 无关的回调。如：定时器, 数组的方法回调!

- 箭头函数不适合与 this 有关的回调。如：事件回调, 对象的方法!

## 7.三点运算符 - rest(可变)参数 / 扩展运算符(拆包)

1.语法: ...变量名

2.应用: 

​    1) rest(可变)参数

* 用于获取函数的实参

* 用来取代 arguments, 且比 arguments 更灵活

* 使用三点运算符收集实参的时候是一个真数组

* 必须要放到参数最后

​    2) 扩展运算符(拆包)

* 作用: 获取数组里的每一项

* 原理: 三点运算符会调用iterator接口，如果底层有这个接口，那就可以去遍历数组

* 应用: 将伪数组转换为真数组

**实例:**

```js
//rest(可变)参数
function fun(a,b, ...values) {
    // arguments用来收集实参的伪数组
    // 伪数组： 有length属性，可以通过下标获取对应的值，但是没有数组的一般方法(push, filter, map)
    console.log(arguments);
    console.log(a);
    console.log(values);
    values.forEach(function (item, index) {
        console.log(item, index);
    });
}
fun(2, 3, 4, 5);


//扩展运算符(拆包)
let arr2 = [2, 3, 4, 5];
let arr3 = [1, ...arr2, 6];
console.log(arr3);  // [1, 2, 3, 4, 5, 6]
// ...运算符去拆包指定的数组
console.log(...arr2);
// 数组克隆 - 深拷贝
console.log([...arr2]);

// ...运算符去拆包不能直接去遍历对象
let obj2 = {name: 'kobe', age: 43};
// console.log(...obj2);  //报错

// ...运算符合并对象/复制对象
let obj3 = {sex: '男', ...obj2};
//构造字面量对象时可以使用...运算符
let obj4 = {...obj2};
console.log(obj3);
console.log(obj4);

//合并的同时改值或加值
let person = {name:'tom', age:18}
let person3 = {...person, name:'jack', address:"地球"}
console.log(person3);

// 将伪数组转为真正的数组
const divs = document.querySelectorAll('div');
const divArr = [...divs];
console.log(divArr);
```

## 8.形参默认值

1.形参初始值

- 具有默认值的参数, 一般位置要靠后(潜规则)

2.应用

- 与解构赋值结合！

**实例:**

```js
function Point(x = 0, y = 0) {
    //原生实现
    // if(!x && x !== 0){
    //   x = 0;
    // }
    // if(!y && y !== 0){
    //   y = 0;
    // }
    
    this.x = x;
    this.y = y;
}
let point = new Point(22, 45);
console.log(point);

let point2 = new Point();
console.log(point2);


// 形参默认值的类型一般根据需要的实参类型决定
function fun(arr = []) {
    arr.forEach(function (item, index) {
        console.log(item, index);
    })
}
let arr = [1,2,3];
fun(arr);
fun();


// 应用-与解构赋值结合!!!
function connect({host="127.0.0.1", username, password, port}){
    console.log(host)
    console.log(username)
    console.log(password)
    console.log(port)
}
connect({
    host: 'atguigu.com',
    username: 'root',
    password: 'root',
    port: 3306
})
connect({
    username: 'root',
    password: 'root',
    port: 3306
})
```

## 9.Symbol

1.理解: ES6提供的一种新的原始(基本)数据类型symbol

回顾:

* 基本数据类型: String, Number, Boolean, null, undefined, Symbol

* typeof返回值: string, number, boolean, object, undefined, symbol, function

2.特点

- Symbol属性对应的值是唯一的，解决命名冲突问题

- Symbol值不能与其他数据进行计算，包括同字符串拼串

- for in, for of遍历时不会遍历symbol属性

3.内置Symbol值

- Symbol.iterator

	* 对象的Symbol.iterator属性，指向该对象的默认遍历器(迭代器)方法

**实例:**

```js
//使用symbol
let symbol = Symbol();
let symbol2 = Symbol();
console.log(symbol);
console.log(symbol2);
//验证值是唯一的
console.log(symbol === symbol2);
console.log(typeof symbol);

//Symbol函数传参
let symbol3 = Symbol('one');
let symbol4 = Symbol('two');
console.log(symbol3);
console.log(symbol4);
//验证值是唯一的
console.log(symbol3 === symbol4);

//使用symbol当做对象的属性名
let symbol5 = Symbol();
let obj = {};
obj[symbol5] = 'hello';
console.log(obj);

//内置Symbol值: Symbol.iterator
console.log(Symbol.iterator);
```

## 10.for...of 与 iterator接口底层实现（难点！）

1.for(let value of target){}循环遍历

  1）遍历数组

  2）遍历Set

  3）遍历Map

  4）遍历字符串

  5）遍历伪数组

2.iterator接口

  1) 概念

  - iterator 是一种接口机制，为各种不同的数据结构提供统一的访问机制。

  2) 作用

  - 为各种数据结构，提供一个统一的、简便的访问接口。

  - 使得数据结构的成员能够按某种次序排列。

  - ES6创造了一种新的遍历命令 for...of循环，iterator接口主要供for...of调用。

  3) 工作原理

  - 创建一个指针对象(遍历器对象/迭代器对象)，指向数据结构的起始位置。

  - 第一次调用next方法，指针自动指向数据结构的第一个成员

  - 接下来不断调用next方法，指针会一直往后移动，直到指向最后一个成员

  - 每调用next方法返回的是一个包含value和done的对象，{value: 当前成员的值,done: 布尔值}

    * value表示当前成员的值，done对应的布尔值表示当前的数据的结构是否遍历结束。

    * 当遍历结束的时候返回的value值是undefined，done值为true

  4) 原生具备iterator接口的数据结构(可用for...of遍历)

​	1）Array

​	2）arguments

​	3）set容器

​	4）map容器

​	5）String
​	...

**注：** ...运算符也是调用 iterator 接口来实现遍历！

**实例:**

```js
let arr = [1,2,3,4,5,1,2,3,4];
let set = new Set(arr);
//遍历set
for(let item of set){
    console.log(item);
}
console.log(...set);

//遍历字符串
let str = 'abcd'
for(let item of str){
    console.log(item);
}

//遍历伪数组
let btns = document.getElementsByTagName('button');
for(let item of btns){
    console.log(item);
}

//验证for...of与...运算符调用iterator接口
let arr = [2,3,4,5];
// Array.prototype[Symbol.iterator] = 1111;
console.log(arr);
for(let item of arr){
    console.log(item);
}
console.log(...arr);


// iterator接口(方法 / api)底层的简单实现 - 手动调用next()移动指针
function iteratorUtil(target) {
    let index = 0; //  标识指针的起始位置
    return { // 生成iterator遍历器对象
        next: function () {
            return index < target.length?{value: target[index++], done: false}:{value: target[index++], done: true}
        }
    }
}
let iteratorObj = iteratorUtil(arr);  // 生成iterator遍历器对象
console.log(iteratorObj.next());
console.log(iteratorObj.next());
console.log(iteratorObj.next());
console.log(iteratorObj.next());
console.log(iteratorObj.next());
console.log(iteratorObj.next());


// iterator接口(方法 / api)底层的完整实现 - 可遍历数组/字符串/对象
function iteratorUtil() {
    //console.log('我的方法被调用了', this);  // this是遍历的目标数组/字符串/对象
    // console.log(target);  // undefined, 因此target传参可去除
    let that = this;  // 缓存this
    let index = 0;  // 标识指针的起始位置
    let keys = Object.keys(that);  // 获取对象中所有key的数组
    if(this instanceof Array || this instanceof String){  // 遍历数组/字符串
        return {  // 生成iterator遍历器对象
            next:  function (){  // 可以使用箭头函数解决this的指向问题
                return index < that.length?{value: that[index++], done: false}:{value: that[index++], done: true};
            }
        }
    }else {  // 遍历对象
        return {  // 生成iterator遍历器对象
            next:  function (){  // 可以使用箭头函数解决this的指向问题
                return index < keys.length?{value: that[keys[index++]], done: false}:{value: that[keys[index++]], done: true};
            }
        }
    }
}
//使用自己iterator接口去实现for...of/...运算符的遍历
Array.prototype[Symbol.iterator] = iteratorUtil;
Object.prototype[Symbol.iterator] = iteratorUtil;

//遍历数组
let arr = [2,3,4,5];
// for of 调用 iterator接口
// 三点运算符 调用 iterator接口
for(let item of arr){
    console.log(item);
}
console.log(...arr);

//遍历对象
let obj = {
    username: 'kobe',
    age: 43
};
// console.log(Object.keys(obj));  // 获取对象中所有key的数组
for(let o of obj){
    console.log(o);
}

//遍历字符串
let str = "abcdef";
for(let s of str){
    console.log(s);
}
console.log(...str);
```

## 11.Class类（重点！）

1.通过 class 定义类

2.在类中通过 constructor 定义构造方法

3.通过 new 来创建类的实例

4.通过 extends 来实现类的继承 - 相当于ES5的原型继承

5.通过 super 调用父类的构造方法 - 相当于ES5的构造函数继承

6.使用 static (静态资源修饰符)可以给类对象自身添加属性

7.重写从父类中继承的一般方法 - 参数与方法名必须相同

**注:**

  1) extends做的事情: 1.子类的原型 成为 父类的实例 2.添加constructor属性，并指向子类本身

  2) super做的事情: 1.调用父类的构造方法 2.改变父类构造方法的this指向为子类的实例

  3) super必须写在子类constructor里的最前面（所有this.操作的前面）

  4) 类中的构造器不是必须要写的，要对实例进行一些初始化的操作，如添加指定属性时才写。

  5) 如果A类继承了B类，且A类中写了构造器，那么A类构造器中的super是必须要调用的。

  6) 类中所定义的方法，都放在了类的原型对象上，供实例去使用。

  7) 类中的方法默认开启了严格模式

**实例:**

```js
// 定义一个人类: Person
class Person {
    // 静态资源修饰符，使用static可以给 类对象 自身添加属性
    static num = 123;
    //类中可以直接写赋值语句,如下代码的含义是：给Car的实例对象添加一个属性，名为wheel，值为4，
    //等于在构造函数里赋值: this.wheel = 4;
    wheel = 4;

    // 类的构造方法
    constructor(name, age){
        //构造器中的this是谁？—— 类的实例对象
        console.log('--- constructor() ---');
        this.name = name;
        this.age = age;
        //this.wheel = 4;
    }
    // 类的一般方法
    //showInfo方法放在了哪里？ —— 类的原型对象上，供实例使用
    //通过Person实例调用showInfo时，showInfo中的this就是Person实例
    showInfo(){ // userInfo
        console.log(this.name, this.age);
    }
}

//Person类自身添加属性
Person.msg = 'Person自身的属性';
console.log(Person.msg);
console.log(Person.num);

//使用类: 同构造函数使用方式一样
let person1 = new Person('kobe', 43);
console.log(person1);
person1.showInfo();

// 定义一个 子类
class Child extends Person {  // extends做的事情: 1.子类的原型 成为 父类的实例 2.添加constructor属性，并指向子类本身
    constructor(name, age, sex) {
        // super做的事情: 1.调用父类的构造方法 2.改变父类构造方法的this指向为子类的实例
        super(name, age);  // super调用父类的构造方法
        this.sex = sex;
    }
    // 父类的方法重写：当父类原型的方法不能满足子类实例需求的时候
    //showInfo方法放在了哪里？ —— 类的原型对象上，供实例使用
    //通过Child实例调用showInfo时，showInfo中的this就是Child实例
    showInfo(){
        console.log(this.name, this.age, this.sex);
    }
}

let child1 = new Child('xiaoming', 18, '男');
console.log(child1);
child1.showInfo();
```

