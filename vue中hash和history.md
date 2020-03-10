# 在vue-router中hash和history路由的区别

## 一、hash模式及其原理

### 1、简介

早期的前端路由的实现就是基于 location.hash 来实现的。其实现原理很简单，location.hash 的值就是 URL 中 # 后面的内容。比如下面这个网站，它的 location.hash 的值为 ‘#/home/index ‘：http://localhost:8080/#/home/index。

![location.href](./img/location.href.png)



URL 中 hash 值只是客户端的一种状态，也就是说当向服务器端发出请求时，hash 部分不会被发送。

hash 值的改变，都会在浏览器的访问历史中增加一个记录。因此我们能通过浏览器的回退、前进按钮控制hash 的切换。

### 2、hash值的切换方法

#### (1) vue-router

`<router-link to="/home/index">vue点击跳转home</router-link>`

`<router-link to="/about/index">vue点击跳转about</router-link>`

#### (2) a标签

`<a href="#/home/index">点击跳转home</a>`

`<a href="#/about/index">点击跳转about</a>`

#### (3) 通过js对location.hash进行赋值

`location.hash = "#/home/index"; `

`location.hash = "#/about/index";`

### 3、通过#来了解hash

#### （1）“#”代表网页中的一个位置

其右边的字符，就是该位置的标识符。比如：http: // localhost /#/home/index就代表网页index.html的/home/index位置。浏览器读取这个URL后，会自动将/home/index位置滚动至可视区域。（单页应用）

为网页位置指定标识符，有两种方法：

使用锚点：`<a name="/home/index">home</a>`

使用id属性：`<div id="/about/index">about</div>`

#### （2）改变“#”不会触发网页重载

单单改变#后的部分，浏览器只会滚动到相应的位置，不会重新加载页面

简单模拟单页面应用跳转

```
<!DOCTYPE>

<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />

    <title>尝试锚点</title>
  </head>

  <body>
    <ul>
      <li><a href="#/home/index">点击跳转home</a></li>

      <li><a href="#/about/index">点击跳转about</a></li>
    </ul>

    <!--设置锚点方法2-->
    <a name="/home/index">home</a>

    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>
    <p>奥特曼</p>

    <div id="/about/index">about</div>

    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
    <p>打怪兽</p>
  </body>
</html>

```

#### （3）HTTP请求不包括“#”

“#”是用来知道浏览器动作的，对服务端完全无用。所以，HTTP请求中不包括#。

比如访问这个网址：http://localhost:8080/#/home/index

浏览器实际发出的请求是这样的：

![1583810142533](./img/hash请求.png)

可以看到，只是请求index.html，根本没有“#/home/index”的部分

#### （4）在第一个“#”后面出现的任何字符串，都会被浏览器解读为位置标识符

也就是意味着，这些字符串都不会被发送到服务端

比如，这个URL愿意是指定一个颜色值：http://localhost:8080/?color=#fff

但是，浏览器实际发出的请求是：![浏览器请求颜色](./img/浏览器请求颜色.png)

可以看到，“#fff”被省略了。只有将#转码为%23，浏览器才会将其作为实义字符串处理。

也就是说，上面的网址应该被写成：http://localhost:8080/?color=%23fff

#### （5）window.location.hash

window.location.hash这个属性可读可写，在读取和写入时都会识别#后面的值

读取时，可以用来判断网页状态是否改变。

![location.href](./img/location.href.png)

写入时，会在不重载网页的前提下，创在一条访问历史记录。如：

`window.location.hash = "#/home/index"; `

#### （6）onhashchange

这是一个HTML5新增的时间，当#值发生变化时，就会触发这个时间。IE8+、Firefox3.6+、Chrome5+、Safair4.0+支持该事件。

它的使用方法有三种：

```
window.onhashchange = fn
<body onhashchange="fn()">
window.addEventListener("hashchange", fn, false);
```

### 4、hash路由的核心—hashchange事件

hashchange

```
window.addEventListener("hashchange", function(cevent){
      console.log("%cevent.oldURL", "background: blue; color: white", event.oldURL);
      console.log("%cevent.newURL", "background: grey; color: white", event.newURL);
}, false)
```

根据这个特性，通过HashChangeEvent的oldURL和newURL方法可以实现当通过后退重新进入某个页面时进行二次跳转或者页面刷新等操作。

通过a标签或者通过location.hash方法改变路由的方式中，可以使用 hashchange 事件来监听 hash 值的变化，从而直接对于页面进行操作。

$\color{#FF3030}{vue 2.8.0 以上；vue 触发的 hash 改变，不会触发 hashchage 事件}$ 

```
<font color=#00ffff size=72>color=#00ffff</font>
```

<font color=#00ffff size=72>color=#00ffff</font>