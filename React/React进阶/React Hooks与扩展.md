# React进阶3

## 1. setState更新状态的2种写法

0). setState简介

- 疑惑：在组件中并没有实现setState的方法，为什么可以调用呢？
- 原因很简单，setState方法是从Component中继承过来的。

- react更新界面的流程：
  - 调用setState会触发react去更新状态，状态更新完毕之后react调用render方法去更新页面

1). setState(stateChange, [callback]) ----- 对象式的setState (常用)

​    1.stateChange为状态改变对象(该对象可以体现出状态的更改)

​    2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用

2). setState(updater, [callback]) ----- 函数式的setState

​    1.updater为返回stateChange对象的函数。

​    2.updater可以接收到state和props。即：(state,props) => stateChange。

​    3.updater可以保证自己接受到的state和props是最新的。

​    4.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。

**3). setState() 数据的合并：**

- 根据传入的对象的key修改原state对象属性对应的值

- setState()原理是Object.assign({}, preState, setState传入的对象)
- 实例：

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      message: "Hello World",
      name: "admin"
    }
  }

  render() {
    return (
      <div>
        <h2>{this.state.message}</h2>
        <h2>{this.state.name}</h2>
        <button onClick={e => this.changeText()}>改变文本</button>
      </div>
    )
  }

  changeText() {
    // 了解真相你才能获得真正的自由
    this.setState({
      message: "你好,admin"
    });

    // Object.assign({}, this.state, {message: "你好啊,admin"})
  }
}
```

4). 为什么setState设计为异步呢？

- setState设计为异步，可以显著的提升性能；
- 如果每次调用 setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染，这样效率是很低的；
- 最好的办法应该是获取到多个更新，之后进行批量更新；
- 如果同步更新了state，但是还没有执行render函数，那么state和props不能保持同步；
- state和props不能保持一致性，会在开发中产生很多的问题；

5). 那么如何可以获取到异步更新后的值呢？

**方式一：setState的回调**

- setState接受两个参数：第二个参数是一个回调函数，这个回调函数会在更新后会执行；
- 格式如下：setState(更新的state, 回调函数)

**方式二：在生命周期componentDidUpdate()中**

```js
componentDidUpdate() {
    console.log(this.state.message);
}
```

**总结:**

​    1.setState一般是异步更新状态！！！

​    2.对象式的setState是函数式的setState的简写方式(语法糖)

​    3.使用原则：

​        (1).如果新状态不依赖于原状态 => 使用对象方式

​        (2).如果新状态依赖于原状态 => 使用函数方式

​        (3).如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取

**实例**

```jsx
export default class Demo extends Component {

	state = {count:0}

	add = ()=>{
		//对象式的setState
		/* //1.获取原来的count值
		const {count} = this.state
		//2.更新状态
		this.setState({count:count+1},()=>{
			console.log(this.state.count);
		})
		//console.log('12行的输出',this.state.count); //0 */

		//函数式的setState
		this.setState( state => ({count:state.count+1}))
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{this.state.count}</h1>
				<button onClick={this.add}>点我+1</button>
			</div>
		)
	}
}
```

## 2. setState的同步与异步

- setState()更新状态的动作是同步还是异步的？---> 看它的执行位置

- setState()执行的位置对其后续动作的影响：

​	1.在由react所控制的回调函数中更新的动作是【异步】的。

​	如：生命周期钩子 / react所监听的事件（react合成事件）

​	2.在非react控制的异步回调函数中更新的动作是【同步】的。

​	如：定时器的回调 / 原生事件监听回调 / Promise所指定的成功或失败回调 / ajax所指定的成功或失败的回调

**实例：**

```react
class App extends Component {

	state = {
		count:0
	}

	//在react生命周期钩子中，setState执行更新的动作是异步的
	componentDidMount(){
		console.log('调用setState之前',this.state.count);
		this.setState({count:1})
		console.log('调用setState之后',this.state.count);
	}

	//在react所监听的事件回调中，setState执行更新的动作是异步的
	update1 = ()=>{
		console.log('调用setState之前',this.state.count);
		this.setState({count:1})
		console.log('调用setState之后',this.state.count);
	}

	//在定时器的回调中，setState执行更新的动作是同步的
	update2 = ()=>{
		setTimeout(()=>{
			console.log('调用setState之前',this.state.count);
			this.setState({count:1})
			console.log('调用setState之后',this.state.count);
		},500)
	}

	//在原生事件监听回调，setState执行更新的动作是同步的
	update3 = ()=>{
		const h1 = this.refs.text
		h1.onclick = ()=>{
			console.log('调用setState之前',this.state.count);
			this.setState({count:1})
			console.log('调用setState之后',this.state.count);
		}
	}

