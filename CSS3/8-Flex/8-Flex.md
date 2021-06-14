# flex

## 1.弹性盒简介（flex）

```css
flex(弹性盒、伸缩盒)
    - 是CSS中的又一种布局手段，它主要用来代替浮动来完成页面的布局
    - flex可以使元素具有弹性，让元素可以跟随页面的大小的改变而改变
1.弹性容器
    - 要使用弹性盒，必须先将一个元素设置为弹性容器
    - 我们通过 display 来设置弹性容器
        display:flex  设置为块级弹性容器
        display:inline-flex 设置为行内的弹性容器

2.弹性元素
    - 弹性容器的子元素是弹性元素（弹性项）
    - 弹性元素可以同时是弹性容器


实例:
ul{
    width: 500px;
    border: 10px red solid;
    /* 将ul设置为弹性容器 */
    display: flex;

    flex-direction: row;
}
li{
    width: 200px;
    height: 100px;
    background-color: #bfa;
    font-size: 50px;
    text-align: center;
    line-height: 100px;
    
    /* flex-grow: 1; */
    
    flex-shrink: 0;
}
li:nth-child(1){
    flex-grow: 0;
    flex-shrink: 1;
}
li:nth-child(2){
    background-color: pink;
    /* flex-grow: 2; */
    flex-shrink: 2;
}
li:nth-child(3){
    background-color: orange;
    /* flex-grow: 3; */
    flex-shrink: 3;
}
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
```

## 2.弹性容器的样式

```css
1.flex-direction 指定容器中弹性元素的排列方式
  可选值：
    row 默认值，弹性元素在容器中水平排列（左向右）
        - 主轴 自左向右
    row-reverse 弹性元素在容器中反向水平排列（右向左）
        - 主轴 自右向左
    column 弹性元素纵向排列（自上向下）
    column-reverse 弹性元素方向纵向排列（自下向上）

    主轴：（默认为x轴）
        弹性元素的排列方向称为主轴
    侧轴：（默认为y轴）
        与主轴垂直方向的称为侧轴

2.flex-wrap: 设置弹性元素是否在弹性容器中自动换行
  可选值：
    nowrap 默认值，元素不会自动换行
    wrap 元素沿着辅轴方向自动换行
    wrap-reverse 元素沿着辅轴反方向换行

3.flex-flow: wrap 和 direction 的简写属性
flex-flow: row wrap;

4.justify-content: 主轴上的元素的对齐方式（如何分配主轴上的空白空间）
  可选值：
    flex-start 元素沿着主轴起边排列
    flex-end 元素沿着主轴终边排列
    center 元素居中排列
    space-around 空白分布到元素两侧
    space-between 空白均匀分布到元素间(两边贴边)
    space-evenly 空白分布到元素的单侧


实例:
ul{
    width: 800px;
    border: 10px red solid;
    /* 设置ul为弹性容器 */
    display: flex;
	
    flex-direction: column;
	/* flex-wrap: 设置弹性元素是否在弹性容器中自动换行 */
    flex-wrap: wrap-reverse;

    /* flex-flow:  wrap 和 direction 的简写属性 */
    flex-flow: row wrap;

    /*  justify-content 如何分配主轴上的空白空间（主轴上的元素如何排列） */
    /* justify-content: center; */
}

li{
    width: 200px;
    height: 100px;
    background-color: #bfa;
    font-size: 50px;
    text-align: center;
    line-height: 100px;
    flex-shrink: 0;
}
/* li:nth-child(1){
} */

li:nth-child(2){
    background-color: pink;
}

li:nth-child(3){
    background-color: orange;
}
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>


5.align-items: 
- 元素在辅轴上的对齐方式
  可选值：
    stretch 默认值，拉伸，将元素的长度设置为相同的值
		注意：子元素不能设高度，不然无效！
    flex-start 元素不会拉伸，沿着辅轴起边对齐
    flex-end 沿着辅轴的终边对齐
    center 居中对齐
    baseline 基线对齐

- 元素在弹性盒中水平垂直居中，设置如下：
    justify-content: center;
    align-items: center;

6.align-content: 元素在辅轴上的对齐方式（多行元素）
- 如果元素只有一根轴线，该属性不起作用。即必须有多行的元素！
- 可选值 跟 justify-content 类似:
	flex-start | flex-end | center | space-between | space-around | stretch;


实例:
ul {
    width: 600px;
    height: 800px;
    border: 10px red solid;
    /* 设置ul为弹性容器 */
    display: flex;

    /* flex-flow:  wrap 和 direction 的简写属性 */
    flex-flow: row wrap;

    /*
    align-items: 
        - 元素在辅轴上如何对齐
        - 元素间的关系
    */

    /* justify-content: center; */
    align-items: flex-start;

    /* align-content: 辅轴空白空间的分布，跟justify-content类似*/
    align-content: space-between;
}

li {
    width: 200px;
    background-color: #bfa;
    font-size: 50px;
    text-align: center;
    line-height: 100px;
    flex-shrink: 0;
}
li:nth-child(1) {
    /* align-self: 用来覆盖当前弹性元素上的align-items */
    align-self: stretch;
}

li:nth-child(2) {
    background-color: pink;
}

li:nth-child(3) {
    background-color: orange;
}

li:nth-child(4) {
    background-color: yellow;
}

li:nth-child(5) {
    background-color: chocolate;
}
<ul>
    <li>1</li>
    <li>
        2
        <div>2</div>
    </li>
    <li>
       	3
        <div>3</div>
        <div>3</div>
    </li>
    <li>1</li>
    <li>
        2
        <div>2</div>
    </li>
</ul>
```

