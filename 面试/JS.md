# Javascript面试题

## JS基础

### 1. get请求传参长度的误区

***误区：我们经常说get请求参数的大小存在限制，而post请求的参数大小是无限制的。***

实际上HTTP 协议从未规定 GET/POST 的请求长度限制是多少。对get请求参数的限制是来源与浏览器或web服务器，浏览器或web服务器限制了url的长度。为了明确这个概念，我们必须再次强调下面几点:

- HTTP 协议 未规定 GET 和POST的长度限制
- GET的最大长度显示是因为 浏览器和 web服务器限制了 URI的长度
- 不同的浏览器和WEB服务器，限制的最大长度不一样
- 要支持IE，则最大长度为2083byte，若只支持Chrome，则最大长度 8182byte

### 2. 补充get和post请求在缓存方面的区别

post/get的请求区别，具体不再赘述。

补充补充一个get和post在缓存方面的区别：

- get请求类似于查找的过程，用户获取数据，可以不用每次都与数据库连接，所以可以使用缓存。
- post不同，post做的一般是修改和删除的工作，所以必须与数据库交互，所以不能使用缓存。因此get请求适合于请求缓存。

### 3. 闭包

一句话可以概括：闭包就是能够读取其他函数内部变量的函数，或者子函数在外调用，子函数所在的父函数的作用域不会被释放。

### 4. 类的创建和继承

（1）类的创建（es5）：new一个function，在这个function的prototype里面增加属性和方法。

下面来创建一个Animal类：

```
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
复制代码
```

这样就生成了一个Animal类，实力化生成对象后，有方法和属性。

（2）类的继承——原型链继承

```
--原型链继承
function Cat(){ }
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';
//&emsp;Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
复制代码
```

- 介绍：在这里我们可以看到new了一个空对象,这个空对象指向Animal并且Cat.prototype指向了这个空对象，这种就是基于原型链的继承。
- 特点：基于原型链，既是父类的实例，也是子类的实例
- 缺点：无法实现多继承

（3）构造继承：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）

```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
复制代码
```

- 特点：可以实现多继承
- 缺点：只能继承父类实例的属性和方法，不能继承原型上的属性和方法。

（4）实例继承和拷贝继承

实例继承：为父类实例添加新特性，作为子类实例返回

拷贝继承：拷贝父类元素上的属性和方法

上述两个实用性不强，不一一举例。

