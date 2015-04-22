---
title: Element对象
category: dom
layout: page
date: 2015-04-15
modifiedOn: 2015-04-15
---

Element对象就是网页中HTML标签元素的节点。

## 属性

### attributes，id，tagName

以下属性返回元素节点的性质。

**（1）attributes**

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

**（2）id属性**

id属性返回指定元素的id标识。该属性可读写。

**（3）tagName属性**

tagName属性返回指定元素的大写的标签名，与nodeName属性的值相等。

```javascript
// 假定HTML代码如下
// <span id="span">Hello</span>
var span = document.getElementById("span");
span.tagName // "SPAN"
```

### innerHTML，outerHTML

以下属性返回元素节点的HTML内容。

**（1）innerHTML**

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

**（2）outerHTML**

outerHTML属性返回一个字符串，内容为指定元素的所有HTML代码，包括它自身和包含的所有子元素。

```javascript
// 假定HTML代码如下
// <div id="d"><p>Hello</p></div>

d = document.getElementById("d");
dump(d.outerHTML);

// '<div id="d"><p>Hello</p></div>'
```

outerHTML属性是可读写的，对它进行赋值，等于替换掉当前元素。

```javascript
// 假定HTML代码如下
// <div id="container"><div id="d">Hello</div></div>

container = document.getElementById("container");
d = document.getElementById("d");
container.firstChild.nodeName // "DIV"
d.nodeName // "DIV"

d.outerHTML = "<p>Hello</p>";
container.firstChild.nodeName // "P"
d.nodeName // "DIV"
```

上面代码中，outerHTML属性重新赋值以后，内层的div元素就不存在了，被p元素替换了。但是，变量d依然指向原来的div元素，这表示被替换的DIV元素还存在于内存中。

如果指定元素没有父节点，对它的outerTHML属性重新赋值，会抛出一个错误。

```javascript
document.documentElement.outerHTML = "test";  // DOMException
```

### children，childElementCount，firstElementChild，lastElementChild

以下属性与元素节点的子元素相关。

**（1）children**

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

**（2）childElementCount**

childElementCount属性返回当前元素节点包含的子元素节点的个数。

**（3）firstElementChild**

firstElementChild属性返回第一个子元素，如果没有，则返回null。

**（4）lastElementChild**

lastElementChild属性返回最后一个子元素，如果没有，则返回null。

### nextElementSibling，previousElementSibling

以下属性与元素节点的同级元素相关。

**（1）nextElementSibling**

nextElementSibling属性返回指定元素的后一个同级元素，如果没有则返回null。

```javascript
// 假定HTML代码如下
// <div id="div-01">Here is div-01</div>
// <div id="div-02">Here is div-02</div>
var el = document.getElementById('div-01');
el.nextElementSibling
// <div id="div-02">Here is div-02</div>

```

**（2）previousElementSibling**

previousElementSibling属性返回指定元素的前一个同级元素，如果没有则返回null。

### className，classList

className属性用来读取和设置当前元素的class属性。它的值是一个字符串，每个class之间用空格分割。

classList属性则返回一个类似数组的对象，当前元素节点的每个class就是这个对象的一个成员。

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

以下属性与元素节点的可见区域的坐标相关。

**（1）clientHeight**

clientHeight属性返回元素节点的可见高度，包括padding、但不包括水平滚动条、边框和margin的高度，单位为像素。该属性可以计算得到，等于元素的CSS高度，加上CSS的padding高度，减去水平滚动条的高度（如果存在水平滚动条）。

如果一个元素是可以滚动的，则clientHeight只计算它的可见部分的高度。

**（2）clientLeft**

clientLeft属性等于元素节点左边框（border）的宽度，单位为像素，包括垂直滚动条的宽度，不包括左侧的margin和padding。但是，除非排版方向是从右到左，且发生元素宽度溢出，否则是不可能存在左侧滚动条。如果该元素的显示设为`display: inline`，clientLeft一律为0，不管是否存在左边框。

**（3）clientTop**

clientTop属性等于网页元素顶部边框的宽度，不包括顶部的margin和padding。

**（4）clientWidth**

clientWidth属性等于网页元素的可见宽度，即包括padding、但不包括垂直滚动条（如果有的话）、边框和margin的宽度，单位为像素。

如果一个元素是可以滚动的，则clientWidth只计算它的可见部分的宽度。

### scrollHeight，scrollWidth，scrollLeft，scrollTop

以下属性与元素节点占据的总区域的坐标相关。

**（1）scrollHeight**

scrollHeight属性返回指定元素的总高度，包括由于溢出而无法展示在网页的不可见部分。如果一个元素是可以滚动的，则scrollHeight包括整个元素的高度，不管是否存在垂直滚动条。scrollHeight属性包括padding，但不包括border和margin。该属性为只读属性。

如果不存在垂直滚动条，scrollHeight属性与clientHeight属性是相等的。如果存在滚动条，scrollHeight属性总是大于clientHeight属性。当滚动条滚动到内容底部时，下面的表达式为true。

```javascript
element.scrollHeight - element.scrollTop === element.clientHeight
```

如果滚动条没有滚动到内容底部，上面的表达式为false。这个特性结合`onscroll`事件，可以判断用户是否滚动到了指定元素的底部，比如是否滚动到了《使用须知》区块的底部。

