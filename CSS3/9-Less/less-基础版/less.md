# less

## 1.less简介

```css
less是一门css的预处理语言
- less是一个css的增强版，通过less可以编写更少的代码实现更强大的样式
- 在less中添加了许多的新特性：像对变量的支持、对mixin的支持... ...
- less的语法大体上和css语法一致，但是less中增添了许多对css的扩展，
	所以浏览器无法直接执行less代码，要执行必须向将less转换为css，然后再由浏览器执行

原生css变量语法:
1.声明变量   --length:200px;
2.使用变量   var(--length);


实例:
html{
    /* css原生也支持变量的设置 */
    --color:#ff0;
    --length:200px;
}

.box1{
    /* calc()计算函数 */
    width: calc(200px*2);
    height: var(--length);
    background-color: var(--color);
}

.box2{
    width: var(--length);
    height: var(--length);
    color: var(--color);
}

.box3{
    width: var(--length);
    height: var(--length);
    border: 10px solid var(--color);
}

<div class="box1">aaaa</div>
<div class="box2">aaaa</div>
<div class="box3">aaaa</div>

注:
VSCode里解析LESS:
- 安装vscode插件: EasyLESS
- EasyLESS的setting.json配置项设置:
"less.compile": {
    "compress": false, //是否压缩
    "sourceMap": true, //是否生成map文件，有了这个可以在调试台看到less行数
    "out": true, // 是否输出css文件，false为不输出
    "outExt": ".css", // 输出文件的后缀,小程序可以写'wxss'
}
```

## 2.less基础语法

```less
1.后代/子元素选择器语法
.box1{
    .box2{
        color: red;
        .box4{
            color: red;
        }
    }

    >.box3{
        color: red;
        //& 就表示外层的父元素
        &:hover{
            color: blue;
        }
    }

    //为box1设置一个hover
    //& 就表示外层的父元素
    &:hover{
        color: orange;
    }

    div &{
        width: 100px;
    }
}


2.变量声明与使用
变量，在变量中可以存储一个任意的值
并且我们可以在需要时，任意的修改变量中的值
变量的语法： @变量名

@a:200px;
@b:#bfa;
@c:box6;
// @c:.box6;

变量的使用:
.box5{
    //使用变量时，如果是直接使用则以 @变量名 的形式使用即可
    width: @a;
    color: @b;
}


3.作为类名，或者一部分值使用时必须以 @{变量名} 的形式使用
.@{c}{
// @{c}{
    width: @a;
    background-image: url("@{c}/1.png");
}


4.同名变量的处理与声明前就使用变量
@d:200px;
@d:300px;

div{
    // 变量发生重名时，用同名变量最后赋值的内容
    @d:115px;
    width: @d;
    height: @e;
}

//可以在变量声明前就使用变量
@e:335px;


5.应用其他属性值
div{
    width: 300px;
    //应用width的值
    height: $width;
}


6.extend与mixin混合函数
.p1{
    width: 100px;
    height: 200px;
}

//:extend() 对当前选择器扩展指定选择器的样式（选择器分组）
.p2:extend(.p1){
    color: red;
}

.p3{
    //直接对指定的样式进行引用，这里就相当于将p1的样式在这里进行了复制
    //mixin 混合
    .p1();
}

// 使用类选择器时可以在选择器后边添加一个括号，这时我们实际上就创建了一个mixins
.p4(){
    width: 100px;
    height: 100px;
}

.p5{
    .p4;
}

//混合函数 在混合函数中可以 直接设置变量
.test(@w:100px,@h:200px,@bg-color:red){
    width: @w;
    height: @h;
    border: 1px solid @bg-color;
}

div{
    //调用混合函数，按顺序传递参数
    // .test(200px,300px,#bfa);
    .test(300px);
    // .test(@bg-color:red, @h:100px, @w:300px);
}


7.less预设的mixin混合函数
span{
    color: average(red,blue);
}

html{
    width: 100%;
    height: 100%; 
}
body {
    width: 100%;
    height: 100%;
    background-color: #bfa;
}

body:hover{
    background-color: darken(#bfa,50%);
}


8.less的加减乘除
.box1{
    // 在less中所有的数值都可以直接进行运算
    // + - * /
    width: 100px + 100px;
    height: 100px/2;
    background-color: #bfa;
}

9.less文件的导入（模块化）
//import用来将其他的less引入到当前的less
@import "syntax2.less";


注:
// less中的单行注释，注释中的内容不会被解析到css中
/*
    css中的注释，内容会被解析到css文件中
*/
```