	//在Promise所指定的成功、失败回调，setState执行更新的动作是同步的
	update4 = ()=>{
		Promise.resolve('ok').then(()=>{
			console.log('调用setState之前',this.state.count);
			this.setState({count:1})
			console.log('调用setState之后',this.state.count);
		})
	}

	render() {
		console.log('render执行了',this.state.count);
		return (
			<div>
				<h1 ref="text">当前求和为:{this.state.count}</h1>
				<button onClick={this.update1}>更新1</button>&nbsp;
				<button onClick={this.update2}>更新2</button>&nbsp;
				<button onClick={this.update3}>更新3</button>&nbsp;
				<button onClick={this.update4}>更新4</button>&nbsp;
			</div>
		)
	}
}
```

## 3.关于异步的setState()连续多次调用的问题

(1).多次调用，如何处理？

​	(a).若是对象式的setState，即：setState({})

​		多次更新状态的动作合并为一次(只以最后一次为准),所以很自然的就调用一次render

​		【简记：更新的动作、render的调用，均合并成一次，且以最后一次为准】

​	(b).若是函数式的setState，即：setState(()=>{})

​		每次更新的动作都会生效(更新的动作不合并)，但是只会调用一次render

​		【简记：更新状态的动作没有合并，但render合并】

(2).注意：

- 若函数式setState和对象式setState混用的时候，要把函数式写在最后

(3).如何得到异步更新后的状态？

- 在setState所指定的那个回调里面

**实例：**

```react
class App extends Component {

	state = {
		count:0
	}

	/* 	关于异步的setState()连续多次调用的问题 */
	//在由react所控制的update1中，连续4次调用setState({})
	update1 = ()=>{
		// console.log('调用setState之前',this.state.count);
		// this.setState({count:this.state.count+1})
		// this.setState({count:this.state.count+2})
		// this.setState({count:this.state.count+3})
		// this.setState({count:this.state.count+100})
		// console.log('调用setState之后',this.state.count);
	}

	//在由react所控制的update2中，连续4次调用setState({})
	update2 = ()=>{
		console.log('调用setState之前',this.state.count);
		this.setState(state => ({count:state.count+1}))
		this.setState(state => ({count:state.count+2}))
		this.setState(state => ({count:state.count+3}))
		this.setState(state => ({count:state.count+4}))
		console.log('调用setState之后',this.state.count);
	}

	//在由react所控制的update3中，连续2次调用setState(),第一次使用对象式，第二次使用函数式
	update3 = ()=>{
		// console.log('调用setState之前',this.state.count);
		// this.setState({count:this.state.count+1}) //对象式
		// this.setState(state => ({count:state.count+2})) //函数式
		// console.log('调用setState之后',this.state.count);
	}

	//在由react所控制的update4中，连续2次调用setState(),第一次使用函数式，第二次使用对象式
	update4 = ()=>{
		// console.log('调用setState之前',this.state.count);
		// this.setState(state => ({count:state.count+1})) //函数式
		// this.setState({count:this.state.count+2}) //对象式
		// console.log('调用setState之后',this.state.count);
	}

	update5  = ()=>{
		this.setState({count:9000},()=>{
			console.log(this.state.count);
		})
	}
	
	render() {
		console.log('render执行了',this.state.count);
		return (
			<div>
				<h1 ref="text">当前求和为:{this.state.count}</h1>
				<button onClick={this.update1}>更新1</button>&nbsp;
				<button onClick={this.update2}>更新2</button>&nbsp;
				<button onClick={this.update3}>更新3</button>&nbsp;
				<button onClick={this.update4}>更新4</button>&nbsp;
				<button onClick={this.update5}>更新5</button>&nbsp;
			</div>
		)
	}
}
```

## 4. 路由组件的懒加载（lazyLoad）

1）引入lazy函数, Suspense组件

​	import {lazy, Suspense} from 'react'

2）通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包

​	const Login = lazy(()=>import('../pages/Login'))

3）通过< Suspense >组件指定在加载得到路由打包文件前显示一个自定义loading界面

- 将路由放在Suspense组件里面，并传一个自定义的loading界面（组件）

```jsx
<Suspense fallback={<Loading/>}>
{/*<Suspense fallback={<h1>loading.....</h1>}>*/}
	<Switch>
    	<Route path="/xxx" component={Xxxx}/>
        <Route path="/xxx" component={Xxxx}/>
        <Redirect to="/login"/>
    </Switch>
</Suspense>
```

**实例**

```jsx
//app.js
import React, { Component,lazy,Suspense} from 'react'
import Loading from './Loading'
const Home = lazy(()=> import('./Home') )
const About = lazy(()=> import('./About'))
<Suspense fallback={<Loading/>}>
    {/* 注册路由 */}
    <Route path="/about" component={About}/>
    <Route path="/home" component={Home}/>
