# Vue3

## 一、Vue3入门

### 1.vue3出现

- 在2020年的9月19日，万众期待的Vue3终于发布了正式版，命名为“One Piece”。
- 它也带来了很多新的特性：更好的性能、更小的包体积、更好的TypeScript集成、更优秀的API设计。

### 2.vue3变化

**1）源码**

- 源码通过monorepo的形式来管理源代码：
  - Mono：单个
  - Repo：repository仓库
  - 主要是将许多项目的代码存储在同一个repository中；
  - 这样做的目的是多个包本身相互独立，可以有自己的功能逻辑、单元测试等，同时又在同一个仓库下方便管理；
  - 而且模块划分的更加清晰，可维护性、可扩展性更强；

- 源码使用TypeScript来进行重写：
  - 在Vue2.x的时候，Vue使用Flow来进行类型检测；
  - 在Vue3.x的时候，Vue的源码全部使用TypeScript来进行重构，并且Vue本身对TypeScript支持也更好了；

**2）性能**

- 使用Proxy进行数据劫持
  - 在Vue2.x的时候，Vue2是使用Object.defineProperty来劫持数据的getter和setter方法的；
  - 这种方式一致存在一个缺陷就是当给对象添加或者删除属性时，是无法劫持和监听的；
  - 所以在Vue2.x的时候，不得不提供一些特殊的API，比如`$set`或`$delete`，事实上都是一些hack方法，也增加了开发者学习新的API的成本；
  - 而在Vue3.x开始，Vue使用Proxy来实现数据的劫持；

- 删除了一些不必要的API：
  - 移除了实例上的 `$on`, `$off` 和 `$once`；
  - 移除了一些特性：如filter、内联模板等；
- 包括编译方面的优化：
  - 生成Block Tree、Slot编译优化、diff算法优化；

**3）API**

- 由Options API 到 Composition API：
  - 在Vue2.x的时候，我们会通过Options API来描述组件对象；
  - Options API包括data、props、methods、computed、生命周期等等这些选项；
  - 存在比较大的问题是多个逻辑可能是在不同的地方：
    - 比如created中会使用某一个method来修改data的数据，代码的内聚性非常差；
  - Composition API可以将 相关联的代码 放到同一处 进行处理，而不需要在多个Options之间寻找；
- Hooks函数增加代码的复用性：
  - 在Vue2.x的时候，我们通常通过mixins在多个组件之间共享逻辑；
  - 但是有一个很大的缺陷就是 mixins 也是由一大堆的Options组成的，并且多个mixins会存在命名冲突的问题；
  - 在Vue3.x中，我们可以通过Hook函数，来将一部分独立的逻辑抽取出去，并且它们还可以做到是响应式的；

### 3.如何使用Vue？

- 方式一：在页面中通过CDN的方式来引入；

```html
<div id="app"></div>
<script src="https://unpkg.com/vue@3"></script>
<script>
    // vue相关代码
    const why = {
        template: '<h2>Hello World</h2>'
    }
    const app = Vue.createApp(why);
    // 将vue实例挂载到id为app的div上
    app.mount("#app")
</script>
```

- 方式二：下载Vue的JavaScript文件，并且自己手动引入；
  - 下载Vue的源码，可以直接打开CDN的链接：
    - 打开链接，复制其中所有的代码；
    - 创建一个新的文件，比如vue.js，将代码复制到其中；

```html
<div id="app"></div>

<script src="../js/vue.js"></script>
<script>
    // vue实例可链式调用
    Vue.createApp({
        template: `<h2>你好啊, 小辰</h2>`
    }).mount("#app");
</script>
```

- 方式三：通过npm包管理工具安装使用它；
- 方式四：直接通过Vue CLI创建项目，并且使用它；

### 4.计数器案例：

```html
<div id="app">哈哈哈哈啊</div>

<script src="../js/vue.js"></script>
<script>
    // vue底层会清空绑定的div里的内容，等价于appdom.innerHTML=""的操作
    Vue.createApp({
      template: `
        <div>
          <h2>{{message}}</h2>
          <h2>{{counter}}</h2>
          <button @click='increment'>+1</button>
          <button @click='decrement'>-1</button>
        </div>
      `,
      data: function() {
        return {
          message: "Hello World",
          counter: 100
        }
      },
      // 定义各种各样的方法
      methods: {
        increment() {
          console.log("点击了+1");
          this.counter++;
        },
        decrement() {
          console.log("点击了-1");
          this.counter--;
        }
      }
    }).mount('#app');
</script>
```

### 5.template属性

**1）概念**

template表示的是Vue需要帮助我们渲染的模板信息：

- 目前我们看到它里面 **有很多的HTML标签** ，这些标签会 **替换掉** 我们挂载到的元素（比如id为app的div）的innerHTML；
- 模板中有一些语法，比如 {{}}，比如 @click，这些都是 **模板特有的语法**；

**2）写法**

- 使用script标签，并且标记它的类型为 x-template；

```html
<div id="app"></div>

<script type="x-template" id="why">
    <div>
      <h2>{{message}}</h2>
      <h2>{{counter}}</h2>
      <button @click='increment'>+1</button>
      <button @click='decrement'>-1</button>
    </div>
</script>

<script src="../js/vue.js"></script>
<script>
    Vue.createApp({
        // vue底层做了document.querySelector("#why")
        template: '#why',
        data: function() {
            return {
                message: "Hello World",
                counter: 100
            }
        },
        // 定义各种各样的方法
        methods: {
            increment() {
                console.log("点击了+1");
                this.counter++;
            },
            decrement() {
                console.log("点击了-1");
                this.counter--;
            }
        }
    }).mount('#app');
</script>
```

- 使用任意标签（通常使用template标签，因为不会被浏览器渲染），设置id；
  - template元素是一种用于保存客户端内容的机制，该内容再加载页面时不会被呈现，但随后可以在运行时使用JavaScript实例化；

**注：** vue3中 template元素 内可以不用 div 作为根标签来包括内容！

