---
title: 面向对象编程概述
layout: page
category: oop
date: 2012-12-28
modifiedOn: 2013-11-12
---

## 简介

### 对象和面向对象编程

“面向对象编程”（Object Oriented Programming，缩写为OOP）是目前主流的编程方法。它的核心思想是将复杂的编程关系，抽象为一个个对象。

所谓“对象”（object），有两种理解方法。

（1）简单的理解：“对象”是单个实物的抽象。

一本书、一辆汽车、一个人都可以是“对象”，一个数据库、一张网页、一个与远程服务器的连接也可以是“对象”。当实物被抽象成“对象”，实物之间的关系就变成了“对象”之间的关系，从而就可以模拟现实情况，针对“对象”进行编程。

（2）复杂的理解：“对象”是一个容器，封装了相关的“属性”（property）和“方法”（method）。

所谓“属性”，就是对象的状态；所谓“方法”，就是对象的行为（完成某种任务）。比如，我们可以把动物抽象为animal对象，“属性”记录具体是那一种动物，“方法”表示动物的某种行为（奔跑、捕猎、休息等等）。

“面向对象编程”就是将“对象”作为程序的核心，所有的操作都是针对“对象”的。这种编程方法的最大优点，就是使得程序模块化，容易维护和开发。

本章介绍JavaScript如何进行“面向对象编程”。

### 类和构造函数

“面向对象编程”的第一步，就是要生成对象。

前面说过，“对象”是单个实物的抽象。所以，通常需要一个模板，表示某一类实物的共同特征，然后“对象”根据这个模板生成。

典型的面向对象编程语言（比如C++和Java），存在“类”（class）这样一个概念。所谓“类”就是对象的模板，对象就是“类”的实例。

JavaScript语言没有“类”，而改用构造函数（constructor）作为对象的模板。

所谓“构造函数”，就是专门用来生成“对象”的函数。它提供模板，作为对象的基本结构。一个构造函数，可以生成多个对象，这些对象都有相同的结构。

下面就是一个构造函数：

{% highlight javascript %}

var Vehicle = function() {
	this.price = 1000;
};

{% endhighlight %}

Vehicle就是构造函数，它提供模板，用来生成车辆对象。

构造函数的最大特点就是，函数体内部使用了this关键字，代表了所要生成的对象实例。

生成对象的时候，必需用new命令，调用Vehicle函数。

{% highlight javascript %}

var v = new Vehicle();

{% endhighlight %}

new命令的作用，就是让构造函数生成一个对象的实例。此时，构造函数内部的this代表被生成的实例对象，this.price表示实例对象有一个price属性，它的值是1000。

以上就是使用构造函数生成对象的最简单步骤。下面是详细的讲解。

## 基本用法

### new命令

new命令的作用，是让构造函数返回一个实例对象。我们可以把返回的实例对象其保存在一个变量中。

{% highlight javascript %}

var Vehicle = function() {
	this.price = 1000;
};

var v = new Vehicle();

v.price
// 1000

{% endhighlight %}

变量v就是新生成的实例对象，它从构造函数Vehicle继承了price属性。

new命令后面的构造函数可以带括号，也可以不带括号。下面两行代码是等价的。

{% highlight javascript %}

var v = new Vehicle();

var v = new Vehicle;

{% endhighlight %}

我们修改构造函数，使其可以带一个参数。

{% highlight javascript %}

var Vehicle = function(p) {
	this.price = p;
};

{% endhighlight %}

这时使用new命令，就需要同时提供参数值。

{% highlight javascript %}

var v = new Vehicle(500);

{% endhighlight %}

一个很自然的问题是，如果忘了使用new命令，直接调用构造函数会发生什么事？

这种情况下，构造函数就变成了普通函数，并不会生成实例对象。而且由于下面会说到的原因，this这时代表全局对象，将造成一些意想不到的结果。因此，应该避免出现不使用new命令、直接调用构造函数的情况。

### instanceof运算符

instanceof运算符用来确定一个对象是否为某个构造函数的实例。

{% highlight javascript %}

var v = new Vehicle();

v instanceof Vehicle
// true

{% endhighlight %}