</Suspense>

//Loading.jsx
export default class Loading extends Component {
	render() {
		return (
			<div>
				<h1 style={{backgroundColor:'gray',color:'orange'}}>Loading....</h1>
			</div>
		)
	}
}
```

## 5. Hooks

### 5.1 React Hooks是什么?

1). Hooks是React 16.8.0版本增加的新特性/新语法

2). 可以让你在函数组件中使用 state 以及其他的 React 特性（比如生命周期）

**注：** 使用Hooks会有两个额外的规则：

- **只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。**

- **只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。**

 **Hooks使用规则的原因：**

- Hooks 的设计是基于 **数组** 实现。在调用时按顺序加入数组中，如果使用循环、条件或嵌套函数很有可能导致数组取值错位，执行错误的 Hook。当然，实质上 React 的源码里不是数组，是 **链表** 。
- React需要利用调用顺序来正确更新相应的状态，以及调用相应的钩子函数。一旦在循环或条件分支语句中调用Hook，就容易导致调用顺序的不一致性，从而产生难以预料到的后果。

### 5.2 Class组件存在的问题

- 复杂组件变得难以理解：
  - 我们在最初编写一个class组件时，往往逻辑比较简单，并不会非常复杂。但是随着业务的增多，我们的class组件会变得越来越复杂；
  - 比如componentDidMount中，可能就会包含大量的逻辑代码：包括网络请求、一些事件的监听（还需要在componentWillUnmount中移除）；
  - 而对于这样的class实际上非常难以拆分：因为它们的逻辑往往混在一起，强行拆分反而会造成过度设计，增加代码的复杂度；
- 难以理解的class：
  - 很多人发现学习ES6的class是学习React的一个障碍。
  - 比如在class中，我们必须搞清楚this的指向到底是谁，所以需要花很多的精力去学习this；
  - 虽然我认为前端开发人员必须掌握this，但是依然处理起来非常麻烦；
- 组件复用状态很难：
  - 在前面为了一些状态的复用我们需要通过高阶组件或render props；
  - 像我们之前学习的redux中connect或者react-router中的withRouter，这些高阶组件设计的目的就是为了状态的复用；
  - 或者类似于Provider、Consumer来共享一些状态，但是多次使用Consumer时，我们的代码就会存在很多嵌套；
  - 这些代码让我们不管是编写和设计上来说，都变得非常困难；

### 5.3 三个常用的Hook

1). State Hook: React.useState()

2). Effect Hook: React.useEffect()

3). Ref Hook: React.useRef()

#### 5.3.1 State Hook

1). State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作

2). 语法: const [xxx, setXxx] = React.useState(initValue)

- 扩展写法：const [xxx, setXxx] = React.useState(() => initValue)

3). useState()说明:

- 参数: 第一次初始化指定的值在内部作缓存，如果不设置为undefined

- 返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数

4). setXxx()2种写法:

- setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值

- setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值

**注：** 

- setXxx与setState的特点一样

- **如果初始值需要计算才能得到可以使用useState传入回调函数的形式确定初始值**
- **useState设置状态的时候，只有第一次生效，后期需要更新状态，必须通过useEffect或事件回调里调用setXXX**

**实例1：** 多个状态的使用

```jsx
import React, { useState } from 'react';

export default function MultiHookState() {

  const [count, setCount] = useState(0);
  const [age, setAge] = useState(18);
  const [friends, setFriends] = useState(["kobe", "lilei"]);

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <h2>我的年龄: {age}</h2>
      <ul>
        {
          friends.map((item, index) => {
            return <li key={item}>{item}</li>
          })
        }
      </ul>
    </div>
  )
}
```

**实例2：** 复杂状态的修改

```jsx
import React, { useState } from 'react'

