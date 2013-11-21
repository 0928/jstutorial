---
title: jQuery概述
layout: page
category: jquery
date: 2013-02-01
modifiedOn: 2013-11-21
---

jQuery是目前使用最广泛的JavaScript函数库。据统计，全世界使用JavaScript函数库的网页，90%选择了jQuery；全世界排名前10000位的网站，57%使用了jQuery。它的最大优势有两个，一是使得操作网页元素变得异常容易，二是统一了接口，使得开发者可以用同一种方法编写能在所有现代浏览器中运行的代码，而不用担心浏览器之间的差异。

jQuery的核心思想是“选中某个元素，进行某种处理”（find something, do something），也就是说，先选择后处理。所以，绝大多数jQuery操作都是从选择器开始的。

## 选择器

jQuery函数库提供了一个全局对象jQuery，简写为$。在网页中加载jQuery以后，就可以使用这个全局对象了。jQuery的全部方法，都定义在这个对象上面。

jQuery对象本身是一个函数，参数为CSS选择器，返回选中的元素。

{% highlight javascript %}

var listItems = jQuery('li');
// or
var listItems = $('li');

{% endhighlight %}

上面两行代码是等价的，表示选中网页中所有的li元素。

这里需要注意的是，jQuery函数返回的并不是DOM元素，而是jQuery对象实例。因为只有这样，才能在返回对象上面使用jQuery提供的各种方法。

{% highlight javascript %}

$('body').nodeType
// undefined

$('body') instanceof jQuery
// true

{% endhighlight %}

上面代码表示，由于jQuery返回的不是DOM对象，所以没有nodeType属性。它返回的是jQuery对象的实例。 

如果直接将DOM对象作为参数，放入jQuery函数，则会被转为jQuery对象的实例。

{% highlight javascript %}

$(document.body) instanceof jQuery
// true

{% endhighlight %}

上面代码中，jQuery函数的参数不是CSS选择器，而是一个DOM对象，返回的依然是jQuery对象的实例。

如果有多个DOM元素要转为jQuery对象的实例，可以把DOM元素放在一个数组里，输入jQuery函数。

{% highlight javascript %}

$([document.body, document.head]) 

{% endhighlight %}

默认情况下，jQuery将文档的根元素（html）作为寻找匹配对象的起点。如果要指定某个网页元素作为寻找的起点，可以将它放在jQuery函数的第二个参数。

{% highlight javascript %}

$('li', someElement);

{% endhighlight %}

上面代码表示，只寻找属于someElement对象下属的li元素。someElement可以是jQuery对象的实例，也可以是DOM对象。

jQuery对象返回的是一个类似数组的对象，包含了所有被选中的网页元素。查看该对象的length属性，可以知道到底选中了多少个结果。

{% highlight javascript %}

if ( $('li').length === 0 ) {
	console.log('不含li元素');
}

{% endhighlight %}

上面代码表示，如果网页没有li元素，则返回对象的length属性等于0。这就是测试有没有选中的标准方法。

除了length属性，还有一个is方法，返回一个布尔值，表示选中的结果是否符合某个条件。用来验证的判断条件，可以是CSS选择器，也可以是一个函数，或者DOM元素和jQuery实例。

{% highlight javascript %}

$('li').is('li') // true

$('li').is($('.item')) 

$('li').is(document.querySelector('li'))

$('li').is(function() {
      return $("strong", this).length === 0;
});

{% endhighlight %}

需要注意的是，使用下标运算符取出的单个对象，就不是jQuery对象的实例，而是一个DOM对象。

{% highlight javascript %}

$('li')[0] instanceof $ // false
$('li')[0] instanceof Element // true

{% endhighlight %}

上面代码表示，下标运算符取出的是Element节点的实例。所以，通常使用下标运算符将jQuery实例转回DOM对象。

jQuery实例的get方法是下标运算符的另一种写法。

{% highlight javascript %}

$('li').get(0) instanceof Element // true

{% endhighlight %}

如果只需要在选中结果中取出某一个对象，不需要将其转为DOM对象，则使用jQuery实例的eq方法。

{% highlight javascript %}

$('li').eq(0) instanceof jQuery // true

{% endhighlight %}

eq方法返回的还是jQuery的实例，可以接着在返回结果上使用jQuery的方法。

如果直接在jQuery函数中输入HTML代码，则返回一个jQuery实例。

