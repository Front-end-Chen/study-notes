# React进阶1

# 一、React应用(基于React脚手架)

## 1.使用create-react-app创建react应用

### 1.1 react脚手架

1.xxx脚手架: 用来帮助程序员快速创建一个基于xxx库的模板项目

​    1.包含了所有需要的配置（语法检查、jsx编译、devServer…）

​    2.下载好了所有相关的依赖

​    3.可以直接运行一个简单效果

2.react提供了一个用于创建react项目的脚手架库: create-react-app

3.项目的整体技术架构为:  react + webpack + es6 + eslint

4.使用脚手架开发的项目的特点: 模块化, 组件化, 工程化

### 1.2 创建项目并启动

第一步，全局安装：npm i -g create-react-app（不推荐！），导致版本固定，如项目版本改变，需卸载重装

​	局部安装：npm i  create-react-app（推荐！），可安装多个版本适应不用项目

第二步，切换到想创项目的目录，使用命令：create-react-app hello-react（全局安装）

​	在局部安装create-react-app的目录，使用命令：npx create-react-app hello-react（局部安装）

第三步，进入项目文件夹：cd hello-react

第四步，启动项目：npm start / yarn start

- 注：

	- yarn与npm安装库的命令不要混用，只用一个即可

### 1.3 react脚手架项目结构

**public ---- 静态资源文件夹**

​    favicon.icon ------ 网站页签图标

​    **index.html -------- 主页面**

​    logo192.png ------- logo图

​    logo512.png ------- logo图

​    manifest.json ----- 应用加壳的配置文件

​    robots.txt -------- 爬虫协议文件

**src ---- 源码文件夹**

​    App.css -------- App组件的样式

​    **App.js --------- App组件**

​    App.test.js ---- 用于给App做测试

​    index.css ------ 样式

​    **index.js ------- 入口文件**

​    logo.svg ------- logo图

​    reportWebVitals.js

​    	--- 页面性能分析文件(需要web-vitals库的支持)

​    setupTests.js

​    	---- 组件单元测试的文件(需要jest-dom库的支持)

## 2.HelloWorld案例

├─ public
│  ├─ favicon.ico
│  └─ index.html
├─ src
│  ├─ App.jsx
│  ├─ components
│  │  ├─ Hello
│  │  │  ├─ index.jsx
│  │  │  └─ index.module.css
│  │  └─ Welcome
│  │     ├─ index.css
│  │     └─ index.jsx
│  └─ index.js

**总结**

1.常规组件文件/样式文件命名

├─ components
│  │  ├─ Hello
│  │  │  ├─ Hello.css
│  │  │  └─ Hello.jsx

2.组件名简写index命名，通过文件夹区分组件文件/样式文件（推荐！）

├─ components
│  │  ├─ Hello
│  │  │  ├─ index.jsx
│  │  │  └─ index.css

3.样式模块化（了解！）

1）文件命名xxx.module.css

├─ components
│  │  ├─ Hello
│  │  │  ├─ index.jsx
│  │  │  └─ index.module.css

2）引入

import hello from './index.module.css'

3）使用

```jsx
<h2 className={hello.title}>Hello,React!</h2>
```

- 使用css模块化避免组件之间产生样式冲突(样式同名)，使用less则不用

## 3.功能界面的组件化编码流程（通用）(牢记！）

1.拆分组件: 拆分界面，抽取组件

2.实现静态组件: 使用组件实现静态页面效果

3.实现动态组件

​    1）动态显示初始化数据

​    	1.数据类型

​    	2.数据名称

​    	3.保存在哪个组件?

​    2）交互(从绑定事件监听开始)

## 4.组件的组合使用 - TodoList案例

├─ public
│  ├─ css
│  │  └─ bootstrap.css
│  ├─ favicon.ico
│  └─ index.html
├─ src
│  ├─ App.css
│  ├─ App.jsx
│  ├─ components
│  │  ├─ Footer
│  │  │  ├─ index.css
│  │  │  └─ index.jsx
│  │  ├─ Header
│  │  │  ├─ index.css
│  │  │  └─ index.jsx
│  │  ├─ Item
│  │  │  ├─ index.css
│  │  │  └─ index.jsx
│  │  └─ List
│  │     ├─ index.css
│  │     └─ index.jsx
│  └─ index.js

**总结**

1.拆分组件、实现静态组件，注意：className、style的写法

