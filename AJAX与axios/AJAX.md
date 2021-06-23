# Ajax

## 1.原生Ajax

```js
1.简介
  AJAX 全称为Asynchronous Javascript And XML，就是异步的 JS 和 XML。
  通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。

2.AJAX的特点
  2.1AJAX的优点
  1)可以无需刷新页面而与服务器端进行通信。
  2)允许你根据用户事件来更新部分页面内容。
  2.2 AJAX的缺点
  1)没有浏览历史，不能回退
  2)存在跨域问题
  3)SEO不友好

3.使用原生js发送ajax_get（重点！）
注：以下代码必须记住！！！
    //1.实例化一个XMLHttpRequest对象
    let xhr = new XMLHttpRequest()

    //2.绑定一个名为onreadystatechange事件监听
    xhr.onreadystatechange = function () {
      if(xhr.readyState === 4 && xhr.status === 200){
        //如果进入此判断，意味着：请求成功了，且数据已经回来了
        console.log(xhr.response)
      }
    }

    //3.调用open方法---------用什么方法发？给谁发？带着什么过去？
    xhr.open('get','http://localhost:3000/ajax_get?name=kobe&age=18')

    //4.调用send方法---------发送请求
    xhr.send()

完整实例：
    //使用原生js发送ajax_get
    /*
    * 1.实例化一个XMLHttpRequest对象,名为：xhr---------找来一个帮你发请求的人。
    * 2.绑定一个名为onreadystatechange事件监听------和发请求的人约定好--成功了做什么，失败了做什么。
    * 3.调用open方法---------用什么方法发？给谁发？带着什么过去？
    * 4.调用send方法---------发送请求
    * */

    //1.实例化一个XMLHttpRequest对象
    let xhr = new XMLHttpRequest()

    //2.绑定一个名为onreadystatechange事件监听
    xhr.onreadystatechange = function () {
      //xhr对象本身有5种状态：xhr“诞生”的那一刻就是0状态
      /*
      * 0:xhr对象在实例化出来的那一刻，就已经是0状态，代表着xhr是初始化状态。
      * 1:send方法还没有被调用，即：请求没有发出去，此时依然可以修改请求头。
      * 2:send方法被调用了，即：请求已经发出去了，此时已经不可以再修改请求头。
      * 3:已经回来一部分数据了，如果是比较小的数据，会在此阶段一次性接收完毕,较大数据，有待于进一步接收。
      * 4:数据完美的回来了。
      * */
      
      //我们几乎不会在0状态里做任何事。即：如下判断0状态，永远进不去。
      /*if(xhr.readyState === 0){
        console.log('我是一个刚出生的娃娃')
      }*/
      //1:send方法还没有被调用，即：请求没有发出去，此时依然可以修改请求头。
      /*if(xhr.readyState === 1){
        console.log('我是1状态')
        xhr.setRequestHeader('demo','123')
      }*/
      //2:send方法被调用了，即：请求已经发出去了，此时已经不可以再修改请求头。
      /*if(xhr.readyState === 2){
        console.log('我是2状态')
        xhr.setRequestHeader('demo','123')
      }*/
      //3.已经回来一部分数据了，如果是比较小的数据，会在此阶段一次性接收完毕,较大数据，有待于进一步接收。
      /*if(xhr.readyState === 3){
        console.log(xhr.response)
      }*/
      //4:数据完美的回来了。
      if(xhr.readyState === 4 && xhr.status === 200){
        //如果进入此判断，意味着：请求成功了，且数据已经回来了
        console.log(xhr.response)
        let demo = document.getElementById('demo')
        demo.innerHTML = xhr.response
      }
    }

    //3.调用open方法---------用什么方法发？给谁发？带着什么过去？
    xhr.open('get','http://localhost:3000/ajax_get?name=kobe&age=18&t='+Date.now())

    //4.调用send方法---------发送请求
    xhr.send()

4.使用原生js发送ajax_post（重点！）
   //1.实例化一个XMLHttpRequest对象
   let xhr = new XMLHttpRequest()

    //2.绑定一个名为onreadystatechange事件监听
    xhr.onreadystatechange = function () {
      if(xhr.readyState === 4 && xhr.status === 200){
        console.log(xhr.response)
        let demo = document.getElementById('demo')
        demo.innerHTML = xhr.response
      }
    }

    //3.调用open方法---------用什么方法发？给谁发？带着什么过去？
    xhr.open('post','http://localhost:3000/ajax_post')
    
    //特别注意：此处是设置post请求所特有的请求头，若不设置，服务器无法获取参数
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')

    //4.调用send方法---------发送请求
    xhr.send('name=kobe&age=18')

扩展：
get请求
  1.参数放在哪? --- 请求地址里
  2.以什么方式携带? --- 查询字符串参数
  3.参数是什么编码形式? --- urlencoded
	key=value&key=value

post请求
1.参数放在哪?------请求体里
2.以什么方式携带?----请求体参数
3.参数是什么编码形式?-----urlencoded

form-post -- urlencoded   form自动
ajax-post
  1.--urlencoded : 加了一个特殊的请求头 --- name=kobe&age=18
  2.--json '{"name": "kobe","age": "18"}'

5.解决IE缓存问题
问题：在一些浏览器中(IE),由于缓存机制的存在，ajax只会发送第一次请求，剩余多次请求不会在发送给服务器而是直接加载缓存中的数据。
解决方式：浏览器的缓存是根据url地址来记录的，所以我们只需要修改url地址即可避免缓存问题
xhr.open("get","/testAJAX?t="+Date.now());

6.取消上一次请求(实现类似防抖)
xhr.abort()：
  1.如果来得及，半路取消，请求根本没有到达服务器。
  2.如果来不及，拒之门外，请求已经到达了服务器，且服务器已经给出响应。
  3.也存在根本不起作用的情况。

完整示例：
//服务器代码：
//返回验证码的路由，每当有人请求该路由，该路由就会返回一个1000 - 9999的随机数
app.get('/get_code',function (req,res) {
  console.log('有人找我要验证码了')
  setTimeout(()=>{
    let authCode = Math.floor(Math.random()*8999 + 1000)
    console.log(authCode)
    res.send(authCode.toString())
  },4000)
})

//客户端代码：
let btn = document.getElementById('btn')
let lastXhr

btn.onclick = function () {
    if(lastXhr){
        lastXhr.abort() //用于“取消”本次请求
    }
    lastXhr = getAuthCode()
}

//专门用于请求验证码的函数
function getAuthCode() {
    let xhr = new XMLHttpRequest()
    xhr.onreadystatechange = function () {
        if(xhr.readyState === 4 && xhr.status === 200){
            //如果进入此判断，意味着：请求成功了，且数据已经回来了
            console.log(xhr.response)
        }
    }
    xhr.open('get','http://localhost:3000/get_code')
    xhr.send()
    //xhr.abort() //用于“取消”本次请求
    return xhr
}
```

