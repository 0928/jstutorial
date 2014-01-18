---
layout: homepage
title: JavaScript 标准参考教程（alpha）
date: 2012-11-18
modifiedOn: 2014-01-18
---
	
<h2 id="introduction">导论</h2>

- [前言](introduction/preface.html)
- [为什么学习JavaScript？](introduction/why.html)
- [JavaScript的历史](introduction/history.html)

<h2 id="grammar">基本语法</h2>

- [概述](grammar/basic.html)
- [数值](grammar/number.html)
- [字符串](grammar/string.html)
- [对象](grammar/object.html)
- [数组](grammar/array.html)
- [函数](grammar/function.html)
- [运算符](grammar/operator.html)		
- [数据类型转换](grammar/conversion.html)（基本完成）
- [错误处理机制](grammar/error.html)（基本完成）
- [编程风格](grammar/style.html)（基本完成）

<h2 id="stdlib">标准库</h2>

- [Object对象](stdlib/object.html)
- [Array 对象](stdlib/array.html)
- [原始类型的包装对象](stdlib/wrapper.html)
- [Boolean对象](stdlib/boolean.html)
- [Number对象](stdlib/number.html)
- [String对象](stdlib/string.html)
- [Math对象](stdlib/math.html)
- [Date对象](stdlib/date.html)
- [RegExp对象](stdlib/regexp.html)
- [JSON对象](stdlib/json.html)
- [类型化数组](stdlib/arraybuffer.html)

<h2 id="oop">面向对象编程</h2>

- [概述](oop/basic.html)
- [封装](oop/encapsulation.html)
- [继承](oop/inheritance.html)

<h2 id="dom">DOM</h2>

- [DOM API 概述](dom/basic.html)
- [DOM 事件](dom/event.html)
- [样式表操作](dom/stylesheet.html)
- [拖放操作](dom/dragndrop.html)
- [Mutation Observer](dom/mutationobserver.html)

<h2 id="bom">浏览器对象</h2>

- [浏览器的JavaScript引擎](bom/engine.html)
- [window对象](bom/window.html)
- [History对象](bom/history.html)
- [CSS](bom/css.html)
- [Ajax](bom/ajax.html)
- [同域限制与window.postMessage方法](bom/windowpostmessage.html)
- [Web Storage：浏览器端数据储存机制](bom/webstorage.html)
- [IndexedDB：浏览器端数据库](bom/indexeddb.html)
- [WebSocket](bom/websocket.html)（基本完成）
- [WebRTC](bom/webrtc.html)
- [performance对象：高精度时间戳](bom/performance.html)
- [移动设备API](bom/mobile.html)
		
<h2 id="htmlapi">HTML网页的API</h2>

- [Canvas](htmlapi/canvas.html)
- [SVG图像](htmlapi/svg.html)
- [文件与二进制数据的操作](htmlapi/file.html)
- [Web Worker](htmlapi/webworker.html)
- [服务器端发送事件](htmlapi/eventsource.html)
- [Page Visiblity](htmlapi/pagevisibility.html)
- [FullScreen](htmlapi/fullscreen.html)
- [Web Speech](htmlapi/webspeech.html)
- [requestAnimationFrame](htmlapi/requestanimationframe.html)

<h2 id="jquery">jQuery</h2>

- [概述](jquery/basic.html)
- [工具方法](jquery/utility.html)
- [插件开发](jquery/plugin.html)
- [deferred对象](jquery/deferred.html)
- [如何做到jQuery-free？](jquery/jquery-free.html)

<h2 id="tool">开发工具</h2>

- [Chrome开发者工具和console对象](tool/console.html)
- [PhantomJS](tool/phantomjs.html)
- [Bower：客户端库管理工具](tool/bower.html)
- [Grunt：任务自动管理工具](tool/grunt.html)
- [Browserify：浏览器加载Node.js模块](tool/browserify.html)
- [RequireJS和AMD规范](tool/requirejs.html)
- [Source map](tool/sourcemap.html)

<h2 id="advanced">JavaScript高级语法</h2>

- [异步编程的模式](advanced/asynchronous.html)
- [有限状态机](advanced/fsm.html)
- [MVC模式和Backbone.js](advanced/backbonejs.html)
- [严格模式](advanced/strict.html)（基本完成）
- [ECMAScript 6 介绍](advanced/ecmascript6.html)
		
<hr></hr>

<h2 id="library">草稿一：函数库</h2>

- [Underscore.js](library/underscore.html)
- [Modernizr](library/modernizr.html)
- [Datejs](library/datejs.html)
- [D3.js](library/d3.html)
- [设计模式](library/designpattern.html)
- [排序算法](library/sorting.html)

<h2 id="nodejs">草稿二：Node.js</h2>

- [概述](nodejs/basic.html)
- [CommonJS规范](nodejs/commonjs.html)
- [Express框架](nodejs/express.html)

{% comment %}

{% if site.posts.size != 0 %}

## 最新文章

{% for post in site.posts %}
* {{ post.date | date_to_string }} [{{ post.title }}]({{ post.url }})
{% endfor %}

{% endif %}

{% if site.pages.size != 0 %}

## 最新页面

{% for page in site.pages limit:5 %}
{% if page.url !='/index.html' %}
* [{{ page.title }}]( {{ page.url }})（{{ page.date }}）
{% endif %}
{% endfor %}

{% endif %}

{% endcomment %}
