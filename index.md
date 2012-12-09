---
layout: homepage
title: Javascript 标准教程
date: 2012-11-18
---

<h2 id="dom">DOM</h2>

* [dataset](dom/dataset.html)
* [classList](dom/classlist.html)

<h2 id="htmlapi">HTML5 API</h2>

* [Canvas](htmlapi/canvas.html)
* [File](htmlapi/file.html)
* [Page Visiblity](htmlapi/pagevisibility.html)
* [FullScreen](htmlapi/fullscreen.html)

<h2 id="algorithm">算法（Algorithm）</h2>

* [排序（sorting）](algorithm/sorting.html)

<h2 id="jquery">jQuery</h2>

+ [deferred对象](jquery/deferred.html)

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