{% highlight javascript %}

$('<li class="greet">test</lt>')

{% endhighlight %}

上面代码从HTML代码生成了一个jQuery实例，它从CSS选择器生成的jQuery实例完全一样。唯一的区别就是，它对应的DOM结构不属于当前文档。上面代码也可以写成下面这样。

{% highlight javascript %}

$( '<li>', {
  html: 'test',
  'class': 'greet'
});

{% endhighlight %}

上面代码中，由于class是javaScript的保留字，所以只能放在引号中。

## jQuery实例对象的方法

除了上一节提到的is、get、eq方法，jQuery实例还有许多其他方法。

### filter方法，not方法，has方法

通过选择器选中结果以后，有多种方法可以对结果进行过滤。

filter方法用于过滤选中的结果，它可以接受多种类型的参数。

{% highlight javascript %}

// 返回符合CSS选择器的结果
$('li').filter('.item')

// 返回函数返回值为true的结果
$("li").filter(function(index) {
    return index % 2 === 1;
})

// 返回符合特定DOM对象的结果
$("li").filter(document.getElementById("unique"))

// 返回符合特定jQuery实例的结果
$("li").filter($("#unique"))

{% endhighlight %}

not方法的用法与filter方法完全一致，但是返回相反的结果，即过滤掉匹配项。

{% highlight javascript %}

$('li').not('.item')

$("li").not(function(index) {
    return index % 2 === 1;
})

$("li").not(document.getElementById("unique"))

$("li").not($("#unique"))

{% endhighlight %}

has方法与filter方法作用相同，但是只过滤出子元素符合条件的元素。

{% highlight javascript %}

// 返回子元素包含ul元素的li元素
$("li").has("ul")

{% endhighlight %}

### 选择相对位置的元素

如果已有选中的元素，有一些方法可以选出处在相对位置的元素。

first方法返回结果集的第一个成员。

{% highlight javascript %}

$("li").first()

{% endhighlight %}

next方法返回紧邻的下一个同级元素，prev方法返回紧邻的上一个同级元素。

{% highlight javascript %}

$("li").first().next()

$("li").first().next('.item')

$("li").prev()

{% endhighlight %}

parent方法返回选中元素的父元素，parents方法返回选中元素的所有上级元素（直到html元素）。

{% highlight javascript %}

$("p").parent()
$("p").parent(".selected")

$("p").parents()
$("p").parents("div")

{% endhighlight %}

children方法返回选中元素的所有下级元素。

{% highlight javascript %}

$("div").children()
$("div").children(".selected")

{% endhighlight %}

find方法返回符合条件的所有下级元素。

{% highlight javascript %}

$("div").find(".selected")

{% endhighlight %}

closest方法返回选中元素的第一个符合条件的上级元素。

{% highlight javascript %}

