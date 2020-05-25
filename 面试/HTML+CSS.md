# HTML+CSS面试题

## HTML

### 1. HTML语义化

HTML语义化就是让页面内容结构化，它有如下优点

```
1、易于用户阅读，样式丢失的时候能让页面呈现清晰的结构。
2、有利于SEO，搜索引擎根据标签来确定上下文和各个关键字的权重。
3、方便其他设备解析，如盲人阅读器根据语义渲染网页
4、有利于开发和维护，语义化更具可读性，代码更好维护，与CSS3关系更和谐
复制代码
```

如：

```
<header>代表头部
<nav>代表超链接区域
<main>定义文档主要内容
<article>可以表示文章、博客等内容
<aside>通常表示侧边栏或嵌入内容
<footer>代表尾部
复制代码
```

### 2. HTML5新标签

```
有<header>、<footer>、<aside>、<nav>、<video>、<audio>、<canvas>等...
复制代码
```



## CSS

### 1. 盒子模型

盒模型分为标准盒模型和怪异盒模型(IE模型)

```
box-sizing：content-box   //标准盒模型
box-sizing：border-box    //怪异盒模型
复制代码
```



![cmd-markdown-logo](https://user-gold-cdn.xitu.io/2019/9/9/16d1528aff0ef536?imageslim)

标准盒模型：元素的宽度等于style里的width+margin+border+padding宽度





![cmd-markdown-logo](https://user-gold-cdn.xitu.io/2019/9/9/16d158cb1351e5b1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



> 如下代码，整个宽高还是120px

```
div{
    box-sizing: content-box;
    margin: 10px;
    width: 100px;
    height: 100px;
    padding: 10px;
}
复制代码
```

怪异盒模型：元素宽度等于style里的width宽度



![cmd-markdown-logo](https://user-gold-cdn.xitu.io/2019/9/9/16d158e2ebd5fefe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



> 如下代码，整个宽高还是100px

```
div{
    box-sizing: border-box;
    margin: 10px;
    width: 100px;
    height: 100px;
    padding: 10px;
}
复制代码
```

注意：如果你在设计页面中，发现内容区被撑爆了，那么就先检查一下border-sizing是什么，最好在引用reset.css的时候，就对border-sizing进行统一设置，方便管理

### 2. rem与em的区别

> rem是根据根的font-size变化，而em是根据父级的font-size变化

rem：相对于根元素html的font-size，假如html为font-size：12px，那么，在其当中的div设置为font-size：2rem,就是当中的div为24px

em：相对于父元素计算，假如某个p元素为font-size:12px,在它内部有个span标签，设置font-size：2em,那么，这时候的span字体大小为：12*2=24px

### 3. CSS选择器

**css常用选择器**

```
通配符：*
ID选择器：#ID
类选择器：.class
元素选择器：p、a    等
后代选择器：p span、div a   等
伪类选择器：a:hover 等
属性选择器：input[type="text"]  等
复制代码
```

**css选择器权重**

!important -> 行内样式 -> #id -> .class -> 元素和伪元素 -> * -> 继承 -> 默认

### 4. CSS新特性

```
transition：过渡
transform：旋转、缩放、移动或者倾斜
animation：动画
gradient：渐变
shadow：阴影
border-radius：圆角
复制代码
```

### 5. 行内元素和块级元素

**行内元素（display: inline）**

宽度和高度是由内容决定，与其他元素共占一行的元素，我们将其叫行内元素，例如：` 、  、 `等

**块级元素（display: block)**

默认宽度由父容器决定，默认高度由内容决定，独占一行并且可以设置宽高的元素，我们将其叫做块级元素，例如：` 、 、等`

在平时，我们经常使用CSS的display: inline-block，使它们拥有更多的状态

### 6. 绝对定位和相对定位的区别

**position: absolute**
 绝对定位：是相对于元素最近的已定位的祖先元素

**position: relative**
 相对定位：相对定位是相对于元素在文档中的初始位置

### 7. BFC

**什么是BFC?**

BFC格式化上下文，它是一个独立的渲染区域，让处于 BFC 内部的元素和外部的元素相互隔离，使内外元素的定位不会相互影响

**如何产生BFC?**

display: inline-block

position: absolute/fixed

**BFC作用**

BFC最大的一个作用就是：在页面上有一个独立隔离容器，容器内的元素和容器外的元素布局不会相互影响

```
解决上外边距重叠;重叠的两个box都开启bfc;
解决浮动引起高度塌陷;容器盒子开启bfc
解决文字环绕图片;左边图片div,右边文字容器p,将p容器开启bfc
复制代码
```

### 8. 水平垂直居中

**Flex布局**

```css
display: flex  //设置Flex模式
flex-direction: column  //决定元素是横排还是竖着排
flex-wrap: wrap     //决定元素换行格式
justify-content: space-between  //同一排下对齐方式，空格如何隔开各个元素
align-items: center     //同一排下元素如何对齐
align-content: space-between    //多行对齐方式
```

**水平居中**

```css
行内元素：display: inline-block;
块级元素：margin: 0 auto;
Flex: display: flex; justify-content: center
```

**垂直居中**

```css
行高 = 元素高：line-height: height
flex: display: flex; align-item: center
```

### 9. less,sass,styus三者的区别

**变量**

Sass声明变量必须是『$』开头，后面紧跟变量名和变量值，而且变量名和变量值需要使用冒号：分隔开。

Less 声明变量用『@』开头，其余等同 Sass。

Stylus 中声明变量没有任何限定，结尾的分号可有可无，但变量名和变量值之间必须要有『等号』。

**作用域**

Sass：三者最差，不存在全局变量的概念

Less：最近的一次更新的变量有效，并且会作用于全部的引用！

Stylus：Sass 的处理方式和 Stylus 相同，变量值输出时根据之前最近的一次定义计算，每次引用最近的定义有效；

**嵌套**

三种 css 预编译器的「选择器嵌套」在使用上来说没有任何区别，甚至连引用父级选择器的标记 & 也相同

**继承**

Sass和Stylus的继承非常像，能把一个选择器的所有样式继承到另一个选择器上。使用『@extend』开始，后面接被继承的选择器。Stylus 的继承方式来自 Sass，两者如出一辙。 Less 则又「独树一帜」地用伪类来描述继承关系；

**导入@Import**

Sass 中只能在使用 url() 表达式引入时进行变量插值

```
$device: mobile;
@import url(styles.#{$device}.css);
复制代码
```

Less 中可以在字符串中进行插值

```
@device: mobile;
@import "styles.@{device}.css";
复制代码
```

Stylus 中在这里插值不管用，但是可以利用其字符串拼接的功能实现

```
device = "mobile"
@import "styles." + device + ".css"
复制代码
```

**总结**

Sass和Less语法严谨、Stylus相对自由。因为Less长得更像 css，所以它可能学习起来更容易。

Sass 和 Compass、Stylus 和 Nib 都是好基友。

Sass 和 Stylus 都具有类语言的逻辑方式处理：条件、循环等，而 Less 需要通过When等关键词模拟这些功能，这方面 Less 比不上 Sass 和 Stylus

Less 在丰富性以及特色上都不及 Sass 和 Stylus，若不是因为 Bootstrap 引入了 Less，可能它不会像现在这样被广泛应用（个人愚见）

### 10. link与@import区别与选择

```html
<style type="text/css">
	@import url(CSS文件路径地址);
</style>
<link href="CSSurl路径" rel="stylesheet" type="text/css" /
```

link属于html标签，而@import是css提供的

link功能较多，可以定义 RSS，定义 Rel 等作用，而@import只能用于加载 css；

当解析到link时，页面会同步加载所引的 css，而@import所引用的 css 会等到页面加载完才被加载；

@import需要 IE5 以上才能使用；

link方式样式的权重高于@import的

### 11. 多行元素的文本省略号

```
overflow : hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical
```

### 12. transition和animation的区别

Animation和transition大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是transition需要触发一个事件才能改变属性，而animation不需要触发任何事件的情况下才会随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的

### 13. visibility=hidden, opacity=0，display:none

opacity=0，该元素隐藏起来了，但不会改变页面布局，并且，如果该元素已经绑定一些事件，如click事件，那么点击该区域，也能触发点击事件的visibility=hidden，该元素隐藏起来了，但不会改变页面布局，但是不会触发该元素已经绑定的事件display=none，把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删除掉一样