前面章节说过，JavaScript的所有值，都是某种对象。只要是对象，就有对应的构造函数。因此，instanceof运算符可以用来判断值的类型。

{% highlight javascript %}

"" instanceof String // false

[1, 2, 3] instanceof Array // true

{} instanceof Object // true

{% endhighlight %}

上面代码表示字符串不是String对象的实例，而数组和对象则分别是Array对象和Object对象的实例。

## this关键字

### 涵义

this关键字有多种含义，下面就是详细解释。

JavaScript语言的所有变量都存在于某个对象之下，要知道一个变量确切的值，必须知道它属于哪个对象。

{% highlight javascript %}

var a = new Object();
var b = new Object();

a.m = 1;
b.m = 2;

{% endhighlight %}

上面代码先定义了两个对象a和b，这两个对象各有一个名为m的属性，要想知道m确切的值，就必须它属于a对象，还是属于b对象。

JavaScript语言的特点是，某个变量到底属于哪个对象是动态的，可以在运行时切换。完全有可能，m一会属于a对象，一会又属于b对象。

this关键字就是指变量所处的那个对象（即this.m），由于变量的所处对象是动态的，所以this关键字的值也是动态的。如果变量处在全局环境，this就是指全局环境的顶层对象（在浏览器中为window对象）；如果变量不处在全局环境，this就是指对变量求值时所在的对象。

我们分成几种情况来讨论。

**（1）全局环境**

在全局环境使用this，它指的就是顶层对象window。

{% highlight javascript %}

this === window // true

{% endhighlight %}

在浏览器全局环境中，顶层对象就是window，所以this等于window。

**（2）构造函数**

构造函数中的this，指的是实例对象。

{% highlight javascript %}

var O = function(p) {
	this.p = p;
};

O.prototype.m = function() {
    return this.p;
};

{% endhighlight %}

上面代码定义了一个构造函数O。由于this指向实例对象，所以在构造函数内部定义this.p，就相当于定义实例对象有一个p属性；然后m方法可以返回这个p属性。

{% highlight javascript %}

var o = new O("Hello World!");

o.p // "Hello World!"

o.m() // "Hello World!"

{% endhighlight %}

**（3）对象属性**

非构造函数内部的this，指的是函数运行时所在的对象。

{% highlight javascript %}

function f(){
	console.log(this.m);
}

var m = 1;
f() // 1

{% endhighlight %}

上面代码定义了一个函数f，当它在全局环境中运行，this的指向就是全局对象window，所以可以读取全局变量m的值。

{% highlight javascript %}

var o1 = { m : 1 };
o1.f = f;
o1.f() // 1


var o2 = { m : 2 };
o2.f = f;
o2.f() // 2

{% endhighlight %}

上面代码表示，当f在o1下运行时，this指向o1，当f在o2下运行时，this指向o2。

由于this取决于运行时所在的对象，所以如果将某个对象的方法赋值给另一个对象，会改变this的指向。这一点要特别小心。

{% highlight javascript %}

var o1 = new Object();
o1.m = 1;
o1.f = function (){ console.log(this.m);};

o1.f() // 1

var o2 = new Object();
o2.m = 2;
o2.f = o1.f

o2.f() // 2

{% endhighlight %}

从上面代码可以看到，f方法是o1的方法，但是如果在o2上面调用这个方法，f方法中的this就会指向o2。这就说明JavaScript函数的运行环境完全是动态绑定的，可以在运行时切换。

如果不想改变this的指向，可以将o2.f改写成下面这样。

{% highlight javascript %}

o2.f = function (){ o1.f() };

o2.f() // 1

{% endhighlight %}

上面代码表示，由于f方法这时是在o1下面运行，所以this就指向o1。

**（4）结论**

综合上面三种情况，可以看到this就是运行时变量或方法所在的对象。如果在全局环境下运行，就代表全局对象window；如果在某个对象中运行，就代表该对象。

由于this的指向是不确定的，所以切勿在函数中包含多层的this。

{% highlight javascript %}

var o = {
	f1: function() {
		console.log(this); 
		var f2 = function() {
			console.log(this);
		}();
	}
}

