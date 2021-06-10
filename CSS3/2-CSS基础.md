# CSS基础

## 1.基本选择器

### 1.1元素选择器

```css
根据标签名来选中指定的一类元素

语法: 标签名{}

实例:
p {
	color: red;
}
h1 {
	color: green;
}
```

### 1.2id选择器

```css
根据元素的id属性值选中一个元素

语法: #id属性值{}

元素的id值是唯一的，只能对应于文档中某一个具体的元素。

实例:
#red {
    color: red;
}
<p id="red">儿童相见不相识</p>
```

### 1.3类选择器

```css
根据元素的class属性值选中一组元素

语法: .class属性值

实例:
.blue {
	color: blue;
}

.abc {
	font-size: 20px;
}

class 是一个标签的属性，它和id类似，不同的是class可以重复使用
    可以通过class属性来为元素分组
    可以同时为一个元素指定多个class属性

<h1 class="blue abc">我是标题</h1>
<p class="blue">秋水共长天一色</p>
<p class="blue">落霞与孤鹜齐飞</p>
```

### 1.4通配选择器

```css
选中页面中的所有元素

语法: *

实例:
* {
  margin: 0;
  padding: 0;
}
```

## 2.复合选择器

### 2.1交集选择器

```css
选中同时复合多个条件的元素

语法: 选择器1选择器2选择器3选择器n{}

注意点:
	交集选择器中如果有元素选择器，必须使用元素选择器开头

实例:
div.red{
    font-size: 30px;
}
.a.b.c{
    color: blue
}
div#box1{}
```

### 2.2并集选择器

```css
作用：同时选择多个选择器对应的元素

语法：选择器1,选择器2,选择器3,选择器n{}

实例:
h1, span{
    color: green
}
```

## 3.关系选择器

### 3.1子元素选择器

```css
选中指定父元素的指定子元素

语法: 父元素 > 子元素

实例:
div.box > span{
    color: orange;
}

div > p > span{
    color: red;
}
```

### 3.2后代选择器

```css
选中指定元素内的指定后代元素

语法: 祖先 后代

实例:
div span{
     color: skyblue
}
```

### 3.3其他关系选择器

```css
选择下一个兄弟
语法：前一个 + 下一个

选择下边所有的兄弟
语法：兄 ~ 弟

实例:
p + span{
	color: red;
}

p ~ span{
	color: red;
}
```

## 4.属性选择器

```css
[属性名] 选择含有指定属性的元素
[属性名=属性值] 选择含有指定属性和属性值的元素
[属性名^=属性值] 选择属性值以指定值开头的元素
[属性名$=属性值] 选择属性值以指定值结尾的元素
[属性名*=属性值] 选择属性值中含有某值的元素的元素

实例:
p[title]{
p[title=abc]{
p[title^=abc]{
p[title$=abc]{
p[title*=e]{
    color: orange;
}
```

## 5.伪类选择器

```css
伪类（不存在的类，特殊的类）

- 伪类用来描述一个元素的特殊状态
比如：第一个子元素、被点击的元素、鼠标移入的元素...

- 伪类一般情况下都是使用:开头
:first-child 第一个子元素
:last-child 最后一个子元素
:nth-child() 选中第n个子元素
特殊值：
n 第n个 n的范围0到正无穷
2n 或 even 表示选中偶数位的元素
2n+1 或 odd 表示选中奇数位的元素

- 以上这些伪类都是根据所有的子元素进行排序

- 这几个伪类的功能和上述的类似，不通点是他们是在同类型元素中进行排序
:first-of-type
:last-of-type
:nth-of-type()

- :not() 否定伪类
将符合条件的元素从选择器中去除


实例:
ul > li:first-child{
ul > li:last-child{
ul > li:nth-child(2n+1){
ul > li:nth-child(even){
ul > :first-child{
    color: red;
}

ul > li:first-of-type{
    color: red;
}

ul > li:not(:nth-of-type(3)){
    color: yellowgreen;
}


表单伪类:
input {
    outline: none;
}

/* 表单获得焦点 */
input:focus {
    background-color: pink;
    border: 1px solid red;
    height: 30px;
}
<input type="text">
```

## 6.a的伪类选择器

```css
:link 用来表示没访问过的链接（正常的链接）

a:link{
    color: red;
}

:visited 用来表示访问过的链接
由于隐私的原因，所以visited这个伪类只能修改链接的颜色

a:visited{
    color: orange; 
    /* font-size: 50px;   */
}

:hover 用来表示鼠标移入的状态

a:hover{
    color: aqua;
    font-size: 50px;
}

:active 用来表示鼠标点击

a:active{
    color: yellowgreen;
}

<a href="https://www.baidu.com">访问过的链接</a>
<a href="https://www.baidu123.com">没访问过的链接</a>

注: 
	1.:link和:visited是超链接独有的!
	2.写的时候，他们的顺序尽量不要颠倒  按照 lvha 的顺序。否则可能引起错误。
	3.实际工作开发中，我们很少写全四个状态。
```

## 7.伪元素选择器

```css
伪元素，表示页面中一些特殊的并不真实的存在的元素（特殊的位置）

伪元素使用 :: 开头
::first-letter 表示第一个字母
::first-line 表示第一行
::selection 表示选中的内容
::before 元素的开始 
::after 元素的最后
	- before 和 after 必须结合content属性来使用

实例:
p::first-letter{
    font-size: 50px;
}

p::first-line{
    background-color: yellow; 
}

p::selection{
    background-color: greenyellow;
}

div::before{
    content: 'abc';
    color: red;
}

div::after{
    content: 'haha';
    color: blue;
}
```