2.动态显示初始化数据，如何确定将数据放在哪个组件的state中？

- 某个组件使用：放在其自身的state中

- 某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）

3.关于父子之间通信：

​    1)【父组件】给【子组件】传递数据：通过props传递

​    2)【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数

4.注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value

- defaultChecked只在第一次显示时有效

5.状态在哪里，操作状态的方法就在哪里

# 二、React ajax

## 1.理解

1.React本身只关注于界面, 并不包含发送ajax请求的代码

2.前端应用需要通过ajax请求与后台进行交互(json数据)

3.react应用中需要集成第三方ajax库(或自己封装)

## 2.常用的ajax请求库

1.jQuery: 比较重, 如果需要另外引入不建议使用

2.axios: 轻量级, 建议使用

​    1) 封装XmlHttpRequest对象的ajax

​    2) promise风格

​    3) 可以用在浏览器端和node服务器端

## 3.axios基本使用

1）GET请求

```js
//写法1
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });

//写法2
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

2）POST请求

```js
axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

## 4.react脚手架配置代理 - 解决跨域问题

**1.方法一**

在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1.优点：配置简单，前端请求资源时可以不加任何前缀。

2.缺点：不能配置多个代理。

3.工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）

**2.方法二**

1.第一步：创建代理配置文件

在src下创建配置文件：src/setupProxy.js

2.编写setupProxy.js配置具体代理规则：

```js
//引入http-proxy-middleware中间件-脚手架自带
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
    app.use(
        proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
            target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
            changeOrigin: true, //控制服务器接收到的请求头中host字段的值
            /*
         	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
         	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
         	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
            pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
        }),
        proxy('/api2', { 
            target: 'http://localhost:5001',
            changeOrigin: true,
            pathRewrite: {'^/api2': ''}
        })
    )
}
```

说明：

1.优点：可以配置多个代理，可以灵活的控制请求是否走代理。

2.缺点：配置繁琐，前端请求资源时必须加前缀。

## 5.消息订阅-发布机制

1.工具库: PubSubJS

2.下载: npm install pubsub-js

3.使用: 

​    1)  引入PubSub对象

​	import PubSub from 'pubsub-js' 

​    2)   订阅，第一个参数为消息名，第二个参数为回调函数

​	this.xxx = PubSub.subscribe('delete', function(msg,data){ });

​    3)  发布消息，第一个参数为消息名，第二个参数为数据

​	PubSub.publish('delete', data) 

​    4)  取消订阅

​	PubSub.unsubscribe(this.xxx) 

## 6.事件总线（EventBus）- 类似vue的事件总线

1.工具库: events

2.下载: npm install events

3.使用: 

​    1)  引入EventEmitter类

​	import { EventEmitter } from 'events' 

​	2)   创建EventEmitter实例对象：eventBus对象

​	const eventBus = new EventEmitter();

​    3)   监听事件，第一个参数为事件名，第二个参数为监听函数

​	eventBus.addListener("事件名称", function(data){ })；

​    4)  发出事件，第一个参数为事件名，第二个参数为数据

​	eventBus.emit("事件名称", 参数列表);

​    5)  移除事件

​	eventBus.removeListener("事件名称", 监听函数)；

## 7.Fetch

1.fetch: 原生函数，不再使用XmlHttpRequest对象提交ajax请求

2.老版本浏览器可能不支持

3.基本使用

1）GET请求

```js
fetch(url).then(function(response) {
    return response.json()
}).then(function(data) {
    console.log(data)
}).catch(function(e) {
    console.log(e)
});
```

2）POST请求

```js
fetch(url, {
    method: "POST",  // *GET, POST, PUT, DELETE, etc.
    body: JSON.stringify(data),  // 必须匹配请求头的Content-Type
    //mode: 'cors',  // no-cors, cors, *same-origin
    //headers: {
    //  'user-agent': 'Mozilla/4.0',
    //  'content-type': 'application/json'
    //},
}).then(function(data) {
    console.log(data)
}).catch(function(e) {
    console.log(e)
})
```

## 8.github用户搜索案例 - axios/fetch+PubSub/events

├─ public
│  ├─ css
│  │  └─ bootstrap.css
│  ├─ favicon.ico
│  └─ index.html
├─ src
│  ├─ App.jsx
│  ├─ components
│  │  ├─ List
│  │  │  ├─ index.css
│  │  │  └─ index.jsx
│  │  └─ Search
│  │     └─ index.jsx
│  ├─ index.js
│  └─ setupProxy.js

