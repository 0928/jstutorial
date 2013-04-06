---
title: 面向对象编程概述
layout: page
category: oop
date: 2012-12-28
modifiedOn: 2013-04-06
---

## 构造函数

在C++和Java等面向对象编程的语言中，存在“类”（class）这样一个概念。所谓“类”就是对象的模板，对象就是“类”的实例。JavaScript语言中没有“类”，以构造函数（constructor）作为对象的模板。也就是说，可以用构造函数生成多个相同结构的对象。

所以，什么叫“构造函数”，就是可以作为对象的模板的函数。

它的最大特点就是，函数体内部使用了this关键字。

{% highlight javascript %}

var Vehicle = function() {
	this.price = 1000;
}

{% endhighlight %}

上面代码中的Vehicle就是一个构造函数，代表“车辆”对象的模板。函数体内部的this关键字，代表实例对象。this.price表示实例对象有一个price属性，它的值是1000。

## new命令

根据构造函数生成实例对象，需要使用new命令。

{% highlight javascript %}

var v = new Vehicle();

console.info(v.price);
// 1000

{% endhighlight %}

上面代码的变量v，就是新生成的实例对象。它从构造函数Vehicle继承了price属性。

new命令后面的构造函数可以带括号，也可以不带括号。下面两行代码是等价的。

{% highlight javascript %}

var v = new Vehicle();

var v = new Vehicle;

{% endhighlight %}

但是，如果要向构造函数传入参数，就只有使用第一种表示法了。

{% highlight javascript %}

var v = new Vehicle(1000);

{% endhighlight %}

## instanceof运算符

该运算符用来确定一个对象是否是另一个对象的实例。

{% highlight javascript %}

123 instanceof Object 
// false

[1, 2, 3] instanceof Array
// true

{} instanceof Object
// true

var f = function (){};
var o = new f();

o instanceof f
// true

{% endhighlight %}

## this关键字

上面说到，this关键字在构造函数中指的是实例对象。但是，this关键字有多种含义，实例对象只是其中一种。严格地说，this关键字指的是变量所处的上下文环境（context）。我们分成几种情况来讨论。

（1）在全局环境使用this，它指的就是顶层对象（浏览器环境中就是window）。

因此，下面三行命令是等价的。

{% highlight javascript %}

var a = 1;

window.a = 1;

this.a = 1;

{% endhighlight %}

在浏览器全局环境中，变量的顶层对象默认是window，这时this就表示window。

（2）从构造函数生成实例对象时，this代表实例对象。

{% highlight javascript %}

var O = function( p ) {
    this.p = p ;
};

O.prototype.m = function() {
    return this.p;
};

{% endhighlight %}

当使用new命令，生成一个O的实例时，this就代表这个实例。

{% highlight javascript %}

var o = new O("Hello World!");

o.m()
// "Hello World!"

{% endhighlight %}

（3）当函数被赋值给一个对象的属性，this就代表这个对象。

{% highlight javascript %}

var o1 = { m : 1 };

var o2 = { m : 2 };

o1.f = function(){ console.log(this.m);};
o1.f()
// 1

o2.f = o1.f;
o2.f()
// 2

{% endhighlight %}

可以看到，当f成为o1对象的方法时，this代表o1；当f成为o2对象的方法时，this代表o2。f所在的上下文环境，决定了this的指向。

综合上面三种情况，可以看到this就是运行时的上下文环境。如果在全局环境下运行，就代表全局对象；如果在某个对象中运行，就代表该对象。

这种不确定的this指向，会给编程带来一些麻烦。比如，对某个按钮指定click事件的回调函数，可以这样写：

{% highlight javascript %}

$( "#button" ).on( "click", function() {
    var $this = $( this );
});

{% endhighlight %}

这时，this代表这个按钮的DOM对象。但是，如果像下面这样写，就会出错：

{% highlight javascript %}

function f() {
    var $this = $( this );
}

$( "#button" ).on( "click", f );

{% endhighlight %}

这时，this代表全局对象。

为了解决这个问题，可以采用下面的一些方法对this进行绑定，也就是使得this固定指向某个对象，减少编程中的不确定性。

## call方法和apply方法

call方法和apply方法的作用，是指定函数运行的上下文对象。它们接受的第一个参数都是this所要指向的那个对象，如果该参数是null或undefined，则等同于指定全局对象。

{% highlight javascript %}

var n = 123;

var o = { n : 456 };

function a() {
    console.log(this.n);
}

a.call() // 123
a.apply() // 123

a.call(window) // 123
a.apply(window)// 123

a.call(o) // 456
a.apply(o) // 456

a.call(null) // 123
a.apply(null) // 123

{% endhighlight %}

call方法和apply方法的区别在于，call方法除了第一个参数以外，其他参数都会依次传入原函数，而apply方法的第二个参数是一个数组，该数组的所有成员依次作为参数，传入原函数。

因此，上一节按钮点击事件的例子，可以改写成

{% highlight javascript %}

var o = {
	p : 1,
	f : function() { console.log(this.p); } 
}

$( "#button" ).on( "click", o.f.call(o) );

// or

$( "#button" ).on( "click", o.f.apply(o) );

{% endhighlight %}

## bind方法

bind方法则是对原函数绑定上下文以后，返回一个新函数。

{% highlight javascript %}

var o = {
	p : 1,
	f : function() { console.log(this.p); } 
}

$( "#button" ).on( "click", o.f.bind(o) );

{% endhighlight %}

jQuery的$.proxy方法可以起到同样作用，同时对老式浏览器保持兼容。

{% highlight javascript %}

$( "#button" ).on( "click", $.prox（o.f，o) );

{% endhighlight %}

另一种常见的技巧，则是事先在函数体内部将this的值固定。

{% highlight javascript %

var o = {
	p : 1,
	self : this,
	f : function() { console.log(self.p); } 
}

$( "#button" ).on( "click", o.f );

{% endhighlight %}

## 参考链接

- Jonathan Creamer, [Avoiding the "this" problem in JavaScript](http://tech.pro/tutorial/1192/avoiding-the-this-problem-in-javascript) 