export default function ComplexHookState() {

  const [friends, setFrineds] = useState(["kobe", "lilei"]);
  const [students, setStudents] = useState([
    { id: 110, name: "why", age: 18 },
    { id: 111, name: "kobe", age: 30 },
    { id: 112, name: "lilei", age: 25 },
  ])

  // 错误的做法
  function addFriend() {
    friends.push("hmm");
    setFrineds(friends);
  }
  //正确做法
  function incrementAgeWithIndex(index) {
    const newStudents = [...students];
    newStudents[index].age += 1;
    setStudents(newStudents);
  }

  return (
    <div>
      <h2>好友列表:</h2>
      <ul>
        {
          friends.map((item, index) => {
            return <li key={index}>{item}</li>
          })
        }
      </ul>
      <button onClick={e => setFrineds([...friends, "tom"])}>添加朋友</button>
      {/* 错误的做法 */}
      <button onClick={addFriend}>添加朋友</button>

      <h2>学生列表</h2>
      <ul>
        {
          students.map((item, index) => {
            return (
              <li key={item.id}>
                <span>名字: {item.name} 年龄: {item.age}</span>
                <button onClick={e => incrementAgeWithIndex(index)}>age+1</button>
              </li>
            )
          })
        }
      </ul>
    </div>
  )
}
```

#### 5.3.2 Effect Hook

1). Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)

2). React中的副作用操作:

- 发ajax请求数据获取

- 设置订阅 / 启动定时器

- 手动更改真实DOM

3). 语法和说明: 

```js
useEffect(() => { 
    // 在此可以执行任何带副作用操作
    return () => { // 在组件卸载前执行
        // 在此做一些收尾工作, 比如清除定时器/取消订阅等
    }
}, [stateValue])
// 如果指定的是[], 回调函数只会在第一次render()后执行
// 如果指定的是[stateValue], 回调函数则根据stateValue的改变而执行
// 如果不传[], 默认情况下，无论是第一次渲染之后，还是每次更新之后，都会执行这个回调函数
```

4). 可以把 useEffect Hook 看做如下三个函数的组合

- componentDidMount()

- componentDidUpdate()

- componentWillUnmount()

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

//类式组件
/* class Demo extends React.Component {

	state = {count:0}

	add = ()=>{
		this.setState(state => ({count:state.count+1}))
	}

	unmount = ()=>{
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	componentDidMount(){
		this.timer = setInterval(()=>{
			this.setState( state => ({count:state.count+1}))
		},1000)
	}

	componentWillUnmount(){
		clearInterval(this.timer)
	}

	render() {
		return (
			<div>
				<h2>当前求和为{this.state.count}</h2>
				<button onClick={this.add}>点我+1</button>
				<button onClick={this.unmount}>卸载组件</button>
			</div>
		)
	}
} */

//函数式组件
function Demo(){
	//console.log('Demo');

	const [count,setCount] = React.useState(0)

	React.useEffect(()=>{
		let timer = setInterval(()=>{
			setCount(count => count+1 )
		},1000)
		return ()=>{
			clearInterval(timer)
		}
	},[])

	//加的回调
	function add(){
		//setCount(count+1) //第一种写法
		setCount(count => count+1 ) //第二种写法
	}

	//卸载组件的回调
	function unmount(){
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	return (
		<div>
			<h2>当前求和为：{count}</h2>
			<button onClick={add}>点我+1</button>
			<button onClick={unmount}>卸载组件</button>
		</div>
	)
}

export default Demo
```

5). 使用多个Effect

- 使用Effect Hook的其中一个目的就是解决class中生命周期经常将很多的逻辑放在一起的问题：
  - 比如网络请求、事件监听、手动修改DOM，这些往往都会放在componentDidMount中；

- 使用Effect Hook，我们可以将它们分离到不同的useEffect中

- Hook 允许我们按照代码的用途分离它们， 而不是像生命周期函数那样经常将很多的逻辑放在一起
  - React 将按照 effect 声明的顺序依次调用组件中的每一个 effect；

```jsx
import React, { useState, useEffect } from 'react'

export default function MultiEffectHookDemo() {
  const [count, setCount] = useState(0);
  const [isLogin, setIsLogin] = useState(true);

  useEffect(() => {
    console.log("修改DOM", count);
  }, [count]);

  useEffect(() => {
    console.log("订阅事件");
  }, []);

  useEffect(() => {
    console.log("网络请求");
  }, []);

  return (
    <div>
      <h2>MultiEffectHookDemo</h2>
      <h2>{count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <h2>{isLogin ? "admin": "未登录"}</h2>
      <button onClick={e => setIsLogin(!isLogin)}>登录/注销</button>
    </div>
  )
}
```

#### 5.3.3 Ref Hook

1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据

2). 语法: const refContainer = useRef()

3). 作用：保存标签对象，功能与React.createRef()一样

4). 常见用法：

**用法1：** 引用DOM（或者组件，但是需要是class组件）元素

```jsx
import React, { useRef } from 'react';

class TestCpn extends React.Component {
  render() {
    return <h2>TestCpn</h2>
  }
}

function TestCpn2(props) {
  return <h2>TestCpn2</h2>
}

export default function RefHookDemo01() {

  const titleRef = useRef();
  const inputRef = useRef();
  const testRef = useRef();
  const testRef2 = useRef();

  function changeDOM() {
    titleRef.current.innerHTML = "Hello World";
    inputRef.current.focus();
    console.log(testRef.current);
    console.log(testRef2.current);
  }

  return (
    <div>
      <h2 ref={titleRef}>RefHookDemo01</h2>
      <input ref={inputRef} type="text"/>
      <TestCpn ref={testRef}/>
      {/* 会报警告 */}
      <TestCpn2 ref={testRef2}/>

      <button onClick={e => changeDOM()}>修改DOM</button>
    </div>
  )
}
```

