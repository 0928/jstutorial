---
title: 移动设备API
layout: page
category: bom
date: 2012-12-29
modifiedOn: 2013-12-20
---

为了更好地为移动设备服务，HTML 5推出了一系列针对移动设备的API。

## Geolocation API

Geolocation接口用于获取用户的地理位置。它使用的方法基于GPS或者其他机制（比如IP地址、Wifi热点等）。

下面的方法，可以检查浏览器是否支持这个接口。

{% highlight javascript %}

if(navigator.geolocation) { 
   // 支持
} else {
   // 不支持
}

{% endhighlight %}

### getCurrentPosition方法

getCurrentPosition方法，用来获取用户的地理位置。使用它需要得到用户的授权，浏览器会跳出一个对话框，询问用户是否许可当前页面获取他的地理位置。必须考虑两种情况的回调函数：一种是同意授权，另一种是拒绝授权。如果用户拒绝授权，会抛出一个错误。

{% highlight javascript %}

navigator.geolocation.getCurrentPosition(geoSuccess,geoError);

{% endhighlight %}

上面代码指定了处理当前地理位置的两个回调函数。

**（1）同意授权**

如果用户同意授权，就会调用geoSuccess。

{% highlight javascript %}

function geoSuccess(event) {       
   alert(event.coords.latitude + ', ' + event.coords.longitude);
}

{% endhighlight %}

geoSuccess的参数是一个event对象。event.coords属性指向一个对象，包含了用户的位置信息，主要是以下几个值：

- **coords.latitude**：纬度
- **coords.longitude**：经度
- **coords.accuracy**：精度
- **coords.altitude**：海拔
- **coords.altitudeAccuracy**：海拔精度（单位：米）
- **coords.heading**：以360度表示的方向
- **coords.speed**：每秒的速度（单位：米）

**（2）拒绝授权**

如果用户拒绝授权，就会调用getCurrentPosition方法指定的第二个回调函数geoError。

{% highlight javascript %}

function geoError(event) { 
   console.log("Error code " + event.code + ". " + event.message);
}

{% endhighlight %}

geoError的参数也是一个event对象。event.code属性表示错误类型，有四个值：

- **0**：未知错误，浏览器没有提示出错的原因，相当于常量event.UNKNOWN_ERROR。
- **1**：用户拒绝授权，相当于常量event.PERMISSION_DENIED。
- **2**：没有得到位置，GPS或其他定位机制无法定位，相当于常量event.POSITION_UNAVAILABLE。
- **3**：超时，GPS没有在指定时间内返回结果，相当于常量event.TIMEOUT。

**(3)设置定位行为**

getCurrentPosition方法还可以接受一个对象作为第三个参数，用来设置定位行为。

{% highlight javascript %}

var option = {
            enableHighAccuracy : true,
            timeout : Infinity,
            maximumAge : 0
        };

navigator.geolocation.getCurrentPosition(geoSuccess, geoError, option);

{% endhighlight %}

这个参数对象有三个成员：

- **enableHighAccuracy**：如果设为true，就要求客户端提供更精确的位置信息，这会导致更长的定位时间和更大的耗电，默认设为false。

- **Timeout**：等待客户端做出回应的最大毫秒数，默认值为Infinity（无限）。

- **maximumAge**：客户端可以使用缓存数据的最大毫秒数。如果设为0，客户端不读取缓存；如果设为infinity，客户端只读取缓存。

### watchPosition方法和clearWatch方法

watchPosition方法可以用来监听用户位置的持续改变，使用方法与getCurrentPosition方法一样。

{% highlight javascript %}

var watchID = navigator.geolocation.watchPosition(geoSuccess,geoError);   

{% endhighlight %}

一旦用户位置发生变化，就会调用回调函数geoSuccess。

如果要取消监听，则使用clearWatch方法。

{% highlight javascript %}

navigator.geolocation.clearWatch(watchID);

{% endhighlight %}

## Vibration API

Vibration接口用于在浏览器中发出命令，使得设备振动。由于该操作很耗电，在低电量时最好取消该操作。

使用下面的代码检查该接口是否可用。目前，只有Chrome和Firefox的Android平台最新版本支持它。

{% highlight javascript %}

navigator.vibrate = navigator.vibrate 
					|| navigator.webkitVibrate 
					|| navigator.mozVibrate 
					|| navigator.msVibrate;
 
if (navigator.vibrate) {
    // 支持
}

{% endhighlight %}

vibrate方法可以使得设备振动，它的参数就是振动持续的毫秒数。

{% highlight javascript %}

navigator.vibrate(1000);

{% endhighlight %}

上面的代码使得设备振动1秒钟。

vibrate方法还可以接受一个数组作为参数，表示振动的模式。偶数位置的数组成员表示振动的毫秒数，奇数位置的数组成员表示等待的毫秒数。

{% highlight javascript %}

navigator.vibrate([500, 300, 100]);

{% endhighlight %}

上面代码表示，设备先振动500毫秒，然后等待300毫秒，再接着振动500毫秒。

vibrate是一个非阻塞式的操作，即手机振动的同时，JavaScript代码继续向下运行。要停止振动，只有将0毫秒传入vibrate方法。

{% highlight javascript %}

navigator.vibrate(0);

{% endhighlight %}

## 亮度调节

当移动设备的亮度传感器，感知外部亮度发生显著变化时，会触发devicelight事件。目前，只有Firefox部署了这个API。

{% highlight javascript %}

window.addEventListener('devicelight', function(event) {
  console.log(event.value + 'lux');
});

{% endhighlight %}

上面代码表示，devicelight事件的回调函数，接受一个事件对象作为参数。该对象的value属性就是亮度的流明值。

这个API的一种应用是，如果亮度变强，网页可以显示黑底白字，如果亮度变弱，网页可以显示白底黑字。

{% highlight javascript %}

window.addEventListener('devicelight', function(e) {
  var lux = e.value;

  if(lux < 50) {
    document.body.className = 'dim';
  }
  if(lux >= 50 && lux <= 1000) {
    document.body.className = 'normal';
  }
  if(lux > 1000)  {
    document.body.className = 'bright';
  } 
});

{% endhighlight %}

## 参考链接

- Ryan Stewart, [Using the Geolocation API](http://www.adobe.com/devnet/html5/articles/using-geolocation-api.html)
- Rathnakanya K. Srinivasan, [HTML5 Geolocation](http://www.sitepoint.com/html5-geolocation/)
- Craig Buckler, [How to Use the HTML5 Vibration API](http://www.sitepoint.com/use-html5-vibration-api/)
- Tomomi Imura, [Responsive UI with Luminosity Level](http://girliemac.com/blog/2014/01/12/luminosity/)
