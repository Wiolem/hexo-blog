---
title: 原生 JavaScript 封装 jsonp 请求
date: 2018-1-10 21:14:46
categories: Front End
tags: 
- jsonp
- 跨域
description: JSONP 是 JSON 的一种使用模式，可以解决主流浏览器的跨域数据访问问题。
---
# 跨域请求

浏览器的同源策略，出于防范跨站脚本的攻击，禁止客户端脚本（如 JavaScript）对不同域的服务进行跨站调用。
> 同源：协议名protocol、 主机host、 端口号port

# jsonp

JSONP 是 JSON 的一种使用模式，可以解决主流浏览器的跨域数据访问问题。原理是根据 XmlHttpRequest 对象受到同源策略的影响，而 script 标签元素却不受同源策略影响，可以加载跨域服务器上的脚本，网页可以从其他来源动态产生 JSON 资源。用 JSONP 获取的不是 JSON 数据，而是可以直接运行的 JavaScript 语句。

# 实现

我们现在来实现一下，首先我们得有两个服务器来形成跨域，为了简便可以使用 node 的核心模块 http 来创建两个服务器，监听不同端口

> [示例代码(GitHub)](https://github.com/Wiolem/JavaScript-Practice/tree/master/jsonp)

client.js 

```
const http = require("http")
const server = http.createServer((req, res) => {
  res.end('client')
})

server.listen({port: 8000})

module.exports = server

```
server.js

```
const http = require("http")
const server = http.createServer((req, res) => {
  res.end('server')
})

server.listen({port: 9000})

module.exports = server

```

启动服务

```
node client.js
```

```
node server.js
```

新建一个 index.html 页面，并配好一个简易路由

client.js

```
const http = require("http")
const url = require("url")
const fs = require("fs")
const server = http.createServer((req, res) => {
  if (req.url !== "/favicon.ico") {
    const {pathname, query} = url.parse(req.url, true)
    if (pathname === "/") {
        let result = fs.readFileSync("./index.html","utf-8")
        res.setHeader("Content-Type","text/html")
        res.end(result)
      }
    }
})

server.listen({port: 8000})

module.exports = server

```

index.html 使用 jsonp 请求，先封装一个 jsonp 请求的方法

```
<script type="text/javascript">
  var jsonp = function(src, cb, callback, data) {
    // 1. create script tag
    var script = document.createElement("script")
    // 2. create randomCallback
    var randomCallback = "callback" + parseInt(Math.random() * 10000)
    // 3. create random global function
    window[randomCallback] = function(res) {
      callback(res)
    }
    // 4. http://www.baidu.com?callback=randomcallcak
    src = data ? src + "?" + cb + "=" + randomCallback + "&" + data : src + "?" + cb + "=" + randomCallback
    script.src = src
    // 5. mount script
    document.body.appendChild(script)
    // 6. script onload success
    script.onload = function() {
      setTimeout(function() {
        // 7. delete script tag
        script.remove()
        // 8. delete global function
        delete window[randomCallback]
      }, 100)
    }
  }
</script>
```

jsonp 需要后端的配合才可以实现，所以需要后端来拼接一个可执行回调

server.js

```
const http = require("http")
const url = require("url")
const server = http.createServer((req, res) => {
  const {query} = url.parse(req.url, true)
  const callback = query.callback
  res.end(`${callback}({ret: true, data: []})`)
})

server.listen({port: 9000})

module.exports = server

```

这样我们前端请求就可以执行那个成功回调了

```
jsonp('http://localhost:9000', 'callback', function (res) {
  document.getElementById('data').innerHTML = JSON.stringify(res)
})
```

# 注意点

- jsonp 只能使用 GET 方法发起请求，这是由于 script 标签自身的限制决定的
- 虽然 jQuery 是通过 $.ajax 来调用，但是 jsonp 实现的跨域调用不是通过 XmlHttpRequset 对象，而是通过 script 标签，所以在实现原理上，jsonp 和 ajax 已经一点关系都没有了