github-api请求地址: https://api.github.com/search/users?q=xxx

**总结**

1.引用外部css库放在public文件夹中，如bootstrap

2.设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。

3.ES6小知识点：解构赋值+重命名

```js
let obj = {a:{b:1}}
const {a} = obj;  //传统解构赋值
const {a:{b}} = obj;  //连续解构赋值
const {a:{b:value}} = obj;  //连续解构赋值+重命名
```

4.消息订阅与发布机制

​    1）先订阅，再发布（理解：有一种隔空对话的感觉）

```js
search = ()=>{
    PubSub.publish('atguigu',{isFirst:false,isLoading:true})
    ...
}

componentDidMount(){
    this.token = PubSub.subscribe('atguigu',(_,stateObj)=>{ //用_来占位,即不使用第一个参数
        this.setState(stateObj)
    })
}
```

​    2）适用于任意组件间通信

​    3）要在组件的componentWillUnmount中取消订阅（与关闭定时器类似）

```js
componentWillUnmount(){
    PubSub.unsubscribe(this.token)
}
```

5.事件总线（EventBus）- 与消息订阅与发布机制作用相同

​    1）先监听，再发出（理解：有一种隔空对话的感觉）

```js
search = ()=>{
    eventBus.emit('atguigu',{isFirst:false,isLoading:true})
    ...
}

componentDidMount(){
    this.token = eventBus.addListener('atguigu',(stateObj)=>{
        this.setState(stateObj)
    })
}
```

​    2）适用于任意组件间通信

​    3）要在组件的componentWillUnmount中移除事件（与关闭定时器类似）

```js
componentWillUnmount(){
    eventBus.removeListener(this.token)
}
```

6.fetch发送请求（关注分离的设计思想）

​	1) 先判断是否成功连接服务器

​	2) 再判断数据是否获取到

7.fetch发送网络请求

1）常规发送（未优化）

```js
search = ()=>{
    fetch(`/api1/search/users2?q=${keyWord}`).then(
        response => {
            console.log('联系服务器成功了');
            // console.log(response);
            //返回一个promise
            return response.json()
        },
        error => {
            console.log('联系服务器失败了',error);
            //连接服务器失败，中断promise链（返回pending状态的promise）
            return new Promise(()=>{})
        }
    ).then(
        response => {
            console.log('获取数据成功了',response);
            publish('getUsers', { isLoading: false, users: response.items })
        },
        error => {
            console.log('获取数据失败了',error);
            publish('getUsers', { isLoading: false, err: error.message })
        }
    )
}
```

2）async / await（优化）

```js
search = async()=>{
    try {
        const response= await fetch(`/api1/search/users2?q=${keyWord}`)
        const data = await response.json()
        console.log(data);
    } catch (error) {
        console.log('请求出错',error);
    }
}
```

# 三、React Router v4

## 1.相关理解

### 1.1 SPA的理解

1.单页Web应用（single page web application，**SPA**）。

2.整个应用**只有一个完整的页面**。

3.点击页面中的链接**不会刷新**页面，只会做页面的**局部更新**。

4.数据都需要通过ajax请求获取, 并在前端异步展现。

### 1.2 路由的理解

1.什么是路由?

​	1）一个路由就是一个映射关系(key: value)

​	2）key为路径, value可能是 function 或 component

2.路由分类

​	1.后端路由：

​		1)理解： value是function, 用来处理客户端提交的请求。

​		2)注册路由： router.get(path, function(req, res))

​		3)工作过程：当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据

​	2.前端路由：

​		1)浏览器端路由，value是component，用于展示页面内容。

​		2)注册路由: 

```jsx
<Route path="/test" component={Test}>
```

​		3)工作过程：当浏览器的path变为/test时, 当前路由组件就会变为Test组件

### 1.3 react-router-dom的理解

1.react的一个插件库。

2.专门用来实现一个SPA应用。

3.基于react的PC项目基本都会用到此库。

### 1.4 前端路由的原理（重点！）

- 前端路由是如何做到URL和内容进行映射呢？监听URL的改变。

- URL发生变化，同时不引起页面的刷新有两个办法：
  - 通过URL的hash改变URL；
  - 通过HTML5中的history修改URL；

