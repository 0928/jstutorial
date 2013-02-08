---
title: CSS
layout: page
category: bom
date: 2013-02-08
modifiedOn: 2013-02-08
---

## 动画（animation）

CSS的animation动画定义了三个事件，可以绑定回调函数：动画的开始、动画的结束、动画的循环。

{% highlight javascript %}

var e = document.getElementById("animation");

e.addEventListener("animationstart", listener, false);

e.addEventListener("animationend", listener, false);

e.addEventListener("animationiteration", listener, false);

{% endhighlight %}

回调函数的范例：

{% highlight javascript %}

function listener(e) {

  var l = document.createElement("li");

  switch(e.type) {

    case "animationstart":
      l.innerHTML = "Started: elapsed time is " + e.elapsedTime;
      break;

    case "animationend":
      l.innerHTML = "Ended: elapsed time is " + e.elapsedTime;
      break;

    case "animationiteration":
      l.innerHTML = "New loop started at time " + e.elapsedTime;
      break;

  }

  document.getElementById("output").appendChild(l);

}

{% endhighlight %}

上面代码的运行结果是下面的样子：

{% highlight html %}

Started: elapsed time is 0
New loop started at time 3.01200008392334
New loop started at time 6.00600004196167
Ended: elapsed time is 9.234000205993652

{% endhighlight %}

## 参考链接

- MDN, [Using CSS animations](https://developer.mozilla.org/en-US/docs/CSS/Tutorials/Using_CSS_animations)