**用法2：** 保存一个数据，这个对象在整个生命周期中可以保存不变

- 语法: const refContainer = useRef(initValue)

**实例：** 使用ref保存上一次的某一个值

```jsx
import React, { useRef, useState, useEffect } from 'react'

export default function RefHookDemo02() {
  const [count, setCount] = useState(0);

  const numRef = useRef(count);

  useEffect(() => {
    numRef.current = count;
  }, [count])

  return (
    <div>
      {/* <h2>numRef中的值: {numRef.current}</h2>
      <h2>count中的值: {count}</h2> */}
      <h2>count上一次的值: {numRef.current}</h2>
      <h2>count这一次的值: {count}</h2>
      <button onClick={e => setCount(count + 10)}>+10</button>
    </div>
  )
}
```

### 5.4 useContext

- 在之前的开发中，我们要在组件中使用共享的Context有两种方式：
  - 类组件可以通过 类名.contextType = MyContext方式，在类中获取context；
  - 多个Context或者在函数式组件中通过 MyContext.Consumer 方式共享context；

- 但是多个Context共享时的方式会存在大量的嵌套：
  - Context Hook允许我们通过Hook来直接获取某个Context的值

**注：**

- 当组件上层最近的 <MyContext.Provider> 更新时，该 Hook 会触发重新渲染，并使用最新传递给 MyContext provider 的 context value 值。

**实例：**

```jsx
//app.js
export const UserContext = createContext();
export default function App() {
 
  return (
    <div>
      {/* 4.useContext */}
      <UserContext.Provider value={{name: "why", age: 18}}>
          <ContextHookDemo/>
      </UserContext.Provider>
    </div>
  )
}

//ContextHookDemo.jsx
import React, { useContext } from 'react';
import { UserContext } from "../App";

export default function ContextHookDemo(props) {
  const user = useContext(UserContext);

  console.log(user);

  return (
    <div>  
      <h2>ContextHookDemo{user.name}，{user.age}</h2>
    </div>
  )
}
```

### 5.5 useReducer

- 很多人看到useReducer的第一反应应该是redux的某个替代品，其实并不是。

- useReducer仅仅是useState的一种替代方案：
  - 在某些场景下，如果state的处理逻辑比较复杂，我们可以通过useReducer来对其进行拆分；
  - 或者这次修改的state需要依赖之前的state时，也可以使用；

```jsx
//reducer.js
export default function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { ...state, counter: state.counter + 1 };
    case "decrement":
      return { ...state, counter: state.counter - 1 };
    default:
      return state;
  }
}

//home.js
import React, { useState, useReducer } from "react";
import reducer from "./reducer";

export default function Home() {
  // const [count, setCount] = useState(0);
  const [state, dispatch] = useReducer(reducer, { counter: 0 });

  return (
    <div>
      <h2>Home当前计数: {state.counter}</h2>
      <button onClick={(e) => dispatch({ type: "increment" })}>+1</button>
      <button onClick={(e) => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}

//profile.js
import React, { useReducer } from "react";
import reducer from "./reducer";

export default function Profile() {
  // const [count, setCount] = useState(0);
  const [state, dispatch] = useReducer(reducer, { counter: 0 });

  return (
    <div>
      <h2>Profile当前计数: {state.counter}</h2>
      <button onClick={(e) => dispatch({ type: "increment" })}>+1</button>
      <button onClick={(e) => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}
```

### 5.6 useCallback

- useCallback实际的目的是为了进行性能的优化。

- 如何进行性能的优化呢？
  - useCallback会返回一个函数的 memoized（记忆的） 值；
  - 在依赖不变的情况下，多次定义的时候，返回的值是相同的；

```js
const memoizedCallback = useCallback(
    () => {
        doSomething(a，b);
    },
    [a, b]
);
```

**案例一：** 使用useCallback和不使用useCallback定义一个函数是否会带来性能的优化；

```jsx
import React, {useState, useCallback, useMemo} from 'react'

export default function CallbackHookDemo01() {
  const [count, setCount] = useState(0);

  const increment1 = () => {
    console.log("执行increment1函数");
    setCount(count + 1);
  }

  //如果数组为[]则函数只会定义一次
  const increment2 = useCallback(() => {
    console.log("执行increment2函数");
    setCount(count + 1);
  }, [count]);

  //使用useMemo实现useCallback
  const increment3 = useMemo(() => {
    return () => {
      console.log("执行increment2函数");
      setCount(count + 1);
    }
  }, [count]);

  return (
    <div>
      <h2>CallbackHookDemo01: {count}</h2>
      <button onClick={increment1}>+1</button>
      <button onClick={increment2}>+1</button>
    </div>
  )
}
```

