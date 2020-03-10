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

#### 1、“#”代表网页中的一个位置

其右边的字符，就是该位置的标识符。比如：http: // localhost /#/home/index就代表网页index.html的/home/index位置。浏览器读取这个URL后，会自动将/home/index位置滚动至可视区域。（单页应用）

为网页位置指定标识符，有两种方法：

使用锚点：`<a name="/home/index">home</a>`

使用id属性：`<div id="/about/index">about</div>`

#### 2、改变“#”不会触发网页重载

单单改变#后的部分，浏览器只会滚动到相应的位置，不会重新加载页面

简单

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

#### 3、HTTP请求不包括“#”

“#”是用来知道浏览器动作的，对服务端完全无用。所以，HTTP请求中不包括#