o.f1()
// Object
// Window

{% endhighlight %}

上面代码包含两层this，结果运行后，第一层指向该对象，第二层指向全局对象。一个解决方法是在第二层改用一个指向外层this的变量。

{% highlight javascript %}

var o = {
	f1: function() {
		console.log(this); 
		var that = this;
		var f2 = function() {
			console.log(that);
		}();
	}
}

o.f1()
// Object
// Object

{% endhighlight %}

上面代码定义了变量that，固定指向外层的this，然后在内层使用that，就不会发生this指向的改变。

这种不确定的this指向，会给编程带来一些麻烦。比如，使用jQuery函数库，对某个按钮指定click事件的回调函数，可以这样写：

{% highlight javascript %}

$( "#button" ).on( "click", function() {
    var $this = $( this );
});

{% endhighlight %}

这时，this指向这个按钮的DOM对象，因为是在按钮对象的环境中调用这个函数。但是，如果把代码改一改，this对象的指向就会发生变化。

{% highlight javascript %}

function f() {
    var $this = $( this );
}

$( "#button" ).on( "click", f );

{% endhighlight %}

函数f是定义在全局环境中的，如果单独运行f，函数体内部的this指向全局环境。但是，当f被指定为按钮点击事件的回调函数时，就变成了在按钮对象中运行，这时this就指向按钮对象。这种细微的差别，很容易在编程中忽视，而导致难以察觉的错误。

为了解决这个问题，可以采用下面的一些方法对this进行绑定，也就是使得this固定指向某个对象，减少编程中的不确定性。

### call方法

正常情况下，函数体内部的this关键字，总是指向函数运行时所在的那个对象。函数的call方法可以改变this指向的对象，然后再调用该函数。

{% highlight javascript %}

func.call(context, [arg1], [arg2], ...)

{% endhighlight %}

call方法的第一个参数就是this所要指向的那个对象，后面的参数则是函数调用时所需的参数。如果this所要指向的那个对象，设定为null或undefined，则等同于指定全局对象。

{% highlight javascript %}

var n = 123;

var o = { n : 456 };