**案例二：** 使用useCallback和不使用useCallback定义一个函数传递给子组件是否会带来性能的优化；

- 通常使用useCallback的目的是不希望子组件进行多次渲染，并不是为了函数进行缓存。

```jsx
import React, {useState, useCallback, memo} from 'react';

/**
 * useCallback在什么时候使用?
 * 场景: 在将一个组件中的函数, 传递给子元素进行回调使用时, 使用useCallback对函数进行处理.
 */

const HYButton = memo((props) => {
  console.log("HYButton重新渲染: " + props.title);
  return <button onClick={props.increment}>HYButton +1</button>
});

export default function CallbackHookDemo02() {
  console.log("CallbackHookDemo02重新渲染");

  const [count, setCount] = useState(0);
  const [show, setShow] = useState(true);

  const increment1 = () => {
    console.log("执行increment1函数");
    setCount(count + 1);
  }

  const increment2 = useCallback(() => {
    console.log("执行increment2函数");
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <h2>CallbackHookDemo01: {count}</h2>
      {/* <button onClick={increment1}>+1</button>
      <button onClick={increment2}>+1</button> */}
      <HYButton title="btn1" increment={increment1}/>
      <HYButton title="btn2" increment={increment2}/>

      <button onClick={e => setShow(!show)}>show切换</button>
    </div>
  )
}
```

### 5.7 useMemo

- useMemo实际的目的也是为了进行性能的优化。

- 如何进行性能的优化呢？
  - useMemo返回的也是一个 memoized（记忆的） 值；
  - 在依赖不变的情况下，多次定义的时候，返回的值是相同的；

```jsx
const memoizedvalue = useMemo(
    () => {
        doSomething(a, b)
    },
    [a, b]
);
```

- 案例一：进行大量的计算操作，是否有必须要每次渲染时都重新计算；

```jsx
import React, {useState, useMemo} from 'react';

function calcNumber(count) {
  console.log("calcNumber重新计算");
  let total = 0;
  for (let i = 1; i <= count; i++) {
    total += i;
  }
  return total;
}

export default function MemoHookDemo01() {
  const [count, setCount] = useState(10);
  const [show, setShow] = useState(true);

  // const total = calcNumber(count);
  const total = useMemo(() => {
    return calcNumber(count);
  }, [count]);

  return (
    <div>
      <h2>计算数字的和: {total}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <button onClick={e => setShow(!show)}>show切换</button>
    </div>
  )
}
```

- 案例二：对子组件传递相同内容的对象时，使用useMemo进行性能的优化

```jsx
import React, { useState, memo, useMemo } from 'react';

const HYInfo = memo((props) => {
  console.log("HYInfo重新渲染");
  return <h2>名字: {props.info.name} 年龄: {props.info.age}</h2>
});

export default function MemoHookDemo02() {
  console.log("MemoHookDemo02重新渲染");
  const [show, setShow] = useState(true);

  // const info = { name: "why", age: 18 };
  const info = useMemo(() => {
    return { name: "why", age: 18 };
  }, []);

  return (
    <div>
      <HYInfo info={info} />
      <button onClick={e => setShow(!show)}>show切换</button>
    </div>
  )
}
```

### 5.8 useImperativeHandle

- 我们先来回顾一下ref和forwardRef结合使用：
  - 通过forwardRef可以将ref转发到子组件；
  - 子组件拿到父组件中创建的ref，绑定到自己的某一个元素中；

```jsx
import React, { useRef, forwardRef } from 'react';

const HYInput = forwardRef((props, ref) => {
  return <input ref={ref} type="text"/>
})

export default function ForwardRefDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```

- forwardRef的做法本身没有什么问题，但是我们是将子组件的DOM直接暴露给了父组件：
  - 直接暴露给父组件带来的问题是某些情况的不可控；
  - 父组件可以拿到DOM后进行任意的操作；
  - 但是，事实上在上面的案例中，我们只是希望父组件可以操作的focus，其他并不希望它随意操作；

- 通过useImperativeHandle可以暴露固定的操作：
  - 通过useImperativeHandle的Hook，将传入的ref和useImperativeHandle第二个参数返回的对象绑定到了一起；
  - 所以在父组件中，使用 inputRef.current时，实际上使用的是返回的对象；