## 2.XHR的理解和使用

MDN文档

https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest

### 2.1理解

```
1.使用XMLHttpRequest (XHR)对象可以与服务器交互, 也就是发送ajax请求

2.前端可以获取到数据，而无需让整个的页面刷新。

3.这使得Web页面可以只更新页面的局部，而不影响用户的操作。
```

### 2.2区别一般http请求与ajax请求

```
1.ajax请求是一种特别的http请求

2.对服务器端来说, 没有任何区别, 区别在浏览器端

3.浏览器端发请求: 只有XHR或fetch发出的才是ajax请求, 其它所有的都是非ajax请求

4.浏览器端接收到响应

	(1)一般请求: 浏览器一般会直接显示响应体数据, 也就是我们常说的刷新/跳转页面

	(2)ajax请求: 浏览器不会对界面进行任何更新操作, 只是调用监听回调函数并传入响应相关数据
```

### 2.3API

```
1.XMLHttpRequest(): 创建XHR对象的构造函数

2.status: 响应状态码值, 比如：200, 404

3.statusText: 响应状态文本

4.readyState: 标识请求状态的只读属性

	0:  初始

	1:  open()之后

	2:  send()之后

	3:  请求中

	4:  请求完成

5.onreadystatechange: 绑定readyState改变的监听

6.responseType: 指定响应数据类型, 如果是'json', 得到响应后自动解析响应体数据

7.response: 响应体数据, 类型取决于responseType的指定

8.timeout: 指定请求超时时间, 默认为0代表没有限制

9.ontimeout: 绑定超时的监听

10.onerror: 绑定请求网络错误的监听

11.open(): 初始化一个请求, 参数为: (method, url[, async])

12.send(data): 发送请求

13.abort(): 中断请求

14.getResponseHeader(name): 获取指定名称的响应头值

15.getAllResponseHeaders(): 获取所有响应头组成的字符串

16.setRequestHeader(name, value): 设置请求头
```

