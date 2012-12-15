---
title: 对象
layout: page
category: grammar
date: 2012-12-12
modifiedOn: 2012-12-12
---

## 概述

### 字典结构

对象（object）是Javascript语言对字典结构（dictionary）的实现，由若干个“键值对”（key-value）构成。

{% highlight javascript %}

var o = {

	p: "Hello World"

};

{% endhighlight %}

上面代码中的o被定义为对象，里面包含一个键值对，这个键值对就是对象o的成员。其中，p是“键”（成员的名称），“Hello World”是“值”（成员的值）。

“键”又称为“属性”（property）。属性的值可以是数值、字符串，也可以是函数或其他对象。

### 生成方法

对象用大括号{}表示。

生成一个对象，可以直接用{}，可以用new Object()命令。

{% highlight javascript %}

var o = {};

// or

var o = new Object();

{% endhighlight %}

## 属性（property）

### 引用方法

引用一个属性，有两种方法，一种是点结构，还有一种是方括号。

{% highlight javascript %}

var o = {

	p: "Hello World"

};

// 点结构
console.log(o.p); // Hello World

// 方括号
console.log(o["p"]); // Hello World

{% endhighlight %}

这两种方法，不仅可以引用到该属性对应的值，还可以用来赋值。

{% highlight javascript %}

o.p = "abc";
o["p"] = "abc";

{% endhighlight %}

Object.keys方法可以返回对象本身的属性。

{% highlight javascript %}

Object.keys(object)

{% endhighlight %}

### 存取函数（accessor）

属性还可以用存取函数（accessor）定义。存值函数称为setter，使用set关键字；取值函数称为getter，使用getter关键字。

{% highlight javascript %}

var o = {

	get p() {
		return "Getter";
    },

    set p(value) {
        console.log("Setter: "+value);
    }
}

{% endhighlight %}

定义存取函数之后，引用该属性时，取值函数会自动调用；赋值该属性时，存值函数会自动调用。

{% highlight javascript %}

> o.p
'Getter'

> o.p = 123;
Setter: 123

{% endhighlight %}

### 属性的attributes对象

系统内部，每个属性都有一个对应的attributes对象，保存该属性的一些元信息。

attributes对象包含如下属性：

- Value：表示该属性的值，默认为undefined。
- Writable：表示该属性的值（value）是否可以改变，默认为false。
- Get：表示该属性的取值函数（getter），默认为undefined。
- Set：表示该属性的存值函数（setter），默认为undefined。
- Enumerable： 该属性是否可枚举，默认为false。
- Configurable：该属性是否可配置，false。当为false时，你无法删除该属性，除了value，无法改变该属性的其他性质。Configurable控制该属性元数据的可写状态。

有了attributes对象，就可以描述其对应的属性。

{% highlight javascript %}

    {
        value: 123,
        writable: false,
        enumerable: true,
        configurable: false
    }

{% endhighlight %}

### Object.defineProperty方法

该方法允许通过定义attributes对象，来定义一个属性，然后返回修改后的对象。它的格式是

{% highlight javascript %}

Object.defineProperty(object, propertye, attributes)

{% endhighlight %}

比如，前面的例子可以改写成

{% highlight javascript %}

var o = Object.defineProperty({}, "p", {
        value: 123,
        enumerable: true
});

{% endhighlight %}

如果一次性定义多个属性，可以使用下面的写法。

{% highlight javascript %}

var o = Object.defineProperty({}, {
		p1: { value: 123, enumerable: true },
        p2: { value: "abc", enumerable: true }
});

{% endhighlight %}

### Object.getOwnPropertyDescriptor方法

该方法返回属性的attributes对象，格式如下

{% highlight javascript %}

Object.getOwnPropertyDescriptor(object, property)

{% endhighlight %}

使用方法如下:

{% highlight javascript %}

> var o = Object.defineProperty({}, "p", {
        value: 123,
        enumerable: true
});

> Object.getOwnPropertyDescriptor(o, "p")
{
	configurable: false
	enumerable: true
	value: 123
	writable: false
}

{% endhighlight %}

### 可枚举性

可枚举性与两个操作有关：for-in和Object.keys。如果可枚举性为true，则前面两个操作的返回结果都包括该属性；如果为false，就不包括。

假定，对象o有两个属性p1和p2，可枚举性分别为true和false。

{% highlight javascript %}

var o = Object.defineProperties({}, {
        p1: { value: 1, enumerable: true },
        p2: { value: 2, enumerable: false }
});

{% endhighlight %}

那么，for-in操作和Object.keys操作的返回结果，将不包括p2。

{% highlight javascript %}

> for (var x in o) console.log(x);
  p1

> Object.keys(o)
  ["p1"]

{% endhighlight %}

除了上面两个操作，其他操作都不受可枚举性的影响。

{% highlight javascript %}

 > Object.getOwnPropertyNames(o)
 ["p1", "p2"]

{% endhighlight %}

一般来说，系统原生的属性都是不可枚举的。

{% highlight javascript %}

  > Object.keys([])
  []
 
  > Object.getOwnPropertyNames([])
  [ 'length' ]

  > Object.keys(Object.prototype)
  []

  > Object.getOwnPropertyNames(Object.prototype)
  [ 'hasOwnProperty',
    'valueOf',
    'constructor',
    'toLocaleString',
    'isPrototypeOf',
    'propertyIsEnumerable',
    'toString' ]

{% endhighlight %}

## 参考链接

- Dr. Axel Rauschmayer，[Object properties in JavaScript](http://www.2ality.com/2012/10/javascript-properties.html)