- 当监听到URL发生变化时，我们可以通过自己判断当前的URL，决定到底渲染什么样的内容。

1.URL的hash

- URL的hash也就是锚点(#), 本质上是改变window.location的href属性

- 我们可以通过直接赋值location.hash来改变href, 但是页面不发生刷新

**实例：**

```html
<div id="app">
    <a href="#/home">首页</a>
    <a href="#/about">关于</a>

    <div class="router-view"></div>
</div>

<script>
    // 获取router-view的DOM
    const routerViewEl = document.getElementsByClassName("router-view")[0];

    // 监听URL的改变 - hashchange事件
    window.addEventListener("hashchange", () => {
        switch (location.hash) {
            case "#/home":
                routerViewEl.innerHTML = "首页";
                break;
            case "#/about":
                routerViewEl.innerHTML = "关于";
                break;
            default:
                routerViewEl.innerHTML = "";
        }
    })
</script>
```

**注：**

- hash的优势就是兼容性更好，在老版IE中都可以运行
- 但是缺陷是有一个#，显得不像一个真实的路径

2.HTML5的history

history接口是HTML5新增的, 它有六种模式改变URL而不刷新页面：

- replaceState：替换原来的路径

- pushState：使用新的路径

- popState：路径的回退

- go：向前或向后改变路径

- forward：向前改变路径

- back：向后改变路径

```html
<div id="app">
    <a href="/home">首页</a>
    <a href="/about">关于</a>

    <div class="router-view"></div>
</div>

<script>
    // 1.获取router-view的DOM
    const routerViewEl = document.getElementsByClassName("router-view")[0];

    // 获取所有的a元素, 自己来监听a元素的改变
    const aEls = document.getElementsByTagName("a");
    for (let el of aEls) {
        el.addEventListener("click", e => {
            e.preventDefault();
            const href = el.getAttribute("href");
            history.pushState({}, "", href);
            urlChange();
        })
    }

    // 执行返回操作时, 依然来到urlChange
    window.addEventListener('popstate',urlChange);

    // 监听URL的改变
    function urlChange() {
        switch (location.pathname) {
            case "/home":
                routerViewEl.innerHTML = "首页";
                break;
            case "/about":
                routerViewEl.innerHTML = "关于";
                break;
            default:
                routerViewEl.innerHTML = "";
        }
    }
</script>
```

## 2.路由的基本使用

1）准备

​	1.下载 react-router-dom

​	yarn add react-router-dom

2）使用

​	1.明确好界面中的导航区、展示区

​	2.导航区的a标签改为Link标签

```jsx
<Link to="/xxxxx">Demo</Link>
```

​	3.展示区写Route标签进行路径的匹配

```jsx
<Route path='/xxxx' component={Demo}/>
```

​	4.< App >的最外侧包裹了一个< BrowserRouter >或< HashRouter >

- 即全局只能有一个BrowserRouter 或 HashRouter

**实例：**

```jsx
<BrowserRouter>
    <Link to="/">首页</Link>
    <Link to="/about">关于</Link>
    <Link to="/profile">我的</Link>
    <Route exact path="/" component={Home} />
    <Route path="/about" component={About} />
    <Route path="/profile" component={Profile} />
</BrowserRouter>
```

## 3.路由组件与一般组件（重点！）

1.写法不同：

​    一般组件：

```jsx
<Demo/>
```

​    路由组件：

```jsx
<Route path="/demo" component={Demo}/>
```

2.存放位置不同：

​    一般组件：components

​    路由组件：pages

3.接收到的props不同：

​    一般组件：写组件标签时传递了什么，就能收到什么

​    路由组件：接收到三个固定的属性

​        history:

​            go: ƒ go(n)

​            goBack: ƒ goBack()

​            goForward: ƒ goForward()

​            push: ƒ push(path, state)

​            replace: ƒ replace(path, state)

​        location:

​            pathname: "/about"

​            search: ""

​            state: undefined

​        match:

​            params: {}

​            path: "/about"

​            url: "/about"

## 4.NavLink与封装NavLink

1.NavLink可以实现路由链接的高亮，通过activeClassName指定样式名

- 如果未指定activeClassName，默认以active类为样式

```jsx
<NavLink to="/home" activeClassName="link-active">首页</NavLink>
<NavLink to="/about" activeClassName="link-active">关于</NavLink>
<NavLink to="/profile" activeClassName="link-active">我的</NavLink>
```

2.封装NavLink - childern props

```jsx
//MyNavLink.jsx
<NavLink activeClassName="atguigu" className="list-group-item" {...this.props} />

//app.jsx
<MyNavLink to="/about">About</MyNavLink>
```

- 注：

​	1) 使用{...this.props}整体传参，包括标签体childern, 组件标签体内容是特殊的标签属性，因此可简写单标签闭合的形式。