## 3.jQuery封装的AJAX（会用就行）

```js
let btn1 = $('#btn1')
let btn2 = $('#btn2')
  
btn1.click(function () {

    //使用jQuery发送ajax-get请求------完整版写法 --- 2个参数
    /*$.ajax('http://localhost:3000/ajax_get',{
      method:'get', //发送请求的方式
      data:{name:'kobe',age:41}, //发送请求携带的数据
      //成功的回调
      success:function (result) {
        console.log(result)
      },
      //失败的回调
      error:function (err) {
        console.log(err)
      }
    })*/

    //使用jQuery发送ajax-get请求------简写
    $.get('http://localhost:3000/ajax_get',{name:'kobe',age:41},(data)=>{
        console.log(data)
    })
})

btn2.click(function () {
    //使用jQuery发送ajax-post请求------完整版写法 --- 1个参数
    $.ajax({
        url:'http://localhost:3000/ajax_post',
        method:'post', //发送请求的方式
        data:{name:'kobe',age:41}, //发送请求携带的数据
        //成功的回调
        success:function (result) {
            console.log(result)
        },
        //失败的回调
        error:function (err) {
            console.log(err)
        }
    })

    //使用jQuery发送ajax-post请求------简写
    $.post('http://localhost:3000/ajax_post',{name:'kobe',age:41},(data)=>{
        console.log(data)
    })
})
```

## 3.跨域

### 3.1同源策略与跨域解决

```js
1.为什么会有跨域这个问题？
原因是浏览器为了安全，而采用的同源策略（Same origin policy）  

2.什么是同源策略？
1. 同源策略是由Netscape提出的一个著名的安全策略，现在所有支持JavaScript 的浏览器都会使用这个策略。
2. Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。
3. 所谓同源是指：协议，域名（IP），端口必须要完全相同
   即：协议、域名（IP）、端口都相同，才能算是在同一个域里。
19:53 2021/4/20
3.没有同源策略的危险场景：（了解）
危险场景：
> 有一天你刚睡醒，收到一封邮件，说是你的银行账号有风险，赶紧点进www.yinghang.com改密码。
你着急的赶紧点进去，还是熟悉的银行登录界面，你果断输入你的账号密码，登录进去看看钱有没有少了，
睡眼朦胧的你没看清楚，平时访问的银行网站是www.yinhang.com，而现在访问的是www.yinghang.com，
随后你来了一条短信，钱没了。

4.非同源受到哪些限制？（跨域会存在哪些问题？）（重点！！！）
1.Cookie不能读取；
2.DOM无法获得；
3.Ajax请求不能获取数据
（AJAX请求能发送，但不能获取数据，对服务器无影响，对浏览器有影响）
扩展：
form跨域----不拦截
ajax跨域---拦截

ajax发的请求----ajax引擎-----同源策略
form发请求----浏览器其他模块---不受同源策略的限制

面试题：
为什么form表单不受同源策略的限制，而ajax受同源策略的限制？
同源策略是限制浏览器跨域获取数据，引起安全问题，但浏览器不阻止你像其他域名发出请求。那么form发送请求能够跨域就可以理解，他只是发出请求，并没有得到响应。

5.如何在开发中解决跨域问题：（重点！）
1)JSONP解决发送请求跨域问题：
  1.原理：利用了script标签发请求不受同源策略的限制。所以不会产生跨域问题
  2.套路：动态构建script节点，利用节点的src属性，发出get请求，从而绕开ajax引擎
  3.弊端：(1).只能解决get请求跨域的问题。(2).后端工程师必须配合前端


-原生js完整示例：
前端：
<script type="text/javascript">
  let btn = document.getElementById('btn')
  btn.onclick = function () {
    //1.提前定义好一个等待被调用的函数
    window.peiqi = function(result) {
      console.log(result)
    }
    //2.创建一个script节点
    const scriptNode = document.createElement('script')
    //3.为节点指定src地址，同时指定好回调函数的名字
    scriptNode.src = 'http://localhost:3000/test?callback=peiqi'
    //4.将节点插入页面
    document.body.appendChild(scriptNode)
  }

后端：
app.get('/test',function (req,res) {
  let {callback} = req.query
  console.log(callback)
  let personArr = [{name:'peiqi',age:12},{name:'suxi',age:16}]
  res.send(`${callback}(${JSON.stringify(personArr)})`)
})

-jQuery完整示例：
   1.提前定义好一个等待被调用的函数
   2.创建一个script节点
   3.为节点指定src地址，同时指定好回调函数的名字
   4.将节点插入页面
   备注：以上所有步骤，都由jquery底层全部完成

$('#btn').click(function () {
    //完整写法
    $.ajax({
      url:'http://localhost:3000/test',
      method:'get',
      dataType:'jsonp', //该属性，控制了上面的4步
      data:{name:'zhangsan',age:18},
      success:function (result) {
        console.log(result)
      },
      error:function (err) {
        console.log(err)
      }
    })
    
    //精简写法
    /*$.getJSON('http://localhost:3000/test?callback=?',{name:'zhangsan',age:18},function (data) {
      console.log(data)
    })*/
  })

2)后台配置cors解决跨域
CORS是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持get和post请求。

CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

主要是服务器端的设置：( node )
app.get('/test',function (req,res) {
  let personArr = [{name:'peiqi',age:12},{name:'suxi',age:16}]
  res.setHeader('Access-Control-Allow-Origin','http://localhost:63348')
  //res.setHeader('Access-Control-Allow-Origin','*')
  res.send(personArr)
})

3)使用代理服务器
例如：nginx等
```

