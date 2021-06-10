# less

## 1.less简介

	less是一种动态样式语言，属于css预处理器的范畴，它扩展了 CSS 语言，
	它增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。
	
	LESS 既可以在 客户端 上运行 ，也可以借助 Node.js 在服务端运行。
	
	优势: 变量和选择器嵌套
	
	less的中文官网：http://lesscss.cn/
	
	css预编译器分类:
	less --- 支持原生js,node
	sass --- ruby环境
	stylus --- node -- 开发项目中我们使用stylus

## 2.less初体验：

```less
1.定义变量（@zero:0）并运用，凡是页面中使用0，都用@zero代替;

2.子元素需要放在父元素中，此时子元素可以放心使用class，因为他已经有前提条件是父元素
    不用再继续担心对同名class值样式的影响
    
3.<style type="text/less"> style标签的类型需要改成less

4.根据官网我们需要一个less编译的	less.js 文件，并在最下方引入，
    因为需要读取页面中所有less相关的文件，才可以进行编译

注: 分组写css，减少代码的垂直长度。

实例: 元素水平垂直居中
<style type="text/less">
@zero:0;

*{
    margin: @zero;
    padding: @zero;
}

#wrap{
    //分组写css，减少代码的垂直长度
    position: absolute;  left: @zero;  top:@zero;  bottom: @zero;  right:@zero;
    width: 500px;height: 500px;margin: auto;
    background: yellowgreen;
    .box{
        position: absolute;  left: @zero;  top:@zero;  bottom: @zero;  right:@zero;
        width: 200px;height: 300px;margin: auto;
        background: pink;
    }
}
</style>

<!-- 引入less.js可以不解析为css直接使用less -->
<script type="text/javascript" src="js/less.js"></script>

<div id="wrap">
    <div class="box">
        <div class="inner"></div>
    </div>
</div>
```

## 3.less编译工具与编译插件

```json
1.koala 官网:www.koala-app.com
- webstorm可使用

2.引入官网的less.js可免编译，直接使用

3.VSCode里解析LESS:
- 安装vscode插件: EasyLESS
- EasyLESS的setting.json配置项设置:
"less.compile": {
    "compress": false, //是否压缩
    "sourceMap": true, //是否生成map文件，有了这个可以在调试台看到less行数
    "out": true, // 是否输出css文件，false为不输出
    "outExt": ".css", // 输出文件的后缀,小程序可以写'wxss'
}
```

## 4.less中的注释

   	以//开头的注释，不会被编译到css文件中
   	以/**/包裹的注释会被编译到css文件中

## 5.less中的变量

```less
- 使用@来申明一个变量：@pink: pink;

1.作为普通属性值只来使用：直接使用@pink
@a:200px;
@b:#bfa;
.box5{
    width: @a;
    color: @b;
}

2.作为选择器和属性名：#@{selector的值}的选择器形式, @{selector的值}属性名
3.作为URL：@url
@c:box6;
@w:height;
// @c:.box6;
.@{c}{
// @{c}{
    background-image: url("@{c}/1.png");
    @{w}: 500px;
}

4.变量的延迟加载
实例:
@var: 0;
.class {
    @var: 1;
    .brass {
      @var: 2;
      three: @var; //3
      @var: 3;
    }
    @var: 4;
    one: @var; //4
}

变量查找规则:
1.先找子元素，如果子元素中有两个或者多着相同变量，我们默认拿最后一个。(变量发生重名时，用同名变量最后赋值的内容)
2.在找父元素，把子元素东西排除掉,如果没有，向上一层去找
注: 如果自身没有，就会往外找。无论在那一层找，都是使用这一层的最后一个

注: 颜色使用变量较多！
```

## 6.less中的选择器嵌套规则

```less
1.基本嵌套规则
2.&的使用
& 就表示外层的父元素, 也可以说成是前边所有祖先选择器的集合
}
注:
1.在使用嵌套的时候,要注意合理的嵌套。
2.一旦发现有定义比较特殊的class 或者 id值等情况，就可以单独拿出来写。

.box1{
    .box2{
        color: red;
        .box4{
            color: red;
        }
    }

    >.box3{
        color: red;
        //& 就表示外层的父元素, 也可以说成是前边所有祖先选择器的集合
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
```