​	2) 标签体传参, 通过this.props.childern获取组件标签体内容。

## 5.Switch的使用

1.通常情况下，path和component是一一对应的关系。

2.Switch可以提高路由匹配效率(单一匹配)。

3.Switch放在路由的外层

```jsx
<Switch>
    <Route path="/about" component={About}/>
    <Route path="/home" component={Home}/>
    <Route path="/home" component={Test}/>
</Switch>
```

## 6.解决多级路径刷新页面样式丢失的问题

1.public/index.html 中 引入样式时不写 ./ 写 / （常用）

2.public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）

3.使用HashRouter

## 7.路由的严格匹配与模糊匹配

1.默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）

2.开启严格匹配：exact={true} 或 exact

```jsx
<Route exact={true} path="/about" component={About}/>
```

或

```jsx
<Route exact path="/about" component={About}/>
```

3.严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由

## 8.Redirect的使用

1.一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由

2.具体编码：

```jsx
<Switch>
    <Route path="/about" component={About}/>
    <Route path="/home" component={Home}/>
    <Redirect to="/about"/>
</Switch>
```

## 9.嵌套（多级）路由

1.注册子路由时要写上父路由的path值

```jsx
<MyNavLink to="/home/news">News</MyNavLink>

<Route path="/home/news" component={News}/>
```

2.路由的匹配是按照注册路由的顺序进行的

## 10.向路由组件传递参数（重点！）

1.params参数 - 动态路由

​    1）路由链接(携带参数)：

```jsx
<Link to='/demo/test/tom/18'>详情</Link>
```

​    2）注册路由(声明接收)：

```jsx
<Route path="/demo/test/:name/:age" component={Test}/>
```

​    3）接收参数：

```js
const {name, age} = this.props.match.params
```

2.search参数

​    1）路由链接(携带参数)：

```jsx
<Link to='/demo/test?name=tom&age=18'}>详情</Link>
```

​    2）注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/demo/test" component={Test}/>
```

​    3）接收参数：

```js
this.props.location.search

const {search} = this.props.location
qs.parse(search.slice(1))
```

- **注：** 获取到的search是urlencoded编码字符串，需要借助querystring解析！

**3.state参数 - Link中to传入对象（推荐！）**

​    1）路由链接(携带参数)：

```jsx
<Link to={{pathname:'/demo/test', state:{name:'tom',age:18}}}>详情</Link>
```

​    2）注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/demo/test" component={Test}/>
```

​    3）接收参数：

```js
this.props.location.state
```

- 注：刷新也可以保留参数

## 11.路由跳转方式

1.push

- 默认为push，会留下历史记录

2.replace

- 替换掉上一次的跳转记录，且不会留下历史记录

- 在Link与NavLink中加上replace或replace={true}，即开启replace方式跳转

**注：**

- 浏览器历史记录是栈结构

## 12.编程式路由导航（重点！）

借助this.props.history对象上的API对操作路由跳转、前进、后退

- this.props.history.push(path, state)

- this.props.history.replace(path, state)

- this.props.history.goBack()

- this.props.history.goForward()

- this.props.history.go(n)

**实例**

