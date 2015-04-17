---
title: Element对象
category: dom
layout: page
date: 2015-04-15
modifiedOn: 2015-04-15
---

Element对象就是网页中HTML标签元素的节点。

## 属性

### attributes，id

attributes属性返回指定元素节点的所有属性节点，返回的是一个类似数组的对象，每个数字索引对应一个属性节点（Attribute）对象。返回值中，所有成员都是动态的，即属性的变化会实时反映在结果集。

假定下面是一个p元素代码。

```html
<p id="para">Hello World</p>
```

获取attributes成员的代码如下。

```javascript
var para = document.getElementById('para');
var attr = para.attributes[0];

attr.name // id
attr.value // para
```

上面代码说明，通过attributes属性获取属性节点对象（attr）以后，可以通过name属性获取属性名（id），通过value属性获取属性值（para）。

下面代码是遍历一个元素节点的所有属性。

```javascript
var para = document.getElementsByTagName("p")[0];

if (para.hasAttributes()) {
  var attrs = para.attributes;
  var output = "";
  for(var i = attrs.length - 1; i >= 0; i--) {
    output += attrs[i].name + "->" + attrs[i].value;
  }
  result.value = output;
} else {
  result.value = "No attributes to show";
}
```

id属性返回指定元素的id标识。该属性可读写。

### innerHTML

innerHTML属性返回该元素包含的HTML代码。该属性可读写，常用来设置某个节点的内容。

如果将该属性设为空，等于删除所有它包含的所有节点。

```javascript
el.innerHTML = '';
```

上面代码等于将el节点变成了一个空节点，el原来包含的节点被全部删除。

注意，如果文本节点中包含&amp;、小于号（&lt;）和大于号（%gt;），innerHTML属性会将它们转为实体形式&amp、&lt、&gt。

```javascript
// HTML代码如下 <p id="para"> 5 > 3 </p>
document.getElementById('para').innerHTML
// 5 &gt; 3
```

由于上面这个原因，导致在innerHTML插入&lt;script&gt;标签，不会被执行。

```javascript
var name = "<script>alert('haha')</script>";
el.innerHTML = name;
```

上面代码将脚本插入内容，脚本并不会执行。但是，innerHTML还是有安全风险的。

```javascript
var name = "<img src=x onerror=alert(1)>";
el.innerHTML = name;
```

上面代码中，alert方法是会执行的。因此为了安全考虑，如果插入的是文本，最好用textContent属性代替innerHTML。

### children，childElementCount，firstElementChild，lastElementChild

children属性返回一个类似数组的动态对象（实时反映变化），包括当前元素节点的所有子元素。如果当前元素没有子元素，则返回的对象包含零个成员。

```javascript
// para是一个p元素节点
if (para.children.length) {
  var children = para.children;
    for (var i = 0; i < children.length; i++) {
      // ...
    }
}
```

childElementCount属性返回当前元素节点包含的子元素节点的个数。

firstElementChild属性返回第一个子元素，如果没有，则返回null。

lastElementChild属性返回最后一个子元素，如果没有，则返回null。

### className，classList

className属性用来读取和设置当前元素的class属性。它的值是一个字符串，每个class之间用空格分割。

classList属性则返回一个类似数组的对象，当前元素的每个class就是这个对象的一个成员。

{% highlight html %}

<div class="one two three" id="myDiv"></div>

{% endhighlight %}

上面这个div元素的节点对象的className属性和classList属性，分别如下。

```javascript
document.getElementById('myDiv').className
// "one two three"

document.getElementById('myDiv').classList
// {
//   0: "one"
//   1: "two"
//   2: "three"
//   length: 3
// }
```

从上面代码可以看出，className属性返回一个空格分隔的字符串，而classList属性指向一个类似数组的对象，该对象的length属性（只读）返回当前元素的class数量。

classList对象有下列方法。

- add()：增加一个class。
- remove()：移除一个class。
- contains()：检查当前元素是否包含某个class。
- toggle()：将某个class移入或移出当前元素。
- item()：返回指定索引位置的class。
- toString()：将class的列表转为字符串。

```javascript
myDiv.classList.add('myCssClass');
myDiv.classList.add('foo', 'bar');
myDiv.classList.remove('myCssClass');
myDiv.classList.toggle('myCssClass'); // 如果myCssClass不存在就加入，否则移除
myDiv.classList.contains('myCssClass'); // 返回 true 或者 false
myDiv.classList.item(0); // 返回第一个Class
myDiv.classList.toString();
```

下面比较一下，className和classList在添加和删除某个类时的写法。

```javascript

// 添加class
document.getElementById('foo').className += 'bold';
document.getElementById('foo').classList.add('bold');

// 删除class
document.getElementById('foo').classList.remove('bold');
document.getElementById('foo').className =
  document.getElementById('foo').className.replace(/^bold$/, '');

```

toggle方法可以接受一个布尔值，作为第二个参数。如果为true，则添加该属性；如果为false，则去除该属性。

```javascript
el.classList.toggleClass("abc", someBool);

// 等同于

if (someBool){
  el.classList.add("abc");
} else {
  el.classList.remove("abc");
}

```

### clientHeight，clientLeft，clientTop，clientWidth

clientHeight属性返回网页元素的内侧高度，即包括padding、但不包括水平滚动条、边框和margin的高度，单位为像素。该属性可以计算得到，等于元素的CSS高度，加上CSS的padding高度，减去水平滚动条的高度（如果存在水平滚动条）。

clientLeft属性等于网页元素左边框的宽度，单位为像素，包括垂直滚动条的宽度，不包括左侧的margin和padding。但是，除非排版方向是从右到左，且发生元素宽度溢出，否则是不可能存在左侧滚动条。如果该元素的显示设为`display: inline`，clientLeft一律为0，不管是否存在左边框。

clientTop属性等于网页元素顶部边框的宽度，不包括顶部的margin和padding。

clientWidth属性等于网页元素的内侧宽度，即包括padding、但不包括垂直滚动条（如果有的话）、边框和margin的宽度，单位为像素。

## 方法

### hasAttribute()

hasAttribute方法返回一个布尔值，表示当前元素节点是否包含指定的HTML属性。

```javascript
var d = document.getElementById("div1");

if (d.hasAttribute("align")) {
  d.setAttribute("align", "center");
}
```

上面代码检查div节点是否含有align属性。如果有，则设置为“居中对齐”。

### querySelector()，querySelectorAll()

querySelector方法接受CSS选择器作为参数，返回父元素的第一个匹配的子元素。

```javascript
var content = document.getElementById('content');
var el = content.querySelector('p');
```

上面代码返回content节点的第一个p元素。

注意，如果CSS选择器有多个组成部分，比如`div p`，querySelector方法会把父元素考虑在内。假定HTML代码如下。

```html
<div id="outer">
  <p>Hello</p>
  <div id="inner">
    <p>World</p>
  </div>
</div>
```

那么，下面代码会选中第一个p元素。

```javascript
var outer = document.getElementById('outer');
var el = outer.querySelector('div p');
```

querySelectorAll方法接受CSS选择器作为参数，返回一个NodeList对象，包含所有匹配的子元素。

```javascript
var el = document.querySelector('#test');
var matches = el.querySelectorAll('div.highlighted > p');
```

在CSS选择器有多个组成部分时，querySelectorAll方法也是会把父元素本身考虑在内。

还是以上面的HTML代码为例，下面代码会同时选中两个p元素。

```javascript
var outer = document.getElementById('outer');
var el = outer.querySelectorAll('div p');
```