```jsx
import React, { useRef, forwardRef, useImperativeHandle } from 'react';

const HYInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }), [inputRef])

  return <input ref={inputRef} type="text"/>
})

export default function UseImperativeHandleHookDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```

### 5.9 useLayoutEffect

- useLayoutEffect看起来和useEffect非常的相似，事实上他们也只有一点区别而已：
  - useEffect会在渲染的内容更新到DOM上后执行，不会阻塞DOM的更新；
  - useLayoutEffect会在渲染的内容更新到DOM上之前执行，会阻塞DOM的更新；

- 如果我们希望在某些操作发生之后再更新DOM，那么应该将这个操作放到useLayoutEffect。

<img src='./img/useEffect 和 useLayoutEffect 的对比.jpg' />

- 案例： useEffect 和 useLayoutEffect 的对比

```jsx
import React, { useState, useEffect } from 'react'

export default function EffectCounterDemo() {
  const [count, setCount] = useState(10);

  useEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200)
    }
  }, [count]);

  return (
    <div>
      <h2>数字: {count}</h2>
      <button onClick={e => setCount(0)}>修改数字</button>
    </div>
  )
}
```

```jsx
import React, { useState, useEffect, useLayoutEffect } from 'react'

export default function LayoutEffectCounterDemo() {
  const [count, setCount] = useState(10);

  useLayoutEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200)
    }
  }, [count]);

  return (
    <div>
      <h2>数字: {count}</h2>
      <button onClick={e => setCount(0)}>修改数字</button>
    </div>
  )
}
```

### 5.10 自定义Hook

- 自定义Hook本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算React的特性。

**实例：**

需求：所有的组件在创建和销毁时都进行打印

- 组件被创建：打印 组件被创建了；

- 组件被销毁：打印 组件被销毁了；

```jsx
import React, { useEffect } from 'react';

const Home = (props) => {
  useLoggingLife("Home");
  return <h2>Home</h2>
}

const Profile = (props) => {
  useLoggingLife("Profile");
  return <h2>Profile</h2>
}

export default function CustomLifeHookDemo01() {
  useLoggingLife("CustomLifeHookDemo01");
  return (
    <div>
      <h2>CustomLifeHookDemo01</h2>
      <Home/>
      <Profile/>
    </div>
  )
}

function useLoggingLife(name) {
  useEffect(() => {
    console.log(`${name}组件被创建出来了`);

    return () => {
      console.log(`${name}组件被销毁掉了`);
    }
  }, []);
}
```

**案例一：** Context的共享

```jsx
//CustomContextShareHook.js
import React from 'react';
import useUserContext from '../hooks/user-hook';

export default function CustomContextShareHook() {
  const [user, token] = useUserContext();
  console.log(user, token);

  return (
    <div>
      <h2>CustomContextShareHook</h2>
    </div>
  )
}

//hooks/user-hook.js
import { useContext } from "react";
import { UserContext, TokenContext } from "../App";

function useUserContext() {
  const user = useContext(UserContext);
  const token = useContext(TokenContext);

  return [user, token];
}

export default useUserContext;
```

**需求二：** 获取鼠标滚动位置

```jsx
//CustomScrollPositionHook.js
import React, { useEffect, useState } from 'react'
import useScrollPosition from '../hooks/scroll-position-hook'

export default function CustomScrollPositionHook() {
  const position = useScrollPosition();

  return (
    <div style={{padding: "1000px 0"}}>
      <h2 style={{position: "fixed", left: 0, top: 0}}>CustomScrollPositionHook: {position}</h2>
    </div>
  )
}


//hooks/scroll-position-hook.js
import { useState, useEffect } from 'react';

function useScrollPosition() {
  const [scrollPosition, setScrollPosition] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScrollPosition(window.scrollY);
    }
    document.addEventListener("scroll", handleScroll);

    return () => {
      document.removeEventListener("scroll", handleScroll)
    }
  }, []);

  return scrollPosition;
}

export default useScrollPosition;
```

**需求三：** localStorage数据存储

```jsx
//CustomDataStoreHook.js
import React, { useState, useEffect } from 'react';
import useLocalStorage from '../hooks/local-store-hook';

export default function CustomDataStoreHook() {
  const [name, setName] = useLocalStorage("name");

  return (
    <div>
      <h2>CustomDataStoreHook: {name}</h2>
      <button onClick={e => setName("kobe")}>设置name</button>
    </div>
  )
}

//hooks/local-store-hook.js
import {useState, useEffect} from 'react';

function useLocalStorage(key) {
  const [name, setName] = useState(() => {
    const name = JSON.parse(window.localStorage.getItem(key));
    return name;
  });

  useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(name));
  }, [name]);

  return [name, setName];
}

export default useLocalStorage;
```

### 5.11 redux hooks