```javascript
var rules = document.getElementById("rules");
rules.onscroll = checking;

function checking(){
  if (this.scrollHeight - this.scrollTop === this.clientHeight) {
    console.log('谢谢阅读');
  } else {
    console.log('您还未读完');
  }
}
```

**（2）scrollWidth**

scrollWidth属性返回元素的总宽度，包括由于溢出容器而无法显示在网页上的那部分宽度，不管是否存在水平滚动条。该属性是只读属性。

**（3）scrollLeft**

scrollLeft属性设置或返回水平滚动条向右侧滚动的像素数量。它的值等于元素的最左边与其可见的最左侧之间的距离。对于那些没有滚动条或不需要滚动的元素，该属性等于0。该属性是可读写属性，设置该属性的值，会导致浏览器将指定元素自动滚动到相应的位置。

**（4）scrollTop**

scrollTop属性设置或返回垂直滚动条向下滚动的像素数量。它的值等于元素的顶部与其可见的最高位置之间的距离。对于那些没有滚动条或不需要滚动的元素，该属性等于0。该属性是可读写属性，设置该属性的值，会导致浏览器将指定元素自动滚动到相应位置。

```javascript
document.querySelector('div').scrollTop = 150;
```

上面代码将div元素向下滚动150像素。

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

### addEventListener()，removeEventListener()，dispatchEvent()

addEventListener方法为元素节点添加事件监听函数。

```javascript
function print() {
  console.log('已经点击');
}

var el = document.getElementById("div1");
el.addEventListener("click", print, false);
```

上面代码为div元素节点，添加了click事件的监听函数print，即一旦发生click事件，就会调用print函数。

addEventListener方法第三个参数是一个布尔值，如果为true，表示在“捕获阶段”触发监听函数，如果为false，表示在“冒泡阶段”触发监听函数。该参数默认为false。

如果向同一个元素节点的同一个事件，添加两个或多个相同的监听函数，那么重复的监听函数都会被自动删除。

```javascript
el.addEventListener("click", print, false);
el.addEventListener("click", print, false);
```

上面代码向元素节点的click事件，添加了两次监听函数print，第二个print函数会被自动删除。也就是说，发生click事件时，print函数只会被调用一次。

监听函数内部的this，指向触发事件的元素节点。

```javascript
function print() {
  console.log(this.id); // 输出div1
}

var el = document.getElementById("div1");
el.addEventListener("click", print, false);
```

上面代码中，监听函数print内部的this变量，指向触发事件的元素节点。这是因为监听函数被“拷贝”成了元素节点的一个属性，使用下面的写法，会看得更清楚。

```javascript
el.onclick = print;
```

如果事件监听函数写在元素的HTML属性里，事件触发时，this变量指向全局对象。

```html
<div id="div1" onclick="print()">
```

上面代码中，print函数内部的this变量，事件触发时指向全局变量。这是因为这种写法没有“拷贝”监听函数，而是事件触发时，找到print函数，然后执行，类似于下面的代码。

```javascript
function onclick()
{
  print();
}
```

上面代码中，print函数内部的this变量指向全局对象。

解决方法就是把this写进HTML属性。

```javascript
<div id="div1" onclick="console.log(this.id)">
```

上面代码中，this指向元素节点。

总结一下，以下写法的this对象都指向元素节点。

```javascript
element.onclick = print
element.addEventListener('click',print,false)
element.onclick = function () {console.log(this.id);}
<element onclick="console.log(this.id)">
```

以下写法的this对象都指向全局对象。

```javascript
element.onclick = function () {print()}
<element onclick="print()">
```

如果希望向监听函数传递参数，可以用匿名函数包装一下监听函数。

```javascript
function print(x) {
  console.log(x);
}

var el = document.getElementById("div1");
el.addEventListener("click", function(){print('Hello')}, false);
```

上面代码通过匿名函数，向监听函数print传递了一个参数。

removeEventListener方法用来移除addEventListener方法添加的事件监听函数。

```javascript
div.addEventListener('click', listener, false);
div.removeEventListener('click', listener, false);
```

注意，removeEventListener方法移除的监听函数，必须与对应的addEventListener方法的参数完全一致，而且在同一个元素节点，否则无效。

dispatchEvent方法触发元素节点的指定事件，从而调用事件监听函数。如果事件监听函数调用了`Event.preventDefault()`，则返回false，否则为true。

```javascript
cancelled = !target.dispatchEvent(event)
```

### closest()

closest方法返回当前元素节点的最接近的父元素（或者当前节点本身），条件是必须匹配给定的CSS选择器。如果不满足匹配，则返回null。

假定HTML代码如下。

```html
<article>
  <div id="div-01">Here is div-01
    <div id="div-02">Here is div-02
      <div id="div-03">Here is div-03</div>
    </div>
  </div>
</article>
```

div-03节点的closet方法的例子如下。

```javascript
var el = document.getElementById('div-03');
el.closest("#div-02") // div-02
el.closest("div div") // div-03
el.closest("article > div") //div-01
el.closest(":not(div)") // article
```

上面代码中，由于closet方法将当前元素节点也考虑在内，所以第二个closet方法返回div-03。