$(li").closest('div')

{% endhighlight %}

siblings方法返回选中元素的所有同级元素。

{% highlight javascript %}

// 返回第一个li元素的同级元素
$('li').first().siblings()

// 返回第一个li元素的同级、且具有class包含item的元素
$('li').first().siblings('.item')

{% endhighlight %}

nextAll方法返回选中元素其后的所有同级元素，prevAll方法返回选中元素前面的所有同级元素。

{% highlight javascript %}

$('li').first().nextAll()

{% endhighlight %}

add方法用于为结果集添加元素。

{% highlight javascript %}

$('li').add('p')

{% endhighlight %}

addBack方法将当前的选中元素加回原始的结果集。

{% highlight javascript %}

$('li').parent().addBack()

{% endhighlight %}

end方法用于返回原始的结果集。

{% highlight javascript %}

$('li').first().end()

{% endhighlight %}

### DOM相关方法

许多方法可以对DOM元素进行处理。

addClass方法用于添加一个类。

{% highlight javascript %}

$('li').addClass('special')

{% endhighlight %}

removeClass方法用于移除一个类。

{% highlight javascript %}

$('li').removeClass('special')

{% endhighlight %}

toggleClass方法用于折叠一个类（如果无就添加，如果有就移除）。

{% highlight javascript %}

$('li').toggleClass('special')

{% endhighlight %}

css方法用于改变CSS设置。

{% highlight javascript %}

$('li').css('padding-left', '20px')

$('li').css({
  'padding-left': '20px'
});

{% endhighlight %}

val方法返回结果集第一个元素的值，或者设置当前结果集所有元素的值。

{% highlight javascript %}

$('input[type="text"]').val()

$('input[type="text"]').val('new value')

{% endhighlight %}

prop方法返回结果集第一个元素的特定DOM属性的值，或者设置当前结果集所有元素的特定DOM属性的值。

{% highlight javascript %}

$('input[type="checkbox"]').prop('checked');

$('input[type="checkbox"]').prop('checked', true)

{% endhighlight %}

attr方法返回结果集第一个元素的特定HTML属性的值，或者设置当前结果集所有元素的特定HTML属性的值。

{% highlight javascript %}

$('a').attr('title')

$('a').attr('title', 'Click me!')

$('a').attr('href', function(index, value) {
  return value + '?special=true';
});

{% endhighlight %}

attr方法与prop方法的区别在于，attr方法针对HTML属性，prop方法针对DOM属性（比如selectedIndex、tagName、nodeName、nodeType等等）。有时，一个属性既可以用attr方法读取，也可以用prop方法读取。比如，下面这一行HTML代码。

{% highlight html %}

<input type="checkbox" checked="checked" />

{% endhighlight %}

attr方法和prop方法针对checked属性的返回值不一样。

{% highlight javascript %}

$('input[type="checkbox"]').attr('checked')
// 'checked'

$('input[type="checkbox"]').prop('checked') 
// true

{% endhighlight %}

上面代码表示，attr方法返回HTML属性的值，结果为checked；prop方法返回DOM属性的值，结果为true。

removeProp方法移除某个DOM属性。

{% highlight javascript %}

$("a").prop("oldValue",1234).removeProp('oldValue')

{% endhighlight %}

removeAttr方法移除某个HTML属性。

{% highlight javascript %}

$('a').removeAttr("title")

{% endhighlight %}

### html方法和text方法

html方法返回该元素包含的HTML代码，text方法返回该元素包含的文本。

假定网页只含有一个p元素。

{% highlight html %}

<p><em>Hello World!</em></p>

{% endhighlight %}

html方法和text方法的返回结果分别如下。

{% highlight javascript %}

$('p').html()
// <em>Hello World!</em> 

$('p').text()
// Hello World! 

{% endhighlight %}

jQuery的许多方法都是取值器（getter）与赋值器（setter）的合一，即取值和赋值都是同一个方法，不使用参数的时候为取值器，使用参数的时候为赋值器。

上面代码的html方法和text方法都没有参数，就会当作取值器使用，取回元素所包含的代码和文本。如果对它们提供参数，就是当作赋值器使用，重新赋予元素所包含的代码和文本。

{% highlight javascript %}

$('p').html('<strong>你好</strong>')
// 网页代码变为<p><strong>你好</strong></p> 

$('p').text('你好')
// 网页代码变为<p>你好</p> 

{% endhighlight %}

由于jQuery对所有选中的元素，进行下面要讲到的内置循环操作，所以如果有一组元素被选中，html方法和text方法会都对它们进行统一处理。

html方法和text方法还接受一个函数作为参数，函数的返回值就是网页元素所要包含的新的代码和文本。这个函数接受两个参数，第一个是网页元素在集合中的位置，第二个参数是网页元素原来的代码或文本。

{% highlight javascript %}

$('li').html(function (i, v){
	return (i + ': ' + v);		
})

// <li>Hello</li>
// <li>World</li>
// 变为
// <li>0: Hello</li>
// <li>1: World</li>

{% endhighlight %}

### each方法

each方法接受一个函数作为参数，依次处理集合中的每一个元素。

{% highlight javascript %}

$('li').each(function( index, elem ) {
  $(elem).prepend( '<em>' + index + ': </em>' );
});

// <li>Hello</li>
// <li>World</li>
// 变为
// <li><em>0</em>: Hello</li>
// <li><em>1</em>: World</li>

{% endhighlight %}

从上面代码可以看出，作为each方法参数的函数，本身有两个参数，第一个是当前元素在集合中的位置，第二个当前元素对应的DOM对象。

### 改变元素位置的方法

jQuery方法提供一系列方法，可以改变元素在文档中的位置。

**（1）append方法，appendTo方法**

append方法将参数中的元素插入当前元素的尾部。

{% highlight javascript %}

$("div").append("<p>World</p>")

// <div>Hello </div>
// 变为
// <div>Hello <p>World</p></div>

{% endhighlight %}

appendTo方法将当前元素插入参数中的元素尾部。

{% highlight javascript %}

$("<p>World</p>").appendTo("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（2）prepend方法，prependTo方法**

prepend方法将参数中的元素，变为当前元素的第一个子元素。

{% highlight javascript %}

$("p").prepend("Hello ")

// <p>World</p>
// 变为
// <p>Hello World</p>

{% endhighlight %}

如果prepend方法的参数不是新生成的元素，而是当前页面已存在的元素，则会产生移动元素的效果。

{% highlight javascript %}

$("p").prepend("Hello ")

// <p>World</p>
// 变为
// <p>Hello World</p>

{% endhighlight %}

上面代码运行后，strong元素的位置将发生移动，而不是克隆一个新的strong元素。不过，如果当前结果集包含多个元素，则除了第一个以后，后面的p元素都将插入一个克隆的strong子元素。

prependTo方法将当前元素变为参数中的元素的第一个子元素。

{% highlight javascript %}

$("<p></p>").prepend("div")

// <div></div>
// 变为
// <div><p></p></div>

{% endhighlight %}

**（3）after方法，insertAfter方法**

after方法将参数中的元素插在当前元素后面。

{% highlight javascript %}

$("div").after("<p></p>")

// <div></div>
// 变为
// <div></div><p></p>

{% endhighlight %}

insertAfter方法将当前元素插在参数中的元素后面。

{% highlight javascript %}

$("<p></p>").insertAfter("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（4）before方法，insertBefore方法**

before方法将参数中的元素插在当前元素的前面。

{% highlight javascript %}

$("div").before("<p></p>")

// <div></div>
// 变为
// <p></p><div></div>

{% endhighlight %}

insertBefore方法将当前元素插在参数中的元素的前面。

{% highlight javascript %}

$("<p></p>").insertBefore("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（5）wrap方法，wrapAll方法，unwrap方法，wrapInner方法**

wrap方法将参数中的元素变成当前元素的父元素。

{% highlight javascript %}

$("p").wrap("<div></div>")

// <p></p>
// 变为
// <div><p></p></div>

{% endhighlight %}

wrap方法的参数还可以是一个函数。

{% highlight javascript %}

$("p").wrap(function() {
  return "<div></div>";
})

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

wrapAll方法为结果集的所有元素，添加一个共同的父元素。

{% highlight javascript %}

$("p").wrapAll("<div></div>")

// <p></p><p></p>
// 变为
// <div><p></p><p></p></div>

{% endhighlight %}

unwrap方法移除当前元素的父元素。

{% highlight javascript %}

$("p").unwrap()

// <div><p></p></div>
// 变为
// <p></p>

{% endhighlight %}

wrapInner方法为当前元素的所有子元素，添加一个父元素。

{% highlight javascript %}

$("p").wrapInner('<strong></strong>')

// <p>Hello</p>
// 变为
// <p><strong>Hello</strong></p>

{% endhighlight %}

**（6）clone方法**

clone方法克隆当前元素。

{% highlight javascript %}

var clones = $('li').clone();

{% endhighlight %}

对于那些有id属性的节点，clone方法会连id属性一起克隆。所以，要把克隆的节点插入文档的时候，务必要修改或移除id属性。

**（7）remove方法，detach方法，replaceWith方法**

remove方法移除并返回一个元素，取消该元素上所有事件的绑定。detach方法也是移除并返回一个元素，但是不取消该元素上所有事件的绑定。

{% highlight javascript %}

$('p').remove()
$('p').detach()

{% endhighlight %}

replaceWith方法用参数中的元素，替换并返回当前元素，取消当前元素的所有事件的绑定。

{% highlight javascript %}

$('p').replaceWith('<div></div>')

{% endhighlight %}

### 链式操作

jQuery最方便的一点就是，它的所有赋值器返回的都是同样数量的jQuery实例对象，即输入的集合有几个jQuery实例，返回的集合还是那几个实例，因此可以链式操作。也就是说，后一个方法可以紧跟着写在前一个方法后面。

{% highlight javascript %}

$('li').click(function (){
    $(this).addClass( 'clicked' );
}).find('span')
.attr( 'title', 'Hover over me' );

{% endhighlight %}

### 内置循环

jQuery默认对选中的元素进行循环处理。

{% highlight javascript %}

$(".class").addClass("highlight");

{% endhighlight %}

上面代码会执行一个内部循环，对每一个选中的元素进行addClass操作。由于这个原因，对选中的元素使用each方法是多余的。

{% highlight javascript %}

$(".class").each(function(index,element){
	 $(element).addClass("highlight");
});

// 或者

$(".class").each(function(){
	$(this).addClass("highlight");
});

{% endhighlight %}

上面代码的each方法，都是没必要使用的。

由于内置循环的存在，从性能考虑，应该尽量减少不必要的操作步骤。

{% highlight javascript %}

$(".class").css("color", "green").css("font-size", "16px");

// 应该写成

$(".class").css({ 
  "color": "green",
  "font-size": "16px"
});

{% endhighlight %}

### $(document).ready()

$(document).ready方法接受一个函数作为参数，将该参数作为document对象的DOMContentLoaded事件的回调函数。也就是说，当页面解析完成（即下载完&lt;/html&gt;标签）以后，在所有外部资源（图片、脚本等）完成加载之前，该函数就会立刻运行。

{% highlight javascript %}

$( document ).ready(function() {
  console.log( 'ready!' );
});

{% endhighlight %}

上面代码表示，一旦页面完成解析，就会运行ready方法指定的函数，在控制台显示“ready!”。

该方法通常作为网页初始化手段使用，jQuery提供了一种简写法，就是直接把回调函数放在jQuery对象中。

{% highlight javascript %}

$(function() {
  console.log( 'ready!' );
});

{% endhighlight %}

上面代码与前一段代码是等价的。

### 属性的读写

首先，这里要区分两种属性。

一种是网页元素的属性，比如a元素的href属性、img元素的src属性，这要使用attr方法读写：.attr(name)用于读取属性值，.attr(name, val)用于写入属性值。

另一种是DOM元素的属性，比如tagName、nodeName、nodeType等等，这要使用prop方法读写：.prop(name)用于读取属性值，.prop(name, val)用于写入属性值。

所以，attr方法和prop方法针对的是不同的属性。在英语中，attr是attribute的缩写，prop是property的缩写，中文很难表达出这种差异。有时，attr方法和prop方法对同一个属性会读到不一样的值。比如，网页上有一个单选框。

{% highlight html %}

<input type="checkbox" checked="checked" />

{% endhighlight %}

对于checked属性，attr方法读到的是checked，prop方法读到的是true。

{% highlight javascript %}

$(input[type=checkbox]).attr("checked") // "checked"

$(input[type=checkbox]).prop("checked") // true

{% endhighlight %}

可以看到，attr方法读取的是网页上该属性的值，而prop方法读取的是DOM元素的该属性的值，根据规范，elem.checked应该返回一个布尔值。所以，判断单选框是否选中，要使用prop方法。事实上，不管这个单选框是否选中，attr("checked")的返回值都是checked。

{% highlight javascript %}

if ($(elem).prop("checked")) { /*... */ };

// 下面两种方法亦可

if ( elem.checked ) { /*...*/ };
if ( $(elem).is(":checked") ) { /*...*/ };

{% endhighlight %}

## 事件处理

### 事件绑定的简便方法

jQuery提供一系列函数方法，允许直接为常见事件绑定回调函数。比如，click方法可以为一个元素绑定click事件的回调函数。

{% highlight javascript %}

$('li').click(function (e){
  console.log($(this).text());
});

{% endhighlight %}

上面代码为li元素绑定click事件的回调函数，点击后在console显示li元素包含的文本。

这样的方法有如下一些：

- click
- keydown
- keypress
- keyup
- mouseover
- mouseout
- mouseenter
- mouseleave
- scroll
- focus
- blur
- resize

如果不带参数调用这些方法，就是触发相应的事件，从而引发回调函数的运行。

{% highlight javascript %}

$('li').click()

{% endhighlight %}

上面代码将触发click事件的回调函数。

需要注意的是，通过这种方法触发回调函数，将不会引发浏览器对该事件的默认行为。比如，对a元素调用click方法，将只触发事先绑定的回调函数，而不会导致浏览器将页面导向href属性指定的网址。

### on方法，trigger方法，off方法

**（1）on方法**

事件绑定的那些简便方法，其实都是on方法的简写形式。on方法是jQuery事件绑定的统一接口。

on方法接受两个参数，第一个是事件名称，第二个是回调函数。

{% highlight javascript %}

$('li').on('click', function (e){
  console.log($(this).text());
});

{% endhighlight %}

上面代码为li元素绑定click事件的回调函数。

> 注意，在回调函数内部，this关键字指的是发生该事件的DOM对象。为了使用jQuery提供的方法，必须将DOM对象转为jQuery对象，因此写成$(this)。

on方法允许一次为多个事件指定同样的回调函数。

{% highlight javascript %}

$('input[type="text"]').on('focus blur', function (){
  console.log('focus or blur');
});

{% endhighlight %}

上面代码为文本框的focus和blur事件绑定同一个回调函数。

on方法还可以为当前元素的某一个子元素，添加回调函数。

{% highlight javascript %}

$('ul').on('click', 'li', function (e){
  console.log(this);
});

{% endhighlight %}

上面代码为ul的子元素li绑定click事件的回调函数。采用这种写法时，on方法接受三个参数，子元素选择器作为第二个参数，夹在事件名称和回调函数之间。

这种写法有两个好处。首先，click事件还是在ul元素上触发回调函数，但是会检查event.target属性是否为li子元素，如果为true，再调用回调函数。这样就比为li元素一一绑定回调函数，节省了内存空间。其次，这种绑定的回调函数，对于在此后生成的li元素依然有效。

**（2）trigger方法**

trigger方法用于触发回调函数，它的参数就是事件的名称。

{% highlight javascript %}

$('li').trigger('click')

{% endhighlight %}

上面代码触发li元素的click事件回调函数。与那些简便方法一样，trigger方法只触发回调函数，而不会引发浏览器的默认行为。

**（3）off方法**

off方法用于移除事件的回调函数。

{% highlight javascript %}

$('li').off('click')

{% endhighlight %}

上面代码移除li元素所有的click事件回调函数。

**（4）事件的名称空间**

如果只想移除某一个回调函数，可以采用“名称空间”的方式，为每一个回调函数指定一个二级事件名，然后再用off方法移除这个二级事件的回调函数。

{% highlight javascript %}

$('li').on('click.logging', function (){
  console.log('click.logging callback removed');
});

$('li').off('click.logging');

{% endhighlight %}

上面代码为li元素定义了二级事件click.logging的回调函数，click.logging属于click名称空间，当发生click事件时会触发该回调函数。将click.logging作为off方法的参数，就会移除这个回调函数，但是对其他click事件的回调函数没有影响。

trigger方法也适用带名称空间的事件。

{% highlight javascript %}

$('li').trigger('click.logging')

{% endhighlight %}

### event对象

当回调函数被触发后，它们的参数通常是一个事件对象event。

{% highlight javascript %}

$(document).on('click', function (e){
	// ...
});

{% endhighlight %}

上面代码的回调函数的参数e，就代表事件对象event。

event对象有以下属性：

- type：事件类型，比如click。
- which：触发该事件的鼠标按钮或键盘的键。
- target：事件发生的初始对象。
- pageX：事件发生时，鼠标位置的水平坐标。
- pageY：事件发生时，鼠标位置的垂直坐标。

event对象有以下方法：

- preventDefault：取消浏览器默认行为。
- stopPropagation：阻止事件向上层元素传播。

### 一次性事件

one方法指定一次性的回调函数，即这个函数只能运行一次。这对提交表单很有用。

{% highlight javascript %}

$("#button").one( "click", function() { return false; } );

{% endhighlight %}

one方法本质上是回调函数运行一次，即解除对事件的监听。

{% highlight javascript %}

document.getElementById("#button").addEventListener("click", handler);

function handler(e) {
    e.target.removeEventListener(e.type, arguments.callee);
	return false;
}

{% endhighlight %}

上面的代码在点击一次以后，取消了对click事件的监听。如果有特殊需要，可以设定点击2次或3次之后取消监听，这都是可以的。

## 参考链接

- Elijah Manor, [Do You Know When You Are Looping in jQuery?](http://www.elijahmanor.com/2013/01/yo-dawg-i-herd-you-like-loops-so-jquery.html)
- Craig Buckler, [How to Create One-Time Events in JavaScript](http://www.sitepoint.com/create-one-time-events-javascript/)
- jQuery Fundamentals, [jQuery Basics](http://jqfundamentals.com/chapter/jquery-basics)