## 4.封装自己的Ajax - 仿jQuery

```js
function sendAjax(option) {
    //获取配置对象里的属性
    let {url,method,data,success,error} = option
    method = method ? method : 'get' //设置method的默认值

    //实例化xhr
    let xhr = new XMLHttpRequest()
    //绑定监听
    xhr.onreadystatechange = function () {
        if(xhr.readyState !== 4) return
        if(xhr.status >= 200 && xhr.status <= 299){
            if(success) success(xhr.response)
        }else{
            if(error) error('请求出错')
        }
    }

    //整理参数
    let str = ''
    for (let key in data){
        str += `${key}=${data[key]}&`
    }

    //根据请求方式决定如何携带参数
    if(method.toUpperCase() === 'GET'){
        xhr.open(method,url+'?'+str)
        xhr.send()
    }else{
        xhr.open(method,url)
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
        //post要将参数的urlencoded传入send中
        xhr.send(str)
    }
    return xhr
}


//测试自己封装的ajax
sendAjax({
    url:'http://localhost:3000/ajax_post',
    method:'post',
    data:{name:'kobe',age:result},
    success:function (result) {
        console.log(result)
    },
    error:function (err) {
        console.log(err)
    }
})
```

## 5.Fetch（关注分离的设计思想）

**1.Fetch简介**

Fetch是AJAX的替换方案，基于Promise设计，很好的进行了关注分离，有很大一批人喜欢使用fetch进行项目开发；

但是Fetch的缺点也很明显，首先需要明确的是Fetch是一个 low-level（底层）的API，没有帮助你封装好各种各样的功能和实现；

比如发送网络请求需要自己来配置Header的Content-Type，不会默认携带cookie等；

比如错误处理相对麻烦（只有网络错误才会reject，HTTP状态码404或者500不会被标记为reject）；

比如不支持取消一个请求，不能查看一个请求的进度等等；

MDN Fetch学习地址：https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

**2.fetch发送请求步骤**

1) 先判断是否成功连接服务器

2) 再判断数据是否获取到

**3.fetch发送网络请求**

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