## 8.选择器的权重

```css
样式冲突
- 当我们通过不同的选择器，选中相同的元素，并且为相同的样式设置不同的值时，此时就发生了样式的冲突。

发生样式冲突时，应用哪个样式由选择器的权重（优先级）决定

选择器的权重
内联样式        1,0,0,0
id选择器        0,1,0,0
类选择器、伪类选择器、属性选择器   0,0,1,0
元素选择器、伪元素选择器          0,0,0,1
通配选择器       0,0,0,0
继承的样式       没有优先级

比较优先级时，需要将所有的选择器的优先级进行相加计算，最后优先级越高，则越优先显示（并集选择器是单独计算的）,
选择器的累加不会超过其最大的数量级，类选择器在高也不会超过id选择器
如果优先级计算后相同，此时则优先使用靠下的样式

可以在某一个样式的后边添加 !important ，则此时该样式会获取到最高的优先级，甚至超过内联样式，
注意: 在开发中这个玩意一定要慎用！

实例:
#box1{
    background-color: orange;
}

div#box1{
    background-color: yellow;
}

.d1{
    background-color: purple !important;
}
.red{
    background-color: red;
    /* font-size: 20px; */
}

div,p,span{
    background-color:yellowgreen;
}
*{
    font-size: 50px;
}
div{
    font-size: 20px;
}

<div id="box1" class="red d1 d2 d3 d4" style="background-color: chocolate;">我是一个div <span>我是div中的span</span></div>
```

## 9.样式的继承

```css
样式的继承，我们为一个元素设置的样式同时也会应用到它的后代元素上

继承是发生在祖先后后代之间的

继承的设计是为了方便我们的开发，
利用继承我们可以将一些通用的样式统一设置到共同的祖先元素上，
这样只需设置一次即可让所有的元素都具有该样式

注意：并不是所有的样式都会被继承：
比如 背景相关的，布局相关等的这些样式都不会被继承。

实例:
body{
    font-size: 12px;
}
p{
    color: red;
    background-color: orange;
}
div{
    color: yellowgreen
}

<p>
	我是一个p元素
	<span>我是p元素中的span</span>
</p>
<span>我是p元素外的span</span>
<div>
    我是div
    <span>
        我是div中的span
        <em>我是span中的em</em>
    </span>
</div>
```

## 10.长度单位

```css
像素
- 屏幕（显示器）实际上是由一个一个的小点点构成的
- 不同屏幕的像素大小是不同的，像素越小的屏幕显示的效果越清晰
- 所以同样的200px在不同的设备下显示效果不一样

百分比
- 也可以将属性值设置为相对于其父元素属性的百分比
- 设置百分比可以使子元素跟随父元素的改变而改变

em
- em是相对于元素的字体大小来计算的
- 1em = 1font-size
- em会根据字体大小的改变而改变

rem
- rem是相对于根元素的字体大小来计算

实例:
html{
    font-size: 30px;
}

.box1{
    width:300px;
    height: 100px;
    background-color: orange;
}
.box2{
    width: 50%;
    height: 50%;
    background-color:aqua;
}
.box3{
    font-size: 50px;
    /* width: 10em;
    height: 10em; */
    width: 10rem;
    height: 10rem;
    background-color: greenyellow;
}

<div class="box1">
	<div class="box2"></div>
</div>
<div class="box3"></div>
```

## 11.颜色单位

```css
在CSS中可以直接使用颜色名来设置各种颜色
比如：red、orange、yellow、blue、green ... ...
但是在css中直接使用颜色名是非常的不方便

RGB值：
- RGB通过三种颜色的不同浓度来调配出不同的颜色
- R red，G green ，B blue
- 每一种颜色的范围在 0 - 255 (0% - 100%) 之间
- 语法：RGB(红色,绿色,蓝色)

RGBA:
- 就是在rgb的基础上增加了一个a表示不透明度
- 需要四个值，前三个和rgb一样，第四个表示不透明度
1表示完全不透明   0表示完全透明  .5半透明

十六进制的RGB值：
- 语法：#红色绿色蓝色
- 颜色浓度通过 00-ff
- 如果颜色两位两位重复可以进行简写  
#aabbcc --> #abc

HSL值 HSLA值
H 色相(0 - 360)
S 饱和度，颜色的浓度 0% - 100%
L 亮度，颜色的亮度 0% - 100%

实例:
.box1{
    width: 100px;
    height: 100px;
    background-color: red;
    background-color: rgb(255, 0, 0);
    background-color: rgb(0, 255, 0);
    background-color: rgb(0, 0, 255);
    background-color: rgb(255,255,255);
    background-color: rgb(106,153,85);
    background-color: rgba(106,153,85,.5);
    background-color: #ff0000;
    background-color: #ffff00;
    background-color: #ff0;
    background-color: #bbffaa;
    background-color: #9CDCFE;
    background-color: rgb(254, 156, 156);
    background-color: hsl(98, 48%, 40%, 0.658);
    background-color: hsla(98, 48%, 40%, 0.658);
}
<div class="box1"></div>
```

