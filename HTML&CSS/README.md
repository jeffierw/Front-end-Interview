# 1 - HTML&CSS

## 1. HTML

### 1. 如何理解HTML语义化？

可根据一个HTML例子来描述下面的两个特点：

一种HTML全部使用div，另一种使用**header nav section footer**等语义化标签布局

- 让人更容易读懂（增加代码可读性）
- 让搜索引擎更容易读懂（SEO）

### 2. 默认情况下，哪些HTML标签是块级元素、哪些是内联元素？

块级元素独占一行，内联元素只占据元素本身大小。

- display: block/table;有 div h1 h2 table ul ol p等标签
- display: inline/inline-block; 有span img input button等标签

 ## 2. CSS

> CSS部分可分为`布局`、`定位`、`图文样式`、`响应式`、`CSS3`等模块

 

### 布局

#### 1. 盒子模型的宽度如何计算？

分为`标准盒模型`和`IE盒模型`，两者的区别在于**content**的不同，IE盒模型的**content**包括**border**、**padding**，盒子宽度可以通过DOM的`offsetWidth`属性获取，Chrome浏览器默认标准盒模型，所以宽度包括content，padding，border；如果设置盒子的`box-sizing`属性为`border-box`，那么盒子的`width`属性就包括padding和border的值。

#### 2. margin纵向重叠的问题？

相邻元素的`margin-top`和`margin-bottom`属性会发生重叠，选择值更大的一方渲染。

#### 3. margin负值的问题？

- `margin-top` 和`margin-left`负值，元素向上、向左移动
- `margin-right`负值，右侧的相邻标签左移，自身不动；`margin-bottom`负值，下方相邻标签上移，自身不动。（类似**吸附相邻元素标签的效果）

#### 4.BFC的理解和应用

`Block format context`,**块级格式化上下文**； 表示一块独立渲染区域，内部标签的渲染不会影响边界以外的标签。

##### 形成BFC的常见条件：

- float不为none，(设置`float浮动`)
- position是`absolute`或`fixed`
- overflow不是`visible`
- display是`flex`、`inline-block`等 

##### BFC的常见应用：

- 清除浮动

#### 5. float布局的问题，以及clearfix

##### 5.1如何实现圣杯布局和双飞翼布局？

###### 圣杯布局和双飞翼布局的目的：

- 三栏布局，中间一栏最先加载和渲染（内容最重要）
- 两侧内容固定，中间内容随着宽度自适应
- 一般用于PC网页

 ###### 圣杯布局和双飞翼布局的技术要点：

- 使用float布局
- 两侧使用margin负值，以便和中间内容横向重叠
- 防止中间内容被两侧覆盖，可以用`padding`或者`margin`

###### clearfix

```css
/* 手写 clear fix */
.clearfix:after {
    content: '';
    display: table;
    clear: both;
}
.clearfix {
	*zoom: 1; /* 兼容 IE 低版本 */
}
```



#### 6. flex如何画一个三色骰子？

```css
.box {
	display: flex;
    justify-content: space-between;
}
.item {
    /* 背景色、大小、边框等 */
}
.item:nth-child(2) {
	align-self: center;
}
.item:nth-child(3) {
	align-self: flex-end;
}
```



### 定位

#### 1.absolute和relative分别依据什么定位？

- `relative`依据自身位置定位
- `absolute`依据最近一层的定位元素定位（定位元素包括 `absolute`，`relative`,`fixed`或者是`body`） 

#### 2. 居中问题

##### 2.1 水平居中

- `inline`元素： `text-align: center`
- `block`元素：`margin: auto`
- `absolute`元素：`left:50%` + `margin-left`负值（需要知道子元素宽度）

##### 2.2 垂直居中

- `inline`元素： `line-height`值等于`height`
- `absolute`元素：`top:50%` + `margin-top`负值（需要知道子元素高度）
- `absolute`元素：`transform(-50%,-50%)`
- `absolute`元素：`top,left,bottom,right = 0` + `margin: auto`

### 图文样式

#### 1. line-height的继承问题？

`Line-height`取值不同，继承效果不一样

- 具体数值，如16px,则继承16px

- 比例，如2 / 1.5，则继承该比例

- **百分比**，如200%，**则继承计算出来的值**

  

  例如：

```css
body {
	font-size: 20px;
    line-height: 200%;
}
.item {
    font-size: 16px; /* item的行高不是36px 而是40px */
}
```

### 响应式

#### 1. rem是什么？

`rem`是一个长度单位，相对于根元素`html`,常用于响应式布局

- `px`，绝对长度单位，最常用
- `em`，相对长度单位，相对于父元素，不常用
- `rem`，相对长度单位，相对于根元素，常用于响应式布局

#### 2. 如何实现响应式？

##### 2.1 媒体查询 + rem

- `media-query`，媒体查询，根据不同的屏幕宽度设置根元素的`font-size`

- `rem`,基于根元素的相对单位

##### 2.2 基于视口的vw/vh单位适配 + rem字体适应

首先了解一下网页视口尺寸：

- window.screen.height  屏幕高度
- window.innerHeight **网页视口高度**
- document.body.clientHeight body高度

而`vw`，`vh`作为视口单位，1vw表示**网页视口宽度**的1/100,1vh表示**网页视口高度**的1/100

使用前必须加上meta元素的视口申明：

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```

然后字体依然使用媒体查询配合rem。

### CSS3

#### 1. CSS3动画