```react
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
	state = {
		messageArr:[
			{id:'01',title:'消息1'},
			{id:'02',title:'消息2'},
			{id:'03',title:'消息3'},
		]
	}

	replaceShow = (id,title)=>{
		//replace跳转+携带params参数
		//this.props.history.replace(`/home/message/detail/${id}/${title}`)

		//replace跳转+携带search参数
		// this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

		//replace跳转+携带state参数
		this.props.history.replace(`/home/message/detail`,{id,title})
	}

	pushShow = (id,title)=>{
		//push跳转+携带params参数
		// this.props.history.push(`/home/message/detail/${id}/${title}`)

		//push跳转+携带search参数
		// this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

		//push跳转+携带state参数
		this.props.history.push(`/home/message/detail`,{id,title})
		
	}

	back = ()=>{
		this.props.history.goBack()
	}

	forward = ()=>{
		this.props.history.goForward()
	}

	go = ()=>{
		this.props.history.go(-2)
	}

	render() {
		const {messageArr} = this.state
		return (
			<div>
				<ul>
					{
						messageArr.map((msgObj)=>{
							return (
								<li key={msgObj.id}>

									{/* 向路由组件传递params参数 */}
									{/* <Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递search参数 */}
									{/* <Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递state参数 */}
									<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>

									&nbsp;<button onClick={()=> this.pushShow(msgObj.id,msgObj.title)}>push查看</button>
									&nbsp;<button onClick={()=> this.replaceShow(msgObj.id,msgObj.title)}>replace查看</button>
								</li>
							)
						})
					}
				</ul>
				<hr/>
				{/* 声明接收params参数 */}
				{/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

				{/* search参数无需声明接收，正常注册路由即可 */}
				{/* <Route path="/home/message/detail" component={Detail}/> */}

				{/* state参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>
				
				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>
			</div>
		)
	}
}
```

## 13.withRouter函数的使用

1.withRouter函数可以加工一般组件，让一般组件具备路由组件所特有的API

2.withRouter函数的返回值是一个新组件

**注：**

- 使用withRouter高阶组件包裹的一般组件必须在Router（hash或browser）组件之内

**实例**

```react
//Header.js
import React, { Component } from 'react'
import {withRouter} from 'react-router-dom'

class Header extends Component {

	back = ()=>{
		this.props.history.goBack()
	}

	forward = ()=>{
		this.props.history.goForward()
	}

	go = ()=>{
		this.props.history.go(-2)
	}

	render() {
		console.log('Header组件收到的props是',this.props);
		return (
			<div className="page-header">
				<h2>React Router Demo</h2>
				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>
			</div>
		)
	}
}

export default withRouter(Header)

//app.js
<BrowserRouter>
	<Header /> 
</BrowserRouter>
```

## 14.BrowserRouter与HashRouter的区别

1.底层原理不一样：

​    BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。

​    HashRouter使用的是URL的哈希值（window.hash）。

2.path表现形式不一样

​    BrowserRouter的路径中没有#, 例如：localhost:3000/demo/test

​    HashRouter的路径包含#, 且#后面的内容不会作为请求地址发给服务器, 例如：localhost:3000/#/demo/test

3.刷新后对路由state参数的影响

​    (1).BrowserRouter没有任何影响，因为state保存在history对象中。

​    (2).HashRouter刷新后会导致路由state参数的丢失！！！

4.**注：** HashRouter可以用于解决一些路径错误相关的问题。

## 15.react-router-config

- 目前我们所有的路由定义都是直接使用Route组件，并且添加属性来完成的。

- 但是这样的方式会让路由变得非常混乱，我们希望将所有的路由配置放到一个地方进行集中管理：
  - 这个时候可以使用react-router-config来完成

- 安装react-router-config
  - yarn add react-router-config

- 配置路由映射的关系数组

```js
const routes = [
  {
    path: "/",
    exact: true,
    // component: WYDiscover,
    render: () => <Redirect to="/discover" />,
  },
  {
    path: "/discover",
    component: WYDiscover,
    routes: [
      {
        path: "/discover",
        exact: true,
        render: () => <Redirect to="/discover/recommend" />,
      },
      {
        path: "/discover/recommend",
        component: WYRecommend,
      },
      {
        path: "/discover/ranking",
        component: WYRanking,
      },
      {
        path: "/discover/songs",
        component: WYSongs,
      },
      {
        path: "/discover/djradio",
        exact: true,
        component: WYDjradio,
      },
      {
        path: "/discover/artist",
        component: WYArtist,
      },
      {
        path: "/discover/album",
        component: WYAlbum,
      },
      {
        path: "/discover/player",
        component: WYPlayer,
      }
    ],
  },
  {
    path: "/mine",
    component: WYMine,
  },
  {
    path: "/friend",
    component: WYFriend,
  },
];

export default routes;
```

- 使用renderRoutes函数完成配置

```
{renderRoutes(routes)}
```

- 有二级路由的组件可以用通过this.props.route获取子路由数组

```jsx
//WYDiscover.jsx
...
{renderRoutes(this.props.route.routes)}
```



