---
title: 封装
layout: page
category: oop
date: 2012-12-14
modifiedOn: 2013-11-16
---

## 原型prototype对象

上一节提到，构造函数可以用作对象的模板。

{% highlight javascript %}

function Animal (name) {
  this.name = name;
}

var myAnimal = new Animal('大毛');

{% endhighlight %}

上面代码的Animal函数是一个构造函数，可以用它生成Animal的实例对象。

构造函数本身也是一个对象，也有自己的属性和方法。其中最重要的是prototype属性，它指向的是一个对象。该对象被称为prototype对象，作用是定义所有实例对象共同拥有的属性和方法。由于它的属性和方法体现在所有实例对象上面，所以它也被称为实例对象的原型，而实例对象可以视作从它的原型衍生出来的。

{% highlight javascript %}

Animal.prototype.walk = function () {
  console.log(this.name + ' is walking.');
};

{% endhighlight %}

上面代码在Animal.protype对象上面定义了一个walk方法，这个方法将可以在所有Animal实例对象上面调用。

### 原型链

JavaScript的每个对象都有自己的原型对象。因此，一个对象的属性和方法，有可能是定义它自身上面，也有可能定义在它的原型对象上面（就像上面代码中的walk方法）。由于原型本身也是对象，所以形成了一条原型链（prototype chain）。比如，a对象是b对象的原型，b对象是c对象的原型，以此类推。因为追根溯源，最源头的对象都是从Object构造函数生成（使用new Object()命令），所以如果一层层地上溯，所有对象的原型最终都可以上溯到Object.prototype。

“原型链”的作用在于，当读取对象的某个属性时，JavaScript引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。以此类推，如果直到最顶层的Object.prototype还是找不到，则返回undefined。

### constructor属性

prototype对象有一个constructor属性，默认指向prototype对象所在的构造函数。它的作用是分辨ptototype对象到底定义在哪个构造函数上面。

{% highlight javascript %}

function Foo() { }

Foo.prototype.constructor === Foo
// true

RegExp.prototype.constructor === RegExp
// true

{% endhighlight %}

上面代码的Foo和RegExp都是构造函数，它们的prototype.constructor属性默认指回Foo和RegExp。

contructor属性是定义在prototype对象上面的，这意味着从构造函数生成的实例对象，都可以读取该属性。

{% highlight javascript %}

new Foo().constructor
// [Function: Foo]

/abc/.constructor
// [Function: RegExp]

{% endhighlight %}

## Object.getPrototypeOf方法

getPrototypeOf方法返回一个对象的原型。

{% highlight javascript %}

Object.getPrototypeOf({}) === Object.prototype
// true

function F() {}
Object.getPrototypeOf(F) === Function.prototype
// true

var f = new F();
Object.getPrototypeOf(f) === F.prototype
// true
	
{% endhighlight %}

## Object.create方法

Object对象的create方法用于生成新的对象。它接受一个原型对象作为参数，返回一个新对象，后者完全继承前者的属性。

{% highlight javascript %}

var o1 = { p: 1 };
var o2 = Object.create(o1);

o2.p // 1

{% endhighlight %}

Object.create方法基本等同于下面的代码，如果老式浏览器不支持Object.create方法，可以用下面代码自己部署。

{% highlight javascript %}

if(typeof Object.create !== "function") {
    Object.create = function (o) {
        function F() {}
        F.prototype = o;
        return new F();
    };
}

{% endhighlight %}

上面代码表示，Object.create方法实质是新建一个构造函数F，然后让F的prototype属性指向作为原型的对象o，最后返回一个F的实例，从而实现让实例继承o的属性。

下面三种方式生成的新对象是等价的。

{% highlight javascript %}

var o1 = Object.create({})
var o2 = Object.create(Object.prototype)
var o3 = new Object();

{% endhighlight %}

如果想要生成一个不继承任何属性（比如toString和valueOf方法）的对象，可以将Object.create的参数设为null。

{% highlight javascript %}

var o = Object.create(null);

o.valueOf()
// TypeError: Object [object Object] has no method 'valueOf'

{% endhighlight %}

上面代码表示，如果对象o的原型是null，它就不具备一些定义在Object.prototype对象上面的属性，比如valueOf方法。

使用Object.create方法的时候，必须提供对象原型，否则会报错。

{% highlight javascript %}

Object.create()
// TypeError: Object prototype may only be an Object or null

{% endhighlight %}

总之，Object.create方法生成的新对象继承了对象原型。

{% highlight javascript %}

var o1 = { p: 1 };
var o2 = Object.create(o1);

o1.p = 2; 
o2.p // 2 

o2.p = 1; 
o1.p // 2 

{% endhighlight %}

上面代码表示，修改对象原型会影响到新生成的对象，反之不成立。

Object.create方法可以接受两个参数，第一个是对象的原型，第二个是描述属性的attributes对象。

{% highlight javascript %}

Object.create(proto, propDescObj)

{% endhighlight %}

用法如下：

{% highlight javascript %}

var o = Object.create(Object.prototype, {
        p1: { value: 123, enumerable: true },
        p2: { value: "abc", enumerable: true }
});

o.p1 // 123
o.p2 // "abc"

{% endhighlight %}

## isPrototypeOf方法

该方法用来判断一个对象是否是另一个对象的原型。

{% highlight javascript %}

var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);
console.log(o2.isPrototypeOf(o3)); // true
console.log(o1.isPrototypeOf(o3)); // true

{% endhighlight %}