```html
<div id="app"></div>

<template id="why">
    <div>
        <h2>{{message}}</h2>
        <h2>{{counter}}</h2>
        <button @click='increment'>+1</button>
        <button @click='decrement'>-1</button>
        <button @click="btnClick">按钮</button>
    </div>
</template>

<script src="../js/vue.js"></script>
<script>
    document.querySelector("#why")
    Vue.createApp({
        template: '#why',
        data: function() {
            return {
                message: "Hello World",
                counter: 100
            }
        },
        // 定义各种各样的方法
        methods: {
            increment() {
                console.log("点击了+1");
                this.counter++;
            },
            decrement() {
                console.log("点击了-1");
                this.counter--;
            },
            btnClick: () => {
                // 此时this === window  true
                // 不可以使用箭头函数，写成一个箭头函数时, 这个this就是window
                // 在箭头函数中是不绑定this, 但是函数中如果使用了this
                console.log(this); //window
            }
        }
    }).mount('#app');
</script>
```

**注：** 这个时候，在createApp的对象中，我们需要传入的template以 # 开头：

- 如果字符串是以 # 开始，那么它将被用作 querySelector，并且使用匹配元素的 innerHTML 作为模板字符串；

### 6.data属性

1）data属性是传入一个函数，并且该函数需要返回一个对象：

- 在 **Vue2.x** 的时候，也可以传入一个 **对象** （虽然官方推荐是一个函数）；

- 在 **Vue3.x** 的时候，必须传入一个 **函数** ，否则就会直接在浏览器中报错；

2）data中返回的对象会被 **Vue的响应式系统劫持** ，之后对该 **对象的修改或者访问** 都会在劫持中被处理：

- 所以我们在template中通过 {{counter}} 访问counter，可以从对象中获取到数据；

- 所以我们修改counter的值时，template中的 {{counter}}也会发生改变；

### 7.methods属性

1）methods属性是一个对象，通常我们会在这个对象中定义很多的方法：

- 这些方法可以 **被绑定到 template 模板** 中；

- 在该方法中，我们可以 **使用this关键字** 来直接访问到data中返回的对象的属性；

### 8.MVC与MVVM模型

- MVC是Model – View –Controller的简称，是在前期被使用非常框架的架构模式，比如iOS、后端；
- MVVM是Model-View-ViewModel的简称，是目前非常流行的架构模式；
- 通常情况下，我们也经常称Vue是一个MVVM的框架。
  - Vue官方其实有说明，Vue虽然并没有完全遵守MVVM的模型，但是整个设计是受到它的启发的。

<img src="img\MVVM模型.jpg" style="zoom: 80%;" />

### 9.面试题1：为什么不能使用箭头函数？

我们在methods中要使用data返回对象中的数据：

- 那么这个 **this是必须有值的** ，并且应该可以 **通过this获取到data返回对象中的数据** 。

- **this不可以是window** ，因为window中我们无法获取到data返回对象中的数据；
- 但是如果我们 **使用箭头函数** ，那么这个 **this就会是window** 了；

### 10.面试题2：this到底指向什么？

<img src="img\vue源码method中this指向.png" />

### 11.配置VSCode代码片段

VSCode中的代码片段有固定的格式，所以我们一般会借助于一个在线工具来完成。

具体的步骤如下：

- 第一步，复制自己需要生成代码片段的代码；
- 第二步，https://snippet-generator.app/在该网站中生成代码片段；
- 第三步，在 VSCode 设置中的 用户代码片段 选项里根据 文件类型 配置代码片段；

## 二、基础指令与Options API

### 1.开发模式对比

- **React的开发模式：**
  - React使用的 **jsx** ，所以对应的代码都是编写的 **类似于js的一种语法** ；
  - 之后通过 **Babel** 将 **jsx** 编译成 **React.createElement** 函数调用；
- **Vue的开发模式：**
  - 虽然也支持jsx的开发模式，但是大多数情况下，使用 **基于HTML的模板语法** ；
  - 在模板中，允许开发者以声明式的方式将 **DOM** 和 **底层组件实例的数据** 绑定在一起；
  - 在底层的实现中，Vue将模板编译成虚拟DOM渲染函数

### 2.Mustache双大括号语法

1）如果我们希望把数据显示到模板（template）中，使用最多的语法是 **“Mustache”语法 (双大括号)** 的 **文本插值** 。

- **data返回的对象** 是有添加到 **Vue的响应式系统** 中；
- 当 **data中的数据发生改变** 时， **对应的内容也会发生更新** 。
- 当然，Mustache中不仅仅可以是data中的属性，也可以是一个 **JavaScript的表达式** 。

**2）mustache的基本使用**

```html
<template id="my-app">
    <!-- 1.基础语法 -->
    <h2>{{message}} - {{message}}</h2>
    <!-- 2.是一个表达式 -->
    <h2>{{counter * 10}}</h2>
    <h2>{{ message.split(" ").reverse().join(" ") }}</h2>
    <!-- 3.调用methods中的函数 或 使用computed(计算属性) -->
    <h2>{{getReverseMessage()}}</h2>
    <!-- 4.三元运算符 -->
    <h2>{{ isShow ? "哈哈哈": "" }}</h2>
    <button @click="toggle">切换</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                counter: 100,
                isShow: true
            }
        },
        methods: {
            getReverseMessage() {
                return this.message.split(" ").reverse().join(" ");
            },
            toggle() {
                this.isShow = !this.isShow;
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**3）错误用法**

- 不能使用 赋值语句 和 if语句

```html
<!-- 这是赋值语句，不是表达式 -->
<h2>{{var name = "abc"}}</h2>
<!-- if语句不支持，但可以使用三元运算符 -->
<h2>{{ if(isShow) {  return "哈哈哈" } }}</h2>
```

### 3.不常用指令

#### 3.1 v-once

1）v-once用于指定 元素 或者 组件 只渲染一次：

- 当数据发生变化时， **元素或者组件以及其所有的子元素** 将视为 **静态内容并且跳过** ；
- 该指令可以用于 **性能优化** ；

```html
<h2 v-once>当前计数：{{counter}}</h2>
<button @click="increment">+1</button>
```

2）如果当前元素有子节点，也是只会渲染一次；

```html
<div v-once>
    <h2>当前计数：{{counter}}</h2>
    <button @click="increment">+1</button>
</div>
```

#### 3.2 v-text

v-text用于更新元素的 textContent

```html
<h2 v-text="message"></h2>
<!-- 等价于 -->
<h2>{{message}}</h2>
```

#### 3.3 v-html

默认情况下，如果我们展示的 **内容本身是 html** 的，那么 **vue并不会对其进行特殊的解析** 。

- 如果我们希望这个内容 **被Vue可以解析出来** ，那么可以 **使用v-html** 来展示；

```html
<template id="my-app">
    <div>{{msg}}</div>
    <div v-html="msg"></div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                msg: '<span style="color:red; background: blue;">哈哈哈</span>'
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 3.4 v-pre