## 7.less加减乘除运算

```less
.box1{
    // 在less中所有的数值都可以直接进行运算
    // + - * /
    width: 100px + 100px;
    height: 100px/2;
    background-color: #bfa;
}
```

## 8.less中的混合 Mixin

```less
混合就是将一系列属性从一个规则集引入到另一个规则集的方式
1.普通混合
2.带参数的混合
3.带参数并且有默认值的混合
4.带多个参数的混合

注:
1) 定义混合 mixin
.juzhong(@w:500px,@h:500px,@bg){
  position: absolute;
  left: @zero;
  right: @zero;
  bottom: @zero;
  top: @zero;
  margin: auto;
  width: @w;
  height: @h;
  background: @bg;
}
2) 如果定义混合时,声明了默认值,在调用不传递参数时，设置了默认值的参数
就会使用默认值;如果传递,就是用传递的值。
3) 使用混合,结尾一定要加分号。

重要！！！
注: 要总结自己的less模板，针对各种需求，如商品布局。


实例:
//水平垂直居中
.juzhong(@w:500px,@h:500px,@bg){
  position: absolute;
  left: @zero;
  right: @zero;
  bottom: @zero;
  top: @zero;
  margin: auto;
  width: @w;
  height: @h;
  background: @bg;
}
#wrap{
    .juzhong(@bg: pink);
  .box1{
    .juzhong(@w:400px,@h: 400px,@bg: yellowGreen);
  }
  .box2{
    .juzhong(@w:300px,@h: 300px,@bg: skyblue);
  }
  .box3{
    .juzhong(@w:200px,@h: 200px,@bg: orange);
  }
}
<div id="wrap">
    <div class="box1">1</div>
    <div class="box2">2</div>
    <div class="box3">3</div>
</div>


//扩展1: 按钮通用混合
.btn1(@w,@h,@bg,@color){
  display: block;
  border-radius: 5px;
  width: @w;
  height: @h;
  line-height: @h;
  color: @color;
  background: @bg;
  border: 1px solid #ccc;
  text-align: center;
}


//扩展2: 商品列表布局
.hideBox(@box,@num,@w,@h,@mr){
//  @box  父容器选择器
//  @num  每行的列数   商品个数
//  @w    商品容器宽度
//  @h    商品容器高度
//  @mr   商品容器右边距
//<div class="box">
//  <div class="hideBox">
//    <div class="item"></div>
//    <div class="item"></div>
//    <div class="item"></div>
//    <div class="item"></div>
//  </div>
//</div>
  .@{box}{
    width: (@w*@num)+( @num - 1)*@mr;
    border: 1px solid #000;
    margin: 0 auto;
    overflow: hidden;
    .hideBox{
      width: (@w + @mr)*@num;
      .item{
        width: @w;
        height: @h;
        margin-right: @mr;
        margin-bottom: 30px;
        background: pink;
        float: left;
      }
    }
  }
}

.hideBox(@box:inner,@num:4,@w:200px,@h:300px,@mr:50px);

<div class="inner">
    <div class="hideBox">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
</div>
```

## 9.less文件的导入（模块化）

```less
//index.less文件
@zero:0;

//引入外部less文件
@import "mixins/mixin.less";

* {
  margin: @zero;
  padding: @zero;
}

#wrap{
  //调用混合
  .juzhong(@w:500px,@h:500px,@bg:grey);
  .box1{
    //调用混合
    .juzhong(@w:400px,@h:400px,@bg:deeppink);
  }
  .box2{
    //调用混合
    .juzhong(@w:300px,@bg:greenyellow);
  }
  .box3{
    //调用混合
    .juzhong(@w:100px,@h:100px,@bg:green);
  }
}

//mixin.less文件
//定义混合 mixin
.juzhong(@w, @h: 250px, @bg) {
  position: absolute;
  top: @zero;
  left: @zero;
  right: @zero;
  bottom: @zero;
  margin: auto;
  width: @w;
  height: @h;
  background: @bg;
}

<div id="wrap">
    <div class="box1">1</div>
    <div class="box2">2</div>
    <div class="box3">3</div>
</div>
```

