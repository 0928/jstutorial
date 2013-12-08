---
title: 前言
layout: page
category: introduction
date: 2013-10-06
modifiedOn: 2013-10-26
---

## 起因

我想写这本书，主要的原因是自己需要。

编程的时候，用到JavaScript语言的某个特性或者某个函数，往往需要查阅资料，确定准确的用法。理想中的JavaScript参考书，应该是简明易懂，能够一目了然地告诉我怎么用，有哪些注意点，并提供代码范例。如果涉及到重要概念，还应该适当讲解。可是大多数时候，现实都不是如此，找到的参考资料往往冗长难懂，抓不住重点，有时候还很陈旧，跟不上语言标准和浏览器的快速发展，而且中文资料很少。

为了解决这个问题，我做了很多JavaScript学习笔记，多年累积下来，数量相当庞大。遇到问题，我往往首先查阅自己的笔记，如果笔记里没有，再到网上查资料，最后回过来头把笔记补充完整。终于有一天，我意识到可以把笔记做成一本书，这就是这本教程的由来。

我是为自己写这本书的，我希望用自己的语言叙述JavaScript的特性，按照自己的方式编排章节，便于将来的查阅。当然，另一个写作动力是觉得这些内容对他人有用，毕竟我在笔记上面花了那么多时间，整理成书可以节省其他人的时间。

正是因为脱胎于笔记，所以这本书跟其他JavaScript参考书有所不同。它有点像教程，包含重要概念的简洁讲解，努力把复杂的问题讲得简单，希望一两分钟内就能抓住重点；它又有点像参考手册，列举各种主要用法并且给出可以立即运行的代码。对编程中实际遇到的问题，谈得比较多，而且从语言本身到浏览器接口各个方面都有涉及。所有章节按照主题编排，把与某个主题相关的内容都放在一起，便于需要的时候查阅。

考虑到这本书有参考手册的性质，所以书名加了“参考”（reference）两个字。至于书名中的“标准”，指的是全书主要讲国际标准（standard）的API。

## 写作目标

本书主要针对Web前端开发，以ECMAScript 5作为标准，目标是所讲的内容在实际开发之中基本够用，力求5-10年之内不会过时。

全书的内容比较广泛，只要是实战中用得到的东西都有涉及（核心语法、标准库、DOM、浏览器模型、外部代码库、开发工具等等）。全书的难度为中级，比较适合对JavaScript已经有所了解、想进一步深入学习的读者，英语中称为“高级初学者”（advanced beginner），但是也照顾到入门者的需要，从最简单的开始讲起，循序渐进、由浅入深。另一方面，对于中级开发者，这本书也是有用的，它可以帮你系统地复习和巩固JavaScript语言知识，你会发现这门语言有许多地方是你以前没有注意到的。

在写作风格上，力求做到清晰易懂，具有可读性。所有章节都带有大量的代码实例，这不仅是为了便于理解和模仿，也是为了随时可以用到实际项目中，做到即学即用。

## 开源许可

本书采用创意共享[“署名—非商业性使用”](http://javascript.ruanyifeng.com/introduction/license.html)许可证（Creative Commons Attribution-NonCommercial license）。所有内容不仅可以免费阅读，还可以自由使用（比如转载），只需遵守两个条件：

- **署名**：必须保留原作者的署名。

- **非商业性使用**：除非得到正式许可，否则不得用于商业目的。

事实上，你还可以得到这本书的源码。它就放在[Github](https://github.com/ruanyf/jstutorial)上，欢迎克隆和提交Pull Request。

## 试验环境

本书采用Google的V8引擎作为JavaScript的标准实现，所有示例都以V8引擎的运行结果为准。

阅读之前，请确认已安装基于V8引擎的Chrome浏览器，它附带的“开发者工具”（Developer Tools）就是本书的标准实验环境，可以在其中的“控制台”（console）运行书中的代码。

进入“控制台”，有两种方法。

- 在Chrome浏览器中，直接按Option + J（Mac）或者Ctrl + Shift + J（Windows/Linux）。

- 从“工具”（Tools）菜单中打开“开发者工具”，然后点击Console选项卡。“开发者工具”的快捷键是F12，或者Option + I（Mac）以及Ctrl+Shift+I（Windows/Linux）。

进入控制台以后，就可以在提示符后输入代码，然后按Enter键，代码就会执行。如果按Shift+Enter键，就是代码换行，不会触发执行。建议阅读本书时，将代码复制到控制台进行实验。

## 参考书目

本书的写作过程中，参考了以下书籍（排名不分先后）。

- Nicholas C. Zakas, [Professional JavaScript for Web Developers](http://www.amazon.com/Professional-JavaScript-Developers-Nicholas-Zakas/dp/1118026691), 3 edition, Wrox, 2012

- Axel Rauschmayer, [The Past, Present, and Future of JavaScript](http://oreilly.com/javascript/radarreports/past-present-future-javascript.html), O'Reilly, 2012

- Cody Lindley, [JavaScript Enlightenment](http://www.javascriptenlightenment.com/), O'Reilly, 2012

- Cody Lindley, [DOM Enlightenment](http://domenlightenment.com/), O'Reilly, 2013

- Aaron Frost, [JS.next: A Manager’s Guide](http://chimera.labs.oreilly.com/books/1234000001623), O'Reilly, 2013

- Eric Elliott, [Programming JavaScript Applications](http://chimera.labs.oreilly.com/books/1234000000262), O'Reilly, 2013

- 邱俊涛, [JavaScript核心概念及实践](http://icodeit.org/jsccp/)，人民邮电出版社，2013