function a() {
    console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(o) // 456

{% endhighlight %}

上面代码中，a函数中的this关键字，如果指向全局对象，返回结果为123。如果使用call方法，将this关键字指向o对象，返回结果为456。

### apply方法

apply方法的作用与call方法类似，也是改变this关键字指向的对象。区别在于，apply方法的第二个参数是一个数组，该数组的所有成员依次作为参数，传入原函数。

{% highlight javascript %}

func.apply(context, [arg1, arg2, ...])

{% endhighlight %}

原函数的参数，在call方法中必须一个个添加，但是在apply方法中，必须以数组形式添加。下面的例子是一个参数数组的例子。

{% highlight javascript %}

function f(x,y){ console.log(x+y);}

f.call(null,1,1)
// 2

// 改写为apply方法

f.apply(null,[1,1])
// 2

{% endhighlight %}

上面的f函数本来接受两个参数，使用apply方法以后，就变成可以接受一个数组作为参数。利用这一点，可以做一些有趣的应用。比如，JavaScript不提供找出数组最大元素的函数，Math.max方法只能返回它的所有参数的最大值，使用apply方法，就可以返回数组的最大元素。

{% highlight javascript %}

Math.max.apply(null, [10, 2, 4, 15, 9])
// 15

{% endhighlight %}

apply方法还有一个特点，就是可以把数组的空元素变成undefined。

{% highlight javascript %}

Array.apply(null, ["a",,"b"])
// [ 'a', undefined, 'b' ]

{% endhighlight %}

这里的差别就是，数组的foreach方法会逃过空元素，但是不会跳过undefined。因此，遍历内部元素的时候，会体现出差别。

{% highlight javascript %}

var a = ["a",,"b"];

function print(i) {
	console.log(i);
}

a.forEach(print)
// a
// b

Array.apply(null,a).forEach(print)
// a
// undefined
// b

{% endhighlight %}

使用apply方法的另一个场合是，使用数组对象的concat方法，展开双层数组（即数组的成员也是数组）。

{% highlight javascript %}

Array.prototype.concat.apply([], [[1], [2]])
// [1, 2]

Array.prototype.concat.apply([], [[1], 2])
// [1, 2]

{% endhighlight %}

concat方法是定义数组对象上的，所以apply方法的第一个参数必须是数组，而第二个参数就是需要被展开的双层数组。不过，这个方法只能展开双层数组，对于更多层的数组，只能展开最外面的一层。

{% highlight javascript %}

Array.prototype.concat.apply([], [[[1]], [2]])
// [[1], 2]

{% endhighlight %}

另外，利用数组对象的slice方法，可以将一个类似数组的对象（比如arguments对象）转为真正的数组。

{% highlight javascript %}

Array.prototype.slice.apply({0:1,length:1})
// [1]

Array.prototype.slice.apply({0:1})
// []

Array.prototype.slice.apply({0:1,length:2})
// [1, undefined]

Array.prototype.slice.apply({length:1})
// [undefined]

{% endhighlight %}

从上面代码可以看到，这个方法起作用的前提是，被处理的对象必须有length属性，以及相对应的数字键。

最后，上一节按钮点击事件的例子，可以改写成

{% highlight javascript %}

var o = {
	p : 1,
	f : function() { console.log(this.p); } 
}

$( "#button" ).on( "click", o.f.call(o) );

// or

$( "#button" ).on( "click", o.f.apply(o) );

{% endhighlight %}

### bind方法

bind方法比call方法和apply方法更进一步，它不仅将原函数的this关键字绑定到其他对象，还绑定原函数的参数，然后返回一个新函数。

{% highlight javascript %}

func.bind(context, [arg1], [arg2], ...)

{% endhighlight %}

请看下面的例子。

{% highlight javascript %}

var o1 = {
	p: 123,
	m: function(){ console.log(this.p);}
};

// 不绑定上下文
var o2 = { p: 456};
o2.m = o1.m;
o2.m()
// 456

// 绑定上下文
o2.m = o1.m.bind(o1);
o2.m()
// 123

{% endhighlight %}

上面代码绑定this对象以后，o1对象的m方法就不再读取o2对象的属性了。

如果bind方法的第一个参数是null或undefined，则函数运行时上下文绑定全局环境。

{% highlight javascript %}

function add(x,y) { return x+y;}

var plus5 = add.bind(null, 5);

plus5(10)
// 15

{% endhighlight %}

bind方法每运行一次，就返回一个新函数，这会产生一些问题。比如，监听事件的时候，不能写成下面这样：

{% highlight javascript %}

element.addEventListener(
        'click', myWidget.handleClick.bind(myWidget));

{% endhighlight %}

click事件绑定的是bind方法生成的一个匿名函数，这导致无法取消绑定。下面的代码是无效的：

{% highlight javascript %}

element.removeEventListener(
        'click', myWidget.handleClick.bind(myWidget));

{% endhighlight %}

正确的方法是写成下面这样：

{% highlight javascript %}

var listener = myWidget.handleClick.bind(myWidget);
element.addEventListener('click', listener);
//  ...
element.removeEventListener('click', listener);

{% endhighlight %}

对于那些不支持bind方法的老式浏览器，可以自行定义bind方法。

{% highlight javascript %}

if(!('bind' in Function.prototype)){
    Function.prototype.bind = function(){
        var fn = this, context = arguments[0], args = Array.prototype.slice.call(arguments, 1);
        return function(){
            return fn.apply(context, args);
        }
    }
}

{% endhighlight %}

另一种方法是使用jQuery的$.proxy方法，它与bind方法的作用基本相同。

{% highlight javascript %}

$( "#button" ).on( "click", $.prox（o.f，o) );

{% endhighlight %}

还有一种更直接的办法，就是索性事先在函数体内部将this的值固定。

{% highlight javascript %}

var o = {
	p : 1,
	self : this,
	f : function() { console.log(self.p); } 
}

$( "#button" ).on( "click", o.f );

{% endhighlight %}

## 参考链接

- Jonathan Creamer, [Avoiding the "this" problem in JavaScript](http://tech.pro/tutorial/1192/avoiding-the-this-problem-in-javascript) 