（5）组合继承：相当于构造继承和原型链继承的组合体。通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
复制代码
```

- 特点：可以继承实例属性/方法，也可以继承原型属性/方法
- 缺点：调用了两次父类构造函数，生成了两份实例

（6）寄生组合继承：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性

```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
复制代码
```

- 较为推荐

### 5. 如何解决异步回调地狱

promise、generator、async/await

### 6. 说说前端中的事件流

HTML中与javascript交互是通过事件驱动来实现的，例如鼠标点击事件onclick、页面的滚动事件onscroll等等，可以向文档或者文档中的元素添加事件侦听器来预订事件。想要知道这些事件是在什么时候进行调用的，就需要了解一下“事件流”的概念。

什么是事件流：事件流描述的是从页面中接收事件的顺序,DOM2级事件流包括下面几个阶段。

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段

**addEventListener**：**addEventListener** 是DOM2 级事件新增的指定事件处理程序的操作，这个方法接收3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。

**IE只支持事件冒泡**。

### 7. 如何让事件先冒泡后捕获

在DOM标准事件模型中，是先捕获后冒泡。但是如果要实现先冒泡后捕获的效果，对于同一个事件，监听捕获和冒泡，分别对应相应的处理函数，监听到捕获事件，先暂缓执行，直到冒泡事件被捕获后再执行捕获之间。

### 8. 事件委托

- 简介：事件委托指的是，不在事件的发生地（直接dom）上设置监听函数，而是在其父元素上设置监听函数，通过事件冒泡，父元素可以监听到子元素上事件的触发，通过判断事件发生元素DOM的类型，来做出不同的响应。
- 举例：最经典的就是ul和li标签的事件监听，比如我们在添加事件时候，采用事件委托机制，不会在li标签上直接添加，而是在ul父元素上添加。
- 好处：比较合适动态元素的绑定，新添加的子元素也会有监听函数，也可以有事件触发机制。

### 9. 图片的懒加载和预加载

- 预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。
- 懒加载：懒加载的主要目的是作为服务器前端的优化，减少请求数或延迟请求数。

两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。 懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。

### 10. mouseover和mouseenter的区别

- mouseover：当鼠标移入元素或其子元素都会触发事件，所以有一个重复触发，冒泡的过程。对应的移除事件是mouseout
- mouseenter：当鼠标移除元素本身（不包含元素的子元素）会触发事件，也就是不会冒泡，对应的移除事件是mouseleave

### 11. js的new操作符做了哪些事情

new 操作符新建了一个空对象，这个对象原型指向构造函数的prototype，执行构造函数后返回这个对象。

### 12.改变函数内部this指针的指向函数（bind，apply，call的区别）

- 通过apply和call改变函数的this指向，他们两个函数的第一个参数都是一样的表示要改变指向的那个对象，第二个参数，apply是数组，而call则是arg1,arg2...这种形式。
- 通过bind改变this作用域会返回一个新的函数，这个函数不会马上执行。

### 13. js的各种位置，比如clientHeight,scrollHeight,offsetHeight ,以及scrollTop, offsetTop,clientTop的区别？

- clientHeight：表示的是可视区域的高度，不包含border和滚动条
- offsetHeight：表示可视区域的高度，包含了border和滚动条
- scrollHeight：表示了所有区域的高度，包含了因为滚动被隐藏的部分。
- clientTop：表示边框border的厚度，在未指定的情况下一般为0
- scrollTop：滚动后被隐藏的高度，获取对象相对于由offsetParent属性指定的父坐标(css定位的元素或body元素)距离顶端的高度。

### 14. js拖拽功能的实现

- 首先是三个事件，分别是mousedown，mousemove，mouseup 当鼠标点击按下的时候，需要一个tag标识此时已经按下，可以执行mousemove里面的具体方法。

- clientX，clientY标识的是鼠标的坐标，分别标识横坐标和纵坐标，并且我们用offsetX和offsetY来表示元素的元素的初始坐标，移动的举例应该是：

  **鼠标移动时候的坐标-鼠标按下去时候的坐标。**

  也就是说定位信息为：

  鼠标移动时候的坐标-鼠标按下去时候的坐标+元素初始情况下的offetLeft.

- 还有一点也是原理性的东西，也就是拖拽的同时是绝对定位，我们改变的是绝对定位条件下的left 以及top等等值。

补充：也可以通过html5的拖放（Drag 和 drop）来实现

### 15. 数组操作

> 在 JavaScript 中，用得较多的之一无疑是数组操作，这里过一遍数组的一些用法

```
map: 遍历数组，返回回调返回值组成的新数组
forEach: 无法break，可以用try/catch中throw new Error来停止
filter: 过滤
some: 有一项返回true，则整体为true
every: 有一项返回false，则整体为false
join: 通过指定连接符生成字符串
push / pop: 末尾推入和弹出，改变原数组， 返回推入/弹出项【有误】
unshift / shift: 头部推入和弹出，改变原数组，返回操作项【有误】
sort(fn) / reverse: 排序与反转，改变原数组
concat: 连接数组，不影响原数组， 浅拷贝
slice(start, end): 返回截断后的新数组，不改变原数组
splice(start, number, value...): 返回删除元素组成的数组，value 为插入项，改变原数组
indexOf / lastIndexOf(value, fromIndex): 查找数组项，返回对应的下标
reduce / reduceRight(fn(prev, cur)， defaultPrev): 两两执行，prev 为上次化简函数的return值，cur 为当前值(从第二项开始)
```



## JS高级

### 1. 闭包

**什么是闭包？**

函数A 里面包含了 函数B，而 函数B 里面使用了 函数A 的变量，那么 函数B 被称为闭包。

又或者：闭包就是能够读取其他函数内部变量的函数

```
function A() {
  var a = 1;
  function B() {
    console.log(a);
  }
  return B();
}
复制代码
```

**闭包的特征**

- 函数内再嵌套函数
- 内部函数可以引用外层的参数和变量
- 参数和变量不会被垃圾回收制回收

**对闭包的理解**

使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念

闭包 的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中

闭包的另一个用处，是封装对象的私有属性和私有方法

**闭包的好处**

能够实现封装和缓存等

**闭包的坏处**

就是消耗内存、不正当使用会造成内存溢出的问题

**使用闭包的注意点**

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露

解决方法是：在退出函数之前，将不使用的局部变量全部删除

**闭包的经典问题**

```
for(var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
复制代码
```

这段代码输出

```
答案：3个3
解析：首先，for 循环是同步代码，先执行三遍 for，i 变成了 3；然后，再执行异步代码 setTimeout，这时候输出的 i，只能是 3 个 3 了
复制代码
```

有什么办法依次输出0 1 2

> 第一种方法

使用let

```
for(let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
复制代码
```

在这里，每个 let 和代码块结合起来形成块级作用域，当 setTimeout() 打印时，会寻找最近的块级作用域中的 i，所以依次打印出 0 1 2

如果这样不明白，我们可以执行下边这段代码

```
for(let i = 0; i < 3; i++) {
  console.log("定时器外部：" + i);
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
复制代码
```

此时浏览器依次输出的是：

```
定时器外部：0
定时器外部：1
定时器外部：2
0
1
2
复制代码
```

即代码还是先执行 for 循环，但是当 for 结束执行到了 setTimeout 的时候，它会做个标记，这样到了 console.log(i) 中，i 就能找到这个块中最近的变量定义

> 第二种方法

使用立即执行函数解决闭包的问题

```
for(let i = 0; i < 3; i++) {
  (function(i){
    setTimeout(function() {
      console.log(i);
    }, 1000);
  })(i)
}
复制代码
```

### 2. JS作用域及作用域链

**作用域**

在JavaScript中，作用域分为 全局作用域 和 函数作用域

> 全局作用域

代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域

> 函数作用域

在固定的代码片段才能被访问

例子：

![cmd-markdown-logo](https://user-gold-cdn.xitu.io/2019/9/16/16d382c7d68e17ae?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

作用域有上下级关系，上下级关系的确定就看函数是在哪个作用域下创建的。如上，fn作用域下创建了bar函数，那么“fn作用域”就是“bar作用域”的上级。



作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

变量取值：到创建 这个变量 的函数的作用域中取值

**作用域链**

一般情况下，变量取值到 创建 这个变量 的函数的作用域中取值。

但是如果在当前作用域中没有查到值，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链

![cmd-markdown-logo](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="465" height="307"></svg>)



### 3. 原型和原型链

**原型和原型链的概念**

每个对象都会在其内部初始化一个属性，就是prototype(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去

**原型和原型链的关系**

```
instance.constructor.prototype = instance.__proto__
复制代码
```

**原型和原型链的特点**

JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变

当我们需要一个属性的时，Javascript引擎会先看当前对象中是否有这个属性， 如果没有的

就会查找他的Prototype对象是否有这个属性，如此递推下去，一直检索到 Object 内建对象

### 4. 对this对象的理解

this总是指向函数的直接调用者（而非间接调用者）

如果有new关键字，this指向new出来的那个对象

### 5. 自己实现一个bind函数

**原理：通过apply或者call方法来实现**

#### (1)初始版本

```
Function.prototype.bind=function(obj,arg){
  var arg=Array.prototype.slice.call(arguments,1);
  var context=this;
  return function(newArg){
    arg=arg.concat(Array.prototype.slice.call(newArg));
    return context.apply(obj,arg);
  }
}
复制代码
```

#### (2) 考虑到原型链

***为什么要考虑？因为在new 一个bind过生成的新函数的时候，必须的条件是要继承原函数的原型\***

```
Function.prototype.bind=function(obj,arg){
  var arg=Array.prototype.slice.call(arguments,1);
  var context=this;
  var bound=function(newArg){
    arg=arg.concat(Array.prototype.slice.call(newArg));
    return context.apply(obj,arg);
  }
  var F=function(){}
  //这里需要一个寄生组合继承
  F.prototype=context.prototype;
  bound.prototype=new F();
  return bound;
}
复制代码
```

### 6. 用setTimeout来实现setInterval

#### (1)用setTimeout()方法来模拟setInterval()与setInterval()之间的什么区别？

首先来看setInterval的缺陷，使用setInterval()创建的定时器确保了定时器代码规则地插入队列中。这个问题在于：如果定时器代码在代码再次添加到队列之前还没完成执行，结果就会导致定时器代码连续运行好几次。而之间没有间隔。不过幸运的是：javascript引擎足够聪明，能够避免这个问题。当且仅当没有该定时器的如何代码实例时，才会将定时器代码添加到队列中。这确保了定时器代码加入队列中最小的时间间隔为指定时间。

这种重复定时器的规则有两个问题：***1.某些间隔会被跳过 2.多个定时器的代码执行时间可能会比预期小。\***

下面举例子说明：

假设，某个onclick事件处理程序使用啦setInterval()来设置了一个200ms的重复定时器。如果事件处理程序花了300ms多一点的时间完成。

![2018-07-10 11 36 43](https://user-gold-cdn.xitu.io/2018/7/10/1648412f2eb805ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

这个例子中的第一个定时器是在205ms处添加到队列中，但是要过300ms才能执行。在405ms又添加了一个副本。在一个间隔，605ms处，第一个定时器代码还在执行中，而且队列中已经有了一个定时器实例，结果是605ms的定时器代码不会添加到队列中。结果是在5ms处添加的定时器代码执行结束后，405处的代码立即执行。

```
function say(){
  //something
  setTimeout(say,200);
}
setTimeout(say,200)
复制代码
```

或者

```
setTimeout(function(){
   //do something
   setTimeout(arguments.callee,200);
},200);
复制代码
```

### 7. js怎么控制一次加载一张图片，加载完后再加载下一张

#### (1)方法1

```
<script type="text/javascript">
var obj=new Image();
obj.src="http://www.phpernote.com/uploadfiles/editor/201107240502201179.jpg";
obj.onload=function(){
alert('图片的宽度为：'+obj.width+'；图片的高度为：'+obj.height);
document.getElementById("mypic").innnerHTML="<img src='"+this.src+"' />";
}
</script>
<div id="mypic">onloading……</div>
复制代码
```

#### (2)方法2

```
<script type="text/javascript">
var obj=new Image();
obj.src="http://www.phpernote.com/uploadfiles/editor/201107240502201179.jpg";
obj.onreadystatechange=function(){
if(this.readyState=="complete"){
alert('图片的宽度为：'+obj.width+'；图片的高度为：'+obj.height);
document.getElementById("mypic").innnerHTML="<img src='"+this.src+"' />";
}
}
</script>
<div id="mypic">onloading……</div>
复制代码
```

### 8. 代码的执行顺序

```
setTimeout(function(){console.log(1)},0);
new Promise(function(resolve,reject){
   console.log(2);
   resolve();
}).then(function(){console.log(3)
}).then(function(){console.log(4)});

process.nextTick(function(){console.log(5)});

console.log(6);
//输出2,6,5,3,4,1
复制代码
```

为什么呢？具体请参考我的文章：[ 从promise、process.nextTick、setTimeout出发，谈谈Event Loop中的Job queue](https://github.com/forthealllight/blog/issues/5)

### 9. 如何实现sleep的效果（es5或者es6）

#### (1)while循环的方式

```
function sleep(ms){
   var start=Date.now(),expire=start+ms;
   while(Date.now()<expire);
   console.log('1111');
   return;
}
复制代码
```

执行sleep(1000)之后，休眠了1000ms之后输出了1111。上述循环的方式缺点很明显，容易造成死循环。

#### (2)通过promise来实现

```
function sleep(ms){
  var temple=new Promise(
  (resolve)=>{
  console.log(111);setTimeout(resolve,ms)
  });
  return temple
}
sleep(500).then(function(){
   //console.log(222)
})
//先输出了111，延迟500ms后输出222
复制代码
```

#### (3)通过async封装

```
function sleep(ms){
  return new Promise((resolve)=>setTimeout(resolve,ms));
}
async function test(){
  var temple=await sleep(1000);
  console.log(1111)
  return temple
}
test();
//延迟1000ms输出了1111
复制代码
```

\####(4).通过generate来实现

```
function* sleep(ms){
   yield new Promise(function(resolve,reject){
             console.log(111);
             setTimeout(resolve,ms);
        })  
}
sleep(500).next().value.then(function(){console.log(2222)})
复制代码
```

### 10.Function.proto(getPrototypeOf)是什么？

获取一个对象的原型，在chrome中可以通过__proto__的形式，或者在ES6中可以通过Object.getPrototypeOf的形式。

那么Function.proto是什么么？也就是说Function由什么对象继承而来，我们来做如下判别。

```
Function.__proto__==Object.prototype //false
Function.__proto__==Function.prototype//true
复制代码
```

我们发现Function的原型也是Function。

我们用图可以来明确这个关系：

![2018-07-10 2 38 27](https://user-gold-cdn.xitu.io/2018/7/10/1648412f2ed72c1d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 11. 实现js中所有对象的深度克隆（包装对象，Date对象，正则对象）

通过递归可以简单实现对象的深度克隆，但是这种方法不管是ES6还是ES5实现，都有同样的缺陷，就是只能实现特定的object的深度复制（比如数组和函数），不能实现包装对象Number，String ， Boolean，以及Date对象，RegExp对象的复制。

#### (1)前文的方法

```
function deepClone(obj){
    var newObj= obj instanceof Array?[]:{};
    for(var i in obj){
       newObj[i]=typeof obj[i]=='object'?  
       deepClone(obj[i]):obj[i];    
    }
    return newObj;
}
复制代码
```

这种方法可以实现一般对象和数组对象的克隆，比如：

```
var arr=[1,2,3];
var newArr=deepClone(arr);
// newArr->[1,2,3]

var obj={
   x:1,
   y:2
}
var newObj=deepClone(obj);
// newObj={x:1,y:2}
复制代码
```

但是不能实现例如包装对象Number,String,Boolean,以及正则对象RegExp和Date对象的克隆，比如：

```
//Number包装对象
var num=new Number(1);
typeof num // "object"

var newNum=deepClone(num);
//newNum ->  {} 空对象

//String包装对象
var str=new String("hello");
typeof str //"object"

var newStr=deepClone(str);
//newStr->  {0:'h',1:'e',2:'l',3:'l',4:'o'};

//Boolean包装对象
var bol=new Boolean(true);
typeof bol //"object"

var newBol=deepClone(bol);
// newBol ->{} 空对象

....
复制代码
```

#### (2)valueof()函数

所有对象都有valueOf方法，valueOf方法对于：如果存在任意原始值，它就默认将对象转换为表示它的原始值。对象是复合值，而且大多数对象无法真正表示为一个原始值，因此默认的valueOf()方法简单地返回对象本身，而不是返回一个原始值。数组、函数和正则表达式简单地继承了这个默认方法，调用这些类型的实例的valueOf()方法只是简单返回这个对象本身。

对于原始值或者包装类：

```
function baseClone(base){
 return base.valueOf();
}

//Number
var num=new Number(1);
var newNum=baseClone(num);
//newNum->1

//String
var str=new String('hello');
var newStr=baseClone(str);
// newStr->"hello"

//Boolean
var bol=new Boolean(true);
var newBol=baseClone(bol);
//newBol-> true
复制代码
```

其实对于包装类，完全可以用=号来进行克隆，其实没有深度克隆一说，

这里用valueOf实现，语法上比较符合规范。

对于Date类型：

因为valueOf方法，日期类定义的valueOf()方法会返回它的一个内部表示：1970年1月1日以来的毫秒数.因此我们可以在Date的原型上定义克隆的方法：

```
Date.prototype.clone=function(){
  return new Date(this.valueOf());
}

var date=new Date('2010');
var newDate=date.clone();
// newDate->  Fri Jan 01 2010 08:00:00 GMT+0800 
复制代码
```

对于正则对象RegExp：

```
RegExp.prototype.clone = function() {
var pattern = this.valueOf();
var flags = '';
flags += pattern.global ? 'g' : '';
flags += pattern.ignoreCase ? 'i' : '';
flags += pattern.multiline ? 'm' : '';
return new RegExp(pattern.source, flags);
};

var reg=new RegExp('/111/');
var newReg=reg.clone();
//newReg->  /\/111\//
复制代码
```

### 11. 简单实现Node的Events模块

简介：观察者模式或者说订阅模式，它定义了对象间的一种一对多的关系，让多个观察者对象同时监听某一个主题对象，当一个对象发生改变时，所有依赖于它的对象都将得到通知。

node中的Events模块就是通过观察者模式来实现的：

```
var events=require('events');
var eventEmitter=new events.EventEmitter();
eventEmitter.on('say',function(name){
    console.log('Hello',name);
})
eventEmitter.emit('say','Jony yu');
复制代码
```

这样，eventEmitter发出say事件，通过On接收，并且输出结果，这就是一个订阅模式的实现，下面我们来简单的实现一个Events模块的EventEmitter。

#### (1)实现简单的Event模块的emit和on方法

```
function Events(){
this.on=function(eventName,callBack){
  if(!this.handles){
    this.handles={};
  }
  if(!this.handles[eventName]){
    this.handles[eventName]=[];
  }
  this.handles[eventName].push(callBack);
}
this.emit=function(eventName,obj){
   if(this.handles[eventName]){
     for(var i=0;o<this.handles[eventName].length;i++){
       this.handles[eventName][i](obj);
     }
   }
}
return this;
}
复制代码
```

这样我们就定义了Events，现在我们可以开始来调用：

```
 var events=new Events();
 events.on('say',function(name){
    console.log('Hello',nama)
 });
 events.emit('say','Jony yu');
 //结果就是通过emit调用之后，输出了Jony yu
复制代码
```

#### (2)每个对象是独立的

因为是通过new的方式，每次生成的对象都是不相同的，因此：

```
var event1=new Events();
var event2=new Events();
event1.on('say',function(){
    console.log('Jony event1');
});
event2.on('say',function(){
    console.log('Jony event2');
})
event1.emit('say');
event2.emit('say');
//event1、event2之间的事件监听互相不影响
//输出结果为'Jony event1' 'Jony event2'
复制代码
```

### 12. 箭头函数中this指向举例

```
var a=11;
function test2(){
  this.a=22;
  let b=()=>{console.log(this.a)}
  b();
}
var x=new test2();
//输出22
复制代码
```

### 13. 组件化和模块化

**组件化**

**为什么要组件化开发**

有时候页面代码量太大，逻辑太多或者同一个功能组件在许多页面均有使用，维护起来相当复杂，这个时候，就需要组件化开发来进行功能拆分、组件封装，已达到组件通用性，增强代码可读性，维护成本也能大大降低

**组件化开发的优点**

很大程度上降低系统各个功能的耦合性，并且提高了功能内部的聚合性。这对前端工程化及降低代码的维护来说，是有很大的好处的，耦合性的降低，提高了系统的伸展性，降低了开发的复杂度，提升开发效率，降低开发成本

**组件化开发的原则**

- 专一
- 可配置性
- 标准性
- 复用性
- 可维护性

**模块化**

**为什么要模块化**

早期的javascript版本没有块级作用域、没有类、没有包、也没有模块，这样会带来一些问题，如复用、依赖、冲突、代码组织混乱等，随着前端的膨胀，模块化显得非常迫切

**模块化的好处**

- 避免变量污染，命名冲突
- 提高代码复用率
- 提高了可维护性
- 方便依赖关系管理

**模块化的几种方法**

- 函数封装

```
var myModule = {
    var1: 1,
    
    var2: 2,
    
    fn1: function(){
    
    },
    
    fn2: function(){
    
    }
}
复制代码
总结：这样避免了变量污染，只要保证模块名唯一即可，同时同一模块内的成员也有了关系

缺陷：外部可以睡意修改内部成员，这样就会产生意外的安全问题
复制代码
```

- 立即执行函数表达式(IIFE)

```
var myModule = (function(){
    var var1 = 1;
    var var2 = 2;
    
    function fn1(){
    
    } 
    
    function fn2(){
    
    }

return {
    fn1: fn1,
    fn2: fn2
};
})();
复制代码
总结：这样在模块外部无法修改我们没有暴露出来的变量、函数

缺点：功能相对较弱，封装过程增加了工作量，仍会导致命名空间污染可能、闭包是有成本的
复制代码
```



## ES6

### 1. var、let、const之间的区别

var声明变量可以重复声明，而let不可以重复声明

var是不受限于块级的，而let是受限于块级

var会与window相映射（会挂一个属性），而let不与window相映射

var可以在声明的上面访问变量，而let有暂存死区，在声明的上面访问变量会报错

const声明之后必须赋值，否则会报错

const定义不可变的量，改变了就会报错

const和let一样不会与window相映射、支持块级作用域、在声明的上面访问变量会报错

### 2. 解构赋值

**数组解构**

```
let [a, b, c] = [1, 2, 3]   //a=1, b=2, c=3
let [d, [e], f] = [1, [2], 3]    //嵌套数组解构 d=1, e=2, f=3
let [g, ...h] = [1, 2, 3]   //数组拆分 g=1, h=[2, 3]
let [i,,j] = [1, 2, 3]   //不连续解构 i=1, j=3
let [k,l] = [1, 2, 3]   //不完全解构 k=1, l=2
复制代码
```

**对象解构**

```
let {a, b} = {a: 'aaaa', b: 'bbbb'}      //a='aaaa' b='bbbb'
let obj = {d: 'aaaa', e: {f: 'bbbb'}}
let {d, e:{f}} = obj    //嵌套解构 d='aaaa' f='bbbb'
let g;
(g = {g: 'aaaa'})   //以声明变量解构 g='aaaa'
let [h, i, j, k] = 'nice'    //字符串解构 h='n' i='i' j='c' k='e'
复制代码
```

**函数参数的定义**

一般我们在定义函数的时候，如果函数有多个参数时，在es5语法中函数调用时参数必须一一对应，否则就会出现赋值错误的情况，来看一个例子：

```
function personInfo(name, age, address, gender) {
  console.log(name, age, address, gender)
}
personInfo('william', 18, 'changsha', 'man')
复制代码
```

上面这个例子在对用户信息的时候需要传递四个参数，且需要一一对应，这样就会极易出现参数顺序传错的情况，从而导致bug，接下来来看es6解构赋值是怎么解决这个问题的：

```
function personInfo({name, age, address, gender}) {
  console.log(name, age, address, gender)
}
personInfo({gender: 'man', address: 'changsha', name: 'william', age: 18})
复制代码
```

这么写我们只知道要传声明参数就行来，不需要知道参数的顺序也没关系

**交换变量的值**

在es5中我们需要交换两个变量的值需要借助临时变量的帮助，来看一个例子：

```
var a=1, b=2, c
c = a
a = b
b = c
console.log(a, b)
复制代码
```

来看es6怎么实现：

```
let a=1, b=2;
[b, a] = [a, b]
console.log(a, b)
复制代码
```

是不是比es5的写法更加方便呢

**函数默认参数**

在日常开发中，经常会有这种情况：函数的参数需要默认值，如果没有默认值在使用的时候就会报错，来看es5中是怎么做的：

```
function saveInfo(name, age, address, gender) {
  name = name || 'william'
  age = age || 18
  address = address || 'changsha'
  gender = gender || 'man'
  console.log(name, age, address, gender)
}
saveInfo()
复制代码
```

在函数离 main先对参数做一个默认值赋值，然后再使用避免使用的过程中报错，再来看es6中的使用的方法：

```
function saveInfo({name= 'william', age= 18, address= 'changsha', gender= 'man'} = {}) {
  console.log(name, age, address, gender)
}
saveInfo()
复制代码
```

在函数定义的时候就定义了默认参数，这样就免了后面给参数赋值默认值的过程，是不是看起来简单多了

### 3. forEach、for in、for of三者区别

forEach更多的用来遍历数

for in 一般常用来遍历对象或json

for of数组对象都可以遍历，遍历对象需要通过和Object.keys()

for in循环出的是key，for of循环出的是value

### 4. 使用箭头函数应注意什么？

1、用了箭头函数，this就不是指向window，而是父级（指向是可变的）
 2、不能够使用arguments对象
 3、不能用作构造函数，这就是说不能够使用new命令，否则会抛出一个错误
 4、不可以使用yield命令，因此箭头函数不能用作 Generator 函数

### 5. Set、Map的区别

应用场景Set用于数据重组，Map用于数据储存

**Set：**
 1，成员不能重复
 2，只有键值没有键名，类似数组
 3，可以遍历，方法有add, delete,has

**Map:**
 1，本质上是健值对的集合，类似集合
 2，可以遍历，可以跟各种数据格式转换

### 6. promise对象的用法,手写一个promise

promise是一个构造函数，下面是一个简单实例

```js
var promise = new Promise((resolve,reject) => {
    if (操作成功) {
        resolve(value)
    } else {
        reject(error)
    }
})
promise.then(function (value) {
    // success
},function (value) {
    // failure
})
```