- 在之前的redux开发中，为了让组件和redux结合起来，我们使用了react-redux中的connect：

  - 但是这种方式必须使用高阶函数结合返回的高阶组件；

  - 并且必须编写：mapStateToProps和 mapDispatchToProps映射的函数；

- 在Redux7.1开始，提供了Hook的方式，我们再也不需要编写connect以及对应的映射函数了

- useSelector的作用是将redux中的state映射到组件中：
  - 参数一：将state映射到需要的数据中；
  - 参数二：可以进行比较来决定是否组件重新渲染，可以使用shallowEqual（浅比较）做性能优化

- useSelector默认会比较我们返回的两个对象是否相等（默认深比较）
  - 如何比较呢？ const refEquality = (a, b) => a === b；
  - 也就是我们必须返回两个完全相等的对象才可以不引起重新渲染；

- useDispatch非常简单，就是直接获取dispatch函数，之后在组件中直接使用即可；

- useStore获取当前的store对象；

```jsx
import { shallowEqual, useDispatch, useSelector } from "react-redux";
import { getTopBannerAction } from "../../store/actionCreators";

//redux hooks
//使用shallowEqual浅比较优化性能
//使用useSelector将state映射到组件中
const { topBanners } = useSelector(
    state => ({
        topBanners: state.recommend.topBanners,
    }),
    shallowEqual
);
//使用useDispatch将dispatch映射到组件中
const dispatch = useDispatch();

useEffect(() => {
    dispatch(getTopBannerAction());
}, [dispatch]);
```

## 6. 错误边界（只用于生产环境）

### 6.1 理解：

错误边界(Error boundary)：用来捕获后代组件错误，渲染出备用页面

### 6.2 特点：

只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误

### 6.3 使用：

getDerivedStateFromError配合componentDidCatch

```js
// 生命周期函数，一旦后代组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true,
    };
}

// 生命周期函数
componentDidCatch(error, info) {
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```

**实例**

```jsx
//Child.jsx
import React, { Component } from 'react'
export default class Child extends Component {
	state = {
		users:[
			{id:'001',name:'tom',age:18},
			{id:'002',name:'jack',age:19},
			{id:'003',name:'peiqi',age:20},
		]
		// users:'abc'
	}
	render() {
		return (
			<div>
				<h2>我是Child组件</h2>
				{
					this.state.users.map((userObj)=>{
						return <h4 key={userObj.id}>{userObj.name}----{userObj.age}</h4>
					})
				}
			</div>
		)
	}
}

//Parent.jsx
import React, { Component } from 'react'
import Child from './Child'
export default class Parent extends Component {
	state = {
		hasError:'' //用于标识子组件是否产生错误
	}
	//当Parent的子组件出现报错时候，会触发getDerivedStateFromError调用，并携带错误信息
	static getDerivedStateFromError(error){
		console.log('@@@',error);
		return {hasError:error}
	}
	componentDidCatch(){
		console.log('此处统计错误，反馈给服务器，用于通知编码人员进行bug的解决');
	}
	render() {
		return (
			<div>
				<h2>我是Parent组件</h2>
				{this.state.hasError ? <h2>当前网络不稳定，稍后再试</h2> : <Child/>}
			</div>
		)
	}
}
```

## 7. 组件通信方式总结

### 7.1 组件间的关系：

- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）

### 7.2 几种通信方式：

1.props：

​    (1).常规props属性传参

​    (2).动态传入结构 --- 对应vue的插槽slot

​		1.children props（标签体传参）

​		2.jsx props（属性传参）

​    	3.render props（render函数属性传参，可以携带数据）

2.消息订阅-发布：

​    pubs-sub，events等等

3.集中式管理：

​    redux、dva等等

4.context

5.refs

- 父子组件间通信: 父组件需要子组件数据（方法）（给子组件设置ref，获取子组件数据）

- 父组件得到子组件对象, 从而可以读取其状态数据或调用其方法更新其状态数据

### 7.3 推荐使用的通信方式：

- 父子组件：
  - props（一般属性 / 函数属性（子传父）/ childen props / jsx props / render props）
  - 直接refs（类组件子传父）/ useRef、forwardRef与useImperativeHandle结合（函数组件子传父）
- 兄弟组件：
  - 消息订阅-发布（pubs-sub / events）
  - 集中式管理（redux / dva）
- 祖孙组件(跨级组件)：
  - 消息订阅-发布（pubs-sub / events）
  - 集中式管理（redux / dva）
  - conText（开发用的少，封装插件用的多）

## 8. serve-快速以文件夹开启服务器的库

1.安装

```
npm i serve -g

npm i serve
```

2.使用

```
serve
serve ./a

npx serve
npx serve ./a
```