## 3.弹性元素的样式

```css
1.flex-grow 指定弹性元素的伸展系数
    - 当父元素有多余空间的时，子元素如何伸展
    - 父元素的剩余空间，会按照比例进行分配

2.flex-shrink 指定弹性元素的收缩系数
    - 当父元素中的空间不足以容纳所有的子元素时，如果对子元素进行收缩
	- 缩减系数的计算方式比较复杂
    - 缩减多少是根据 缩减系数 和 元素大小来计算

3.flex-basis 指定的是元素在主轴上的基础长度
    - 如果主轴是 横向的, 则 该值指定的就是元素的宽度
    - 如果主轴是 纵向的, 则 该值指定的是就是元素的高度
    - 默认值是 auto，表示参考元素自身的高度或宽度
    - 如果传递了一个具体的数值，则以该值为准

4.flex 可以设置弹性元素所有的三个样式
  flex: 增长 缩减 基础;
    可选值：
        initial "flex: 0 1 auto", 默认值。
        auto  "flex: 1 1 auto"
        none "flex: 0 0 auto"  弹性元素没有弹性

5.order 决定弹性元素的排列顺序

6.align-self: 用来覆盖当前弹性元素上的align-items
可选值 跟 align-items 类似:
	flex-start | flex-end | center | baseline | stretch;


实例:
ul{
    width: 900px;
    border: 10px red solid;
    /* 设置弹性盒 */
    display: flex;
}

li{
    width: 200px;
    height: 100px;
    background-color: #bfa;
    font-size: 50px;
    text-align: center;
    line-height: 100px;

    /* 弹性的增长系数 */
    flex-grow: 1;

    /* 弹性元素的缩减系数 */
    flex-shrink: 1;

    /* flex-basis 指定的是元素在主轴上的基础长度 */
    /* flex-basis: 150px; */

    /*
        flex 可以设置弹性元素所有的三个样式
        flex: 增长 缩减 基础;
    */
    flex: initial;

}
li:nth-child(1){
    /* order 决定弹性元素的排列顺序 */
    order: 2;
}

li:nth-child(2){
    background-color: pink;
    /* flex-grow: 2; */
    order: 3;
}

li:nth-child(3){
    background-color: orange;
    /* flex-grow: 3; */
    order: 1;
}
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
```

## 案例1：淘宝导航

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }

    /* 设置外层的容器 */
    .nav{
        width: 100%;
    }

    /* 设置每一行的容器 */
    .nav-inner{
        /* 设置为弹性容器 */
        display: flex;
        /* 设置主轴上空白的分布 */
        justify-content: space-around;
    }

    .item{
        width: 18%;
        /* background-color: #bfa; */
        /* flex: auto; */
        text-align: center;
    }

    .item img{
        /* 设置图片的宽度和父元素宽度一样 */
        width: 100%;
    }

    .item a{
        color: #333;
        text-decoration: none;
        font-size: 16px;
    }
</style>

<!-- 创建一个外层的容器 -->
<nav class="nav">
    <div class="nav-inner">
        <div class="item">
            <a href="#">
                <img src="img/16/1.png">
                <span>天猫</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/2.png">
                <span>聚划算</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/3.png">
                <span>天猫国际</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/4.png">
                <span>外卖</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/5.png">
                <span>天猫超市</span>
            </a>
        </div>
    </div>
    <div class="nav-inner">
        <div class="item">
            <a href="#">
                <img src="img/16/6.png">
                <span>充值中心</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/7.png">
                <span>飞猪旅行</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/8.png">
                <span>领金币</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/9.png">
                <span>拍卖</span>
            </a>
        </div>
        <div class="item">
            <a href="#">
                <img src="img/16/10.png">
                <span>分类</span>
            </a>
        </div>
    </div>
</nav>
```

## 4.媒体查询

+ 用 @media开头 注意@符号
+ mediatype  媒体类型
+ 关键字 and  not  only
+ media feature 媒体特性必须有小括号包含

```
@media mediatype and|not|only (media feature) {
    CSS-Code;
}
```

1.mediatype  媒体类型：

- all 所有设备

- print 打印设备

- screen 带屏幕的设备

- speech 屏幕阅读器

	- 可以使用,连接多个媒体类型，这样它们之间就是一个或的关系

2.关键字

​	关键字将媒体类型或多个媒体特性连接到一起做为媒体查询的条件。

- and：可以将多个媒体特性连接到一起，相当于“且”的意思。
- not：排除某个媒体类型，相当于“非”的意思，可以省略。
- only：指定某个特定的媒体类型，可以省略。
- 注：,连接的是或关系，and连接的是与的关系

3.媒体特性：

- width 视口的宽度

- height 视口的高度

- min-width 视口的最小宽度（视口大于指定宽度时生效）

- max-width 视口的最大宽度（视口小于指定宽度时生效）

样式切换的分界点，我们称其为断点，也就是网页的样式会在这个点时发生变化

一般比较常用的断点：

- 小于768 超小屏幕 max-width=768px

- 大于768 小屏幕  min-width=768px

- 大于992 中型屏幕 min-width=992px

- 大于1200 大屏幕 min-width=1200px

```css
@media only screen and (min-width: 500px) and (max-width:700px){
    body{
        background-color: #bfa;
    }
}
```