v-pre用于跳过元素和它的子元素的编译过程，显示原始的Mustache标签：

- 跳过不需要编译的节点，加快编译的速度；

![](img\v-pre.jpg)

#### 3.5 v-cloak

这个指令保持在元素上直到关联组件实例结束编译。

- 和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到组件实例准备完毕

![](img\v-cloak.jpg)

- `<div>` 不会显示，直到编译结束。

### 4.v-bind

1）除了内容需要动态来决定外，某些 **属性** 我们也希望动态来绑定。

- 比如动态绑定a元素的href属性；
- 比如动态绑定img元素的src属性；

2）v-bind使用规则：

- **用法：** 动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

#### 3）绑定基本属性

- v-bind用于 **绑定一个或多个属性值** ，或者 **向另一个组件传递props值**

- 在开发中，有哪些属性需要动态进行绑定呢？
  - 还是有很多的，比如图片的链接src、网站的链接href、动态绑定一些类、样式等等

- v-bind有一个对应的语法糖，也就是 **使用`:`的简写方式** 。在开发中，我们通常会使用 **语法糖** 的形式，因
  为这样更加简洁。

```html
<!-- vue2 template模板中只能有一个根元素! -->
<!-- vue3 是允许template中有多个根元素! -->
<template id="my-app">
    <!-- 1.v-bind的基本使用 -->
    <img v-bind:src="imgUrl" alt="">
    <a v-bind:href="link">百度一下</a>
    <!-- 2.v-bind提供一个语法糖 : -->
    <img :src="imgUrl" alt="">
    <!-- 注意与上面的区别 -->
    <img src="imgUrl" alt="">
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                imgUrl: "https://avatars.githubusercontent.com/u/10335230?s=60&v=4",
                link: "https://www.baidu.com"
            }
        }
    }

    Vue.createApp(App).mount('#app');
</script>
```

#### 4）绑定class

- 在开发中，有时候我们的元素class也是动态的，比如：
  - 当数据为 **某个状态** 时，字体显示红色。
  - 当数据 **另一个状态** 时，字体显示黑色。

- **绑定class有两种方式：**
  - 对象语法
  
    - 数组语法

方式一：**对象语法：** 我们可以传给 :class (v-bind:class 的简写) 一个对象，以动态地切换 class。

```html
<template id="my-app">
    <!-- 1.普通绑定方式 -->
    <div :class="className">哈哈哈哈</div>
    <!-- 2.对象绑定: {类(变量)(此处引号可不写): boolean(true/false)} -->
    <div :class="{nba: true, 'james': true}">呵呵呵呵</div>
    <!-- 3.默认的class和动态的class结合 -->
    <div class="why" :class="{active: isActive, title: true}">呵呵呵呵</div>
    <button @click="toggle">切换</button>
    <!-- 4.绑定对象: 将对象放到一个单独的属性中 -->
    <div class="abc cba" :class="classObj">呵呵呵呵</div>
    <!-- 5.将返回的对象放到一个methods(computed)方法中 -->
    <div class="abc cba" :class="getClassObj()">呵呵呵呵</div>      
</template>
<script>
    const App = {
        template: "#my-app",
        data() {
            return {
                className: "why",
                isActive: true,
                title: "abc",
                classObj: { 
                    active: true, 
                    title: true 
                },
            };
        },
        methods: {
            toggle() {
                this.isActive = !this.isActive;
            },
            getClassObj() {
                return { 
                    active: true, 
                    title: true 
                }
            }
        },
    };
    Vue.createApp(App).mount("#app");
</script>
```

方式二：**数组语法：** 我们可以把一个数组传给 :class，以应用一个 class 列表；

```html
<template id="my-app">
    <!-- 1.直接传入一个数组 -->
    <div :class="['abc', title]">哈哈哈</div>
    <!-- 2.数组中可以使用三元运算符 或 绑定变量 -->
    <div :class="['abc', title, isActive ? 'active': '']">呵呵呵</div>
    <!-- 2.数组中也可以使用对象语法 --> 
    <div :class="['abc', title, {active: isActive}]">嘻嘻嘻</div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                title: "cba",
                isActive: true
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 5）绑定style

- 利用 **v-bind:style** 来绑定一些 **CSS内联样式** ：
  - 这次因为某些样式我们需要根据 **数据** 动态来决定；
  - 比如某段文字的 **颜色，大小** 等等；

- CSS property 名可以用 **驼峰式 (camelCase)** 或 **短横线分隔 (kebab-case，记得用引号括起来)** 来命名；
- **绑定class有两种方式：**
  - 对象语法
  - 数组语法

方式一：**对象语法：**

```html
<template id="my-app">
    <!-- 1.基本使用: 传入一个对象，并且对象内容是确定的 -->
    <div :style="{color: 'red', 'font-size': '30px'}">哈哈哈哈</div>
    <!-- 2.变量数据: 传入一个对象，值会来源于data -->
    <div :style="{color: finalColor, fontSize: '30px'}">哈哈哈哈</div>
    <div :style="{color: finalColor, fontSize: finalFontSize + 'px'}">哈哈哈哈</div>
    <!-- 3.对象数据: 绑定data中的定义好的对象 -->
    <div :style="finalStyleObj">呵呵呵呵</div>
    <!-- 4.将返回的对象放到一个methods(computed)方法中 -->
    <div :style="getFinalStyleObj()">呵呵呵呵</div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                finalColor: 'red',
                finalFontSize: 50,
                finalStyleObj: {
                    'font-size': '50px',
                    fontWeight: 700,
                    backgroundColor: 'red'
                }
            }
        },
        methods: {
            getFinalStyleObj() {
                return {
                    'font-size': '50px',
                    fontWeight: 700,
                    backgroundColor: 'red'
                }
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

方式二：**数组语法：** :style 的数组语法可以将多个样式对象应用到同一个元素上；

```html
<template id="my-app">
    <div :style="[style1Obj, style2Obj]">{{message}}</div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                style1Obj: {
                    color: 'red',
                    fontSize: '30px'
                },
                style2Obj: {
                    textDecoration: "underline"
                }
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 6）动态绑定属性

- 在某些情况下，我们 **属性的名称** 可能也 **不是固定的** ：
  - 一般绑定的src、href、class、style等属性名称都是固定的；
  - 如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义；

- 这种绑定的方式，我们称之为 **动态绑定属性** 。

```html
<template id="my-app">
    <div :[name]="value">哈哈哈</div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                name: "cba",
                value: "kobe"
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 7）绑定对象的属性

- 将一个 **对象的所有属性** ，绑定到 **元素上的所有属性** 。则可以直接使用 **v-bind 绑定一个 对象** 。

```html
<template id="my-app">
    <div v-bind="info">哈哈哈哈</div>
    <div :="info">哈哈哈哈</div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                info: {
                    name: "why",
                    age: 18,
                    height: 1.88
                }
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

### 5.v-on

- 使用v-on指令监听用户发生的事件，比如点击、拖拽、键盘事件等等。
- 缩写：@
- 语法：v-on:监听的事件="methods中方法"

#### 1）v-on基本使用

```html
<template id="my-app">
    <!-- 完整写法: v-on:监听的事件="methods中方法" -->
    <button v-on:click="btn1Click">按钮1</button>
    <div class="area" v-on:mousemove="mouseMove">div</div>
    <!-- 语法糖(简写方式) -->
    <button @click="btn1Click">按钮1</button>
    <!-- 绑定一个表达式: inline statement -->
    <button @click="counter++">{{counter}}</button>
    <!-- 元素绑定多个事件，则传入一个对象 -->
    <div class="area" v-on="{click: btn1Click, mousemove: mouseMove}"></div>
    <div class="area" @="{click: btn1Click, mousemove: mouseMove}"></div>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                counter: 100
            }
        },
        methods: {
            btn1Click() {
                console.log("按钮1发生了点击");
            },
            mouseMove() {
                console.log("鼠标移动");
            }
        }
    }

    Vue.createApp(App).mount('#app');
