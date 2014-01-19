---
title: MongoDB的应用
layout: page
category: nodejs
date: 2014-01-19
modifiedOn: 2014-01-19
---

MongoDB是目前最流行的noSQL数据库之一，它是专为node.js而开发的。

## Mongoose

多种中间件可以用于连接node.js与MongoDB，目前比较常用的Mongoose。

首先，在项目目录将Mongoose安装为本地模块。

{% highlight bash %}

npm install mongoose --save

{% highlight %}

然后，就可以在node.js脚本中连接MongoDB数据库了。

{% highlight javascript %}

var mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/<数据库名>');

{% highlight %}

注意，运行上面这个脚本时，必须确保MongoDB处于运行中。