</script>
```

#### 2）v-on参数传递

- 当通过methods中定义方法，以供@click调用时，需要注意参数问题：
  - 情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。
    - 但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去。
  - 情况二：如果需要同时传入某个参数，同时需要event时，可以通过$event传入事件。

```html
<template id="my-app">
    <!-- 默认传入event对象, 可以在方法中获取 -->
    <button @click="btn1Click">按钮1</button>
    <!-- $event可以获取到事件发生时的事件对象 -->
    <button @click="btn2Click($event, 'coderwhy', 18)">按钮2</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World"
            }
        },
        methods: {
            btn1Click(event) {
                console.log(event);
            },
            btn2Click(event, name, age) {
                console.log(name, age, event);
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 3）v-on修饰符

- v-on支持修饰符，修饰符相当于对事件进行了一些特殊的处理：
  - .stop - 调用 event.stopPropagation()。
  - .prevent - 调用 event.preventDefault()。
  - .capture - 添加事件侦听器时使用 capture 模式。
  - .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  - .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
  - .once - 只触发一次回调。
  - .left - 只当点击鼠标左键时触发。
  - .right - 只当点击鼠标右键时触发。
  - .middle - 只当点击鼠标中键时触发。
  - .passive - { passive: true } 模式添加侦听器

```html
<template id="my-app">
    <div @click="divClick">
        <button @click.stop="btnClick">按钮</button>
    </div>
    <input type="text" @keyup.enter="enterKeyup">
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World"
            }
        },
        methods: {
            divClick() {
                console.log("divClick");
            },
            btnClick() {
                console.log('btnClick');
            },
            enterKeyup(event) {
                console.log("keyup", event.target.value);
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

### 6.条件渲染

- 在某些情况下，我们需要根据当前的条件决定某些元素或组件是否渲染，这个时候我们就需要进行条件判断了。

- Vue提供了下面的指令来进行条件判断：
  - v-if
  - v-else
  - v-else-if
  - v-show

#### 6.1 v-if、v-else、v-else-if

v-if、v-else、v-else-if 用于根据条件来渲染某一块的内容：

- 这些内容只有在条件为true时，才会被渲染出来；
- 这三个指令与JavaScript的条件语句 if、else、else if 类似；

##### 1.应用：

**1）基本使用**

```html
<template id="my-app">
    <h2 v-if="isShow">哈哈哈哈</h2>
    <button @click="toggle">切换</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                message: "Hello World",
                isShow: true
            }
        },
        methods: {
            toggle() {
                this.isShow = !this.isShow;
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**2）多分支条件渲染**

```html
<template id="my-app">
    <!-- 此处v-model简单使用，具体可看后面的讲解 -->
    <input type="text" v-model="score">
    <h2 v-if="score > 90">优秀</h2>
    <h2 v-else-if="score > 60">良好</h2>
    <h2 v-else>不及格</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                score: 95
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

##### 2.v-if的渲染原理：

- v-if是惰性的；

- 当条件为false时，其判断的内容完全不会被渲染或者会被销毁掉；

- 当条件为true时，才会真正渲染条件块中的内容；

##### 3.template 和 v-if 结合使用

- 因为 v-if 是一个指令，所以必须将其添加到一个元素上，但是如果我们希望 **切换** 的是 **多个元素** 呢？
  - 此时我们 **渲染div** ，但是我们 **并不希望div这种元素被渲染** ；
  - 这个时候，我们可以选择使用 **template** ；
- template元素可以当做不可见的包裹元素，并且在v-if上使用，但是最终template不会被渲染出来：
  - 有点类似于小程序中的block

```html
<template id="my-app">
    <template v-if="isShowHa">
        <h2>哈哈哈哈</h2>
        <h2>哈哈哈哈</h2>
        <h2>哈哈哈哈</h2>
    </template>

    <template v-else>
        <h2>呵呵呵呵</h2>
        <h2>呵呵呵呵</h2>
        <h2>呵呵呵呵</h2>
    </template>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                isShowHa: true
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 6.2 v-show

v-show 和 v-if 的用法看起来是一致的，也是根据一个条件 **决定是否显示元素或者组件** 。

```html
<template id="my-app">
    <h2 v-show="isShow">哈哈哈哈</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                isShow: true
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 6.3 v-show 和 v-if 的区别（重点！）

**1）用法上的区别：**

- v-show **不支持** 和 **template** 结合使用；
- v-show **不可以** 和 **v-else** 一起使用；

**2）本质的区别：**

- v-show 元素无论是否需要显示到浏览器上，它的 **DOM** 实际都是 **有渲染的** ，只是通过 **CSS的display属性** 来进行 **切换** ；
- v-if 当条件为 **false** 时，其对应的元素压根 **不会被渲染到DOM中** ；

**3）开发中如何进行选择呢？**

- 如果我们的元素需要在显示和隐藏之间 **频繁的切换** ，那么使用 **v-show** ；
- 如果 **不会频繁的发生切换** ，那么使用 **v-if** ；

```html
<!-- 调试观察两者DOM变化 -->
<template id="my-app">
    <h2 v-if="isShow">哈哈哈哈</h2>
    <h2 v-show="isShow">呵呵呵呵</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                isShow: true
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

### 7.v-for（列表渲染）

在真实开发中，我们往往会从服务器拿到一组数据，并且需要对其进行渲染。

- 这个时候我们可以使用 **v-for** 来完成；
- **v-for** 类似于JavaScript的 **for循环** ，可以用于遍历一组数据；

#### 7.1 v-for基本使用

v-for的基本格式： **"item in 数组"**：

- 数组通常是来自 **data或者prop** ，也可以是其他方式；
- item是我们给 **每项元素** 起的一个 **别名** ，这个别名可以自定来定义；

我们知道，在遍历一个数组的时候会经常需要拿到 **数组的索引** ：

- 如果我们需要索引，可以使用格式： **"(item, index) in 数组"** ；
- **注意上面的顺序：数组元素项 item 是在前面的，索引项 index 是在后面的** ；

```html
<template id="my-app">
    <h2>电影列表</h2>
    <ul>
        <!-- 遍历数组 -->
        <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                movies: [
                    "星际穿越",
                    "盗梦空间",
                    "大话西游",
                    "教父",
                    "少年派"
                ]
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**注：** 也可以用 `of` 替代 `in` 作为分隔符，因为它更接近 JavaScript 迭代器的语法：

```html
<div v-for="item of items"></div>
```

#### 7.2 v-for支持类型

**1）遍历对象**

v-for 也支持遍历对象，并且支持有一二三个参数：

- 一个参数： "value in object";
- 二个参数： "(value, key) in object";
- 三个参数： "(value, key, index) in object";

**2）遍历数字**

v-for 同时也支持数字的遍历，结果为从1到当前数字：

- 每一个item都是一个数字；

```html
<template id="my-app">
    <h2>个人信息</h2>
    <ul>
        <!-- 遍历对象 -->
        <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
    </ul>
    <h2>遍历数字</h2>
    <ul>
        <li v-for="(num, index) in 10">{{num}}-{{index}}</li>
    </ul>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                info: {
                    name: "why",
                    age: 18,
                    height: 1.88
                }
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 7.3 v-for 和 template 结合使用

类似于 v-if ，你可以使用 template 元素来循环渲染一段 **包含多个元素的内容** ：

- **使用 template** 来对多个元素进行包裹，而 **不是使用 div** 来完成；

```html
<template id="my-app">
    <ul>
        <template v-for="(value, key) in info">
            <li>{{key}}</li>
            <li>{{value}}</li>
            <li class="divider"></li>
        </template>
    </ul>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                info: {
                    name: "why",
                    age: 18,
                    height: 1.88
                }
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 7.4 数组更新检测

Vue 将 **被侦听的数组** 的 **修改原数组的方法** 进行了包裹，所以它们也将会 **触发视图更新** 。

这些被包裹过的方法包括：

- push()、pop()、shift()、unshift()、splice()、sort()、reverse()

**生成新数组的方法：**

- 上面的方法会直接修改原来的数组，但是某些方法不会修改原来的数组，而是会生成新的数组，比如 filter()、concat()、slice() 等。**这时需要将新数组赋值给被侦听的老数组来触发视图更新。**

```html
<template id="my-app">
    <h2>电影列表</h2>
    <ul>
        <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <input type="text" v-model="newMovie">
    <button @click="addMovie">添加电影</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                newMovie: "",
                movies: [
                    "星际穿越",
                    "盗梦空间",
                    "大话西游",
                    "教父",
                    "少年派"
                ]
            }
        },
        methods: {
            addMovie() {
                //修改原数组的方法
                this.movies.push(this.newMovie);
                this.newMovie = "";

                // 使用生成新数组的方法需要将新数组赋值给被侦听的老数组来触发视图更新。
                // this.movies = this.movies.filter(item => item.length > 2);
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 7.5 v-for中的key是什么作用？（重点！）

在使用 v-for 进行列表渲染时，通常会给元素或者组件绑定一个 **key属性** 。

这个key属性有什么作用呢？官方的解释：

- key属性主要用在Vue的 **虚拟DOM的diff算法** ，在 **新旧Nodes** 对比时辨识 **VNodes** ；
- 如果 **不使用key** ，Vue会使用一种 最大限度减少动态元素 并且 尽可能的尝试就地 **修改/复用相同类型元素** 的算法；
- 而 **使用key** 时，它会基于key的变化 **重新排列元素顺序**，并且会 **移除/销毁key不存在的元素** ；

#### 7.6 认识VNode

- VNode的全称是 Virtual Node，也就是 **虚拟节点**；

- 事实上，无论是 **组件** 还是 **元素** ，它们最终在Vue中表示出来的都是一个个 **VNode** ；

- VNode的本质是一个 **JavaScript的对象**；

<img src="img\VNode.jpg" />

#### 7.7 虚拟DOM

如果我们不只是一个简单的div，而是有 **一大堆的元素** ，那么它们应该会形成一个 **VNode Tree** ：

<img src="img\虚拟DOM.jpg" />

#### 7.8 diff算法分析案例：插入f

点击按钮时会在 ul 中间插入一个新的值为 f 的 li。

```html
<template id="my-app">
    <ul>
        <li v-for="item in letters" :key="item">{{item}}</li>
    </ul>
    <button @click="insertF">插入F元素</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                letters: ['a', 'b', 'c', 'd']
            }
        },
        methods: {
            insertF() {
                this.letters.splice(2, 0, 'f')
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

我们可以确定的是，这次更新对于 ul 和 button 是不需要进行更新，需要更新的是我们li的列表：

- 在Vue中，对于相同父元素的子元素节点并不会重新渲染整个列表；
- 因为对于列表中 a、b、c、d 它们都是没有变化的；
- 在操作 真实DOM 的时候，我们只需要在中间插入一个 f 的 li 即可；

那么Vue中对于列表的更新究竟是如何操作的呢？

- Vue事实上会对于 **有key** 和 **没有key** 会 **调用两个不同的方法**；
- **有key**，那么就使用 **patchKeyedChildren** 方法；
- **没有key**，那么久使用 **patchUnkeyedChildren** 方法；

#### 7.9 源码级 diff 算法分析（重点！）

##### 1）Vue源码对于key的判断

- **有key**，那么就使用 **patchKeyedChildren** 方法；

- **没有key**，那么久使用 **patchUnkeyedChildren** 方法；

<img src="img\Vue源码对于key的判断.png" />

##### 2）Vue源码 没有key 的操作

<img src="img\Vue源码没有key的操作.png" />

会发现此时的 **diff算法效率并不高**：

- c 和 d 来说它们事实上并不需要有任何的改动；

- 但是因为我们的c被f所使用了，所有后续所有的内容都要一次进行改动，并且最后进行新增；

<img src="img\没有key的diff算法.png" style="zoom: 120%;" />

##### 3）Vue源码 有key 的操作

<img src="img\Vue源码有key的操作.png" />

**第一步：从头开始进行遍历、比较，遇到相同的节点就继续，遇到不同的就跳出循环：**

- a 和 b 是一致的会继续进行比较；

- c 和 f 因为 key 不一致，所以就会 break 跳出循环；

<img src="img\有key-第一步.png" style="zoom: 120%;" />

**第二步：从尾部开始进行遍历、比较，遇到相同的节点就继续，遇到不同的就跳出循环：**

具体操作和第一步一样。

<img src="img\有key-第二步.png" style="zoom: 120%;" />

**第三步：如果旧节点遍历完毕，但是依然有新的节点，那么就新增节点：**

<img src="img\有key-第三步.png" style="zoom: 120%;" />

**第四步：如果新的节点遍历完毕，但是依然有旧的节点，那么就移除旧节点：**

<img src="img\有key-第四步.png" style="zoom: 120%;" />

**第五步：最特殊的情况 - 中间还有很多未知的或者乱序的节点，那么根据新VNode的 key 建立map，最大限度的使用旧节点：**

<img src="img\有key-第五步.png" style="zoom: 120%;" />

##### 4）diff算法总结

Vue在进行 **diff算法** 的时候，会尽量 **利用 key** 来进行 **优化操作** ：

- 在 **没有 key** 的时候我们的效率是 **非常低效的** ；
- 在进行 **插入或者重置顺序** 的时候，保持 **相同的key** 可以让diff算法更加的 **高效** ；

### 8.v-model

#### 8.1 概念

- **表单提交** 是开发中非常常见的功能，也是和用户交互的重要手段：
  - 比如用户在 **登录、注册** 时需要提交账号密码；
  - 比如用户在 **检索、创建、更新** 信息时，需要提交一些数据；
- 这些都要求我们可以在代码逻辑中获取到用户提交的数据，我们通常会使用v-model指令来完成：
  - v-model指令可以在表单 input、textarea以及select元素上创建双向数据绑定；
  - 它会根据控件类型自动选取正确的方法来更新元素；
  - 尽管有些神奇，但 v-model 本质上不过是语法糖，它负责监听用户的输入事件来更新数据，并在某种极端场景下进行一些特殊处理；

```vue
<template id="my-app">
    <!-- v-model原理：1.v-bind value的绑定 2.监听input事件, 更新message的值 -->
    <!-- <input type="text" :value="message" @input="inputChange"> -->
	<!-- 等价于 -->
    <input type="text" v-model="message">
    <h2>{{message}}</h2>
</template>
```

8.2 v-model绑定其他表单元素





### 9.Options API（为了兼容vue2）

Vue.createApp中的配置项，如data，template，method，watch等。

#### 9.1 computed（计算属性）

##### 1.概念

- 对于任何包含响应式数据的 **复杂逻辑** ，你都应该使用 **计算属性** 。

- **计算属性** 将被混入到 **组件实例** 中。所有 getter 和 setter 的 this 上下文自动地绑定为组件实例。

##### 2.用法

- 属性名：computed

- 属性值：{ [key: string]: Function | { get: Function, set: Function } }

##### 3.案例

- **案例一：**我们有两个变量：firstName 和 lastName，希望它们拼接之后在界面上显示；

- **案例二：**我们有一个分数：score

  - 当score大于60的时候，在界面上显示及格；


  - 当score小于60的时候，在界面上显示不及格；


- **案例三：**我们有一个变量message，记录一段文字：比如Hello World

  - 某些情况下我们是直接显示这段文字；


  - 某些情况下我们需要对这段文字进行反转；


**三种实现思路：**

- **思路一：**在 **模板语法** 中直接使用表达式；

- **思路二：**使用 **method** 对逻辑进行抽取；

- **思路三：**使用计算属性 **computed** ；

**实现思路一：模板语法**

- **缺点一：**模板中存在大量的复杂逻辑， **不便于维护** （模板中 **表达式** 的 **初衷是用于简单的计算** ）；

- **缺点二：**当有多次一样的逻辑时，**存在重复的代码**；

- **缺点三：**多次使用的时候，很多运算也需要 **多次执行** ，**没有缓存**；

```html
<template id="my-app">
    <h2>{{firstName + " " + lastName}}</h2>
    <h2>{{score >= 60 ? '及格': '不及格'}}</h2>
    <h2>{{message.split(" ").reverse().join(" ")}}</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                firstName: "Kobe",
                lastName: "Bryant",
                score: 80,
                message: "Hello World"
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**实现思路二：method**

- 缺点一：事实上先显示的是 **一个结果** ，但是都变成了 **一种方法的调用** ；

- 缺点二：多次使用方法的时候， **没有缓存** ，也需要 **多次计算** ；

```html
<template id="my-app">
    <h2>{{getFullName()}}</h2>
    <h2>{{getResult()}}</h2>
    <h2>{{getReverseMessage()}}</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                firstName: "Kobe",
                lastName: "Bryant",
                score: 80,
                message: "Hello World"
            }
        },
        methods: {
            getFullName() {
                return this.firstName + " " + this.lastName;
            },
            getResult() {
                return this.score >= 60 ? "及格": "不及格";
            },
            getReverseMessage() {
                return this.message.split(" ").reverse().join(" ");
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**实现思路三：computed**

- 计算属性看起来像是一个函数，但是我们在使用的时候不需要加()；

- 我们会发现无论是直观上，还是效果上计算属性都是更好的选择；

- 并且计算属性是有缓存的；

```html
<template id="my-app">
    <h2>{{fullName}}</h2>
    <h2>{{result}}</h2>
    <h2>{{reverseMessage}}</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                firstName: "Kobe",
                lastName: "Bryant",
                score: 80,
                message: "Hello World"
            }
        },
        computed: {
            // 定义了一个计算属性叫fullname
            fullName() {
                return this.firstName + " " + this.lastName;
            },
            result() {
                return this.score >= 60 ? "及格": "不及格";
            },
            reverseMessage() {
                return this.message.split(" ").reverse().join(" ");
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

##### 4.computed 和 methods 的区别

我们来看一下同一个计算多次使用，computed 和 methods 的差异：

```html
<template id="my-app">
    <button @click="changeFirstName">修改firstName</button>
    <h2>{{fullName}}</h2>
    <h2>{{fullName}}</h2>
    <h2>{{fullName}}</h2>
    <hr>
    <h2>{{getFullName()}}</h2>
    <h2>{{getFullName()}}</h2>
    <h2>{{getFullName()}}</h2>
</template>

<script src="../js/vue.js"></script>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                firstName: "Kobe",
                lastName: "Bryant"
            }
        },
        computed: {
            fullName() {
                console.log("computed的fullName中的计算");
                return this.firstName + " " + this.lastName;
            }
        },
        methods: {
            getFullName() {
                console.log("methods的getFullName中的计算");
                return this.firstName + " " + this.lastName;
            },
            changeFirstName() {
                this.firstName = "Coder"
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

第一次执行结果：

```
computed的fullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
```

点击按钮改变firstName后的执行结果：

```
computed的fullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
computed的fullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
methods的getFullName中的计算
```

**总结：**

- 计算属性会基于它们的 **依赖关系进行缓存** ，在 **数据不发生变化** 时，计算属性是 **不需要重新计算** 的；

- 如果 **依赖的数据发生变化** ，在使用时，计算属性依然会 **重新进行计算** ；

##### 5.计算属性的 setter 和 getter

计算属性在大多数情况下，只需要一个 **getter方法** 即可，所以我们会将计算属性直接写成一个函数。

但是，如果我们确实想设置计算属性的值，则可以给计算属性设置一个 **setter方法** 。

```html
<template id="my-app">
    <button @click="changeFullName">修改fullName</button>
    <h2>{{fullName}}</h2>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                firstName: "Kobe",
                lastName: "Bryant"
            }
        },
        computed: {
            // fullName的getter方法
            fullName() {
                return this.firstName + " " + this.lastName;
            },
            // fullName的getter和setter方法
            fullName: {
                get: function() {
                    return this.firstName + " " + this.lastName;
                },
                set: function(newValue) {
                    console.log(newValue);
                    const names = newValue.split(" ");
                    this.firstName = names[0];
                    this.lastName = names[1];
                }
            }
        },
        methods: {
            changeFullName() {
                this.fullName = "Coder Why";
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

**扩展：**Vue内部是如何对我们传入的是一个getter，还是说是一个包含setter和getter的对象进行处
理的呢？

Vue源码内部只是对传入的计算属性做了一个类型判断，如果是函数就当作getter，否则分别取对象中的get和set当作getter和getter。

<img src="img\vue源码对计算属性setter和getter的处理.jpg" />

#### 9.2 watch（侦听器）

##### 1.概念

- 开发中我们在data返回的对象中定义了数据，这个数据通过 **插值语法等方式绑定到template** 中；

- 当数据变化时，template会自动进行更新来显示最新的数据；

- 但是在某些情况下，我们希望在 **代码逻辑** 中监听某个数据的变化，这个时候就需要用 **侦听器watch** 来完成了；

##### 2.用法

- 性名：watch

- 属性值：{ [key: string]: string | Function | Object | Array}

##### 3.案例

- 现在希望用户在input中输入一个问题；

- 每当用户输入了最新的内容，我们就获取到最新的内容，并且使用该问题去服务器查询答案；

- 那么，我们就需要实时的去获取最新的数据变化；

```html
<template id="my-app">
    您的问题: <input type="text" v-model="question">
    <!-- <button @click="queryAnswer">查找答案</button> -->
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                // 侦听question的变化时, 去进行一些逻辑的处理(JavaScript, 网络请求)
                question: "Hello World",
                anwser: ""
            }
        },
        watch: {
            // question侦听的data中的属性的名称
            // newValue变化后的新值
            // oldValue变化前的旧值
            question: function(newValue, oldValue) {
                console.log("新值: ", newValue, "旧值", oldValue);
                this.queryAnswer();
            }
        },
        methods: {
            queryAnswer() {
                this.anwser="哈哈哈"
                console.log(`你的问题${this.question}的答案是${this.anwser}`);
                this.anwser = "";
            }
        }
    }a
    Vue.createApp(App).mount('#app');
</script>
```

##### 4.配置选项

案例：

- 当我们点击按钮的时候会 **修改info.name** 的值；

- 这个时候我们使用 **watch来侦听info，可以侦听到吗** ？答案是 **不可以** 。

这是因为默认情况下， **watch只是在侦听info的引用变化** ， **对于内部属性的变化是不会做出响应的** ：

- 这个时候我们可以使用一个 **选项deep** 进行更深层的侦听；

- 注意前面说过watch里面侦听的属性对应的也可以是一个Object；

还有 **immediate属性** ，是 **希望一开始的就会立即执行一次** ：

- 这个时候我们使用 **immediate选项** ；

- 这个时候无论后面数据是否有变化，侦听的函数都会先执行一次；

```html
<template id="my-app">
    <h2>{{info.name}}</h2>
    <button @click="changeInfo">改变info</button>
    <button @click="changeInfoName">改变info.name</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                info: { name: "why", age: 18, nba: {name: 'kobe'} }
            }
        },
        watch: {
            // 默认情况下我们的侦听器只会针对监听的数据本身的改变(内部发生的改变是不能侦听)
            // info(newInfo, oldInfo) {
            //   console.log("newValue:", newInfo, "oldValue:", oldInfo);
            // }

            // 深度侦听/立即执行(一定会执行一次)
            info: {
                handler: function(newInfo, oldInfo) {
                    console.log("newValue:", newInfo, "oldValue:", oldInfo);
                },
                deep: true,// 深度侦听
                // immediate: true // 立即执行
            }
        },
        methods: {
            changeInfo() {
                this.info = {name: "kobe"};
            },
            changeInfoName() {
                this.info.name = "kobe";
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

##### 5.watch的其他写法

```html
<template id="my-app">
    <h2>{{info.name}}</h2>
    <button @click="changeInfo">改变info</button>
    <button @click="changeInfoName">改变info.name</button>
    <button @click="changeInfoNbaName">改变info.nba.name</button>
    <button @click="changeFriendName">改变friends[0].name</button>
</template>
<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                info: { name: "why", age: 18, nba: {name: 'kobe'} },
                friends: [
                    {name: "why"}, 
                    {name: "kobe"}
                ]
            }
        },
        watch: {
            // 侦听顶级 property
            info(newValue, oldValue) {
                console.log(newValue, oldValue);
            },
            // 侦听单个嵌套 property
            "info.name": function(newName, oldName) {
                console.log(newName, oldName);
            },
            
            // 不支持这种写法
            "friends[0].name": function(newName, oldName) {
                console.log(newName, oldName);
            },
            // watch监听不到引用类型的旧值，因为使用的是同一个内存空间，没有备份！
            // 可以在修改前自己拷贝一份到data中
            // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
            friends: {
                handler(newFriends, oldFriends) {
                    console.log(newFriends, oldFriends);
                },
                deep: true
            },
            //建议在子组件里监听，不要在父组件里嵌套多层监听

            // 字符串方法名
            // info: 'watchInfo',

            // 你可以传入回调数组，它们会被逐一调用
            // info: [
            //   'handle1',
            //   function handle2(val, oldVal) {
            //     console.log('handle2 triggered')
            //   },
            //   {
            //     handler: function handle3(val, oldVal) {
            //       console.log('handle3 triggered')
            //     }
            //     /* ... */
            //   }
            // ]
        },
        methods: {
            changeInfo() {
                this.info = {name: "kobe"};
            },
            changeInfoName() {
                this.info.name = "kobe";
            },
            changeInfoNbaName() {
                this.info.nba.name = "james";
            },
            changeFriendName() {
                this.friends[0].name = "curry";
            },
            watchInfo(newValue, oldValue) {
                console.log(newValue, oldValue);
            },
            handle1() {
                console.log('handle 1 triggered')
            }
        },

        // 命令式设置侦听器，使用 $watch API
        // 在created生命周期里使用 this.$watch 来侦听，并获得取消侦听的函数
        // 第一个参数是要侦听的源；
        // 第二个参数是侦听的回调函数callback；
        // 第三个参数是额外的其他选项，比如deep、immediate；
        created() {
            const unwatch = this.$watch("info", function(newInfo, oldInfo) {
                console.log(newInfo, oldInfo);
            }, {
                deep: true,
                immediate: true
            })
            // 取消侦听器
            // unwatch()
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

##### 6.综合案例 - 书籍购物车

<img src="img\综合案例-书籍购物车.jpg" />

**案例说明：**

1.在界面上以表格的形式，显示一些书籍的数据；

2.在底部显示书籍的总价格；

3.点击+或者-可以增加或减少书籍数量（如果为1，那么不能继续-）；

4.点击移除按钮，可以将书籍移除（当所有的书籍移除完毕时，显示：购物车为空~）；

```html
<head>
    <link rel="stylesheet" href="./style.css" />
</head>
<div id="app"></div>
<template id="my-app">
    <template v-if="books.length > 0">
        <table>
            <thead>
                <th>序号</th>
                <th>书籍名称</th>
                <th>出版日期</th>
                <th>价格</th>
                <th>购买数量</th>
                <th>操作</th>
            </thead>
            <tbody>
                <tr v-for="(book,index) in books">
                    <td>{{index+1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.date}}</td>
                    <td>{{formatPrice(book.price)}}</td>
                    <td>
                        <button :disabled="book.count <= 1" @click="decrement(index)">
                            -
                        </button>
                        <span class="counter">{{book.count}}</span>
                        <button @click="increment(index)">+</button>
                    </td>
                    <td><button @click="removeBook(index)">移除</button></td>
                </tr>
            </tbody>
        </table>
        <h2>总价: {{formatPrice(totalPrice)}}</h2>
    </template>
    <template v-else>
        <h2>购物车为空~</h2>
    </template>
</template>
<script src="../js/vue.js"></script>
<script src="./index.js"></script>
```

```css
table {
  border: 1px solid #e9e9e9;
  border-collapse: collapse;
  border-spacing: 0px;
}

th,
td {
  padding: 8px 16px;
  border: 1px solid #e9e9e9;
  text-align: left;
}

th {
  background-color: #f7f7f7;
  color: #5c6b77;
  font-weight: 600;
}

.counter {
  margin: 0 5px;
}
```

```js
Vue.createApp({
  template: "#my-app",
  data() {
    return {
      books: [
        {
          id: 1,
          name: "《算法导论》",
          date: "2006-9",
          price: 85.0,
          count: 1,
        },
        {
          id: 2,
          name: "《UNIX编程艺术》",
          date: "2006-2",
          price: 59.0,
          count: 1,
        },
        {
          id: 3,
          name: "《编程珠玑》",
          date: "2008-10",
          price: 39.0,
          count: 1,
        },
        {
          id: 4,
          name: "《代码大全》",
          date: "2006-3",
          price: 128.0,
          count: 1,
        },
      ],
    };
  },
  methods: {
    increment(index) {
      this.books[index].count++;
    },
    decrement(index) {
      this.books[index].count--;
    },
    removeBook(index) {
      this.books.splice(index, 1);
    },
    // 价格格式化通用方法
    formatPrice(price) {
      return "￥" + price;
    },
  },
  computed: {
    totalPrice() {
      let totalPrice = 0;
      this.books.forEach(book => {
        totalPrice += book.count * book.price;
      });
      return totalPrice;
    },
  },
}).mount("#app");
```



## 三、组件化开发



