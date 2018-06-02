---
title: 相识-Ajax
date: 2016-11-20 21:14:46
categories: Front End
tags: 
- Ajax
- 网络请求
description: “异步的JavaScript And XML“
---
# Ajax
什么是Ajax ： 异步的JavaScript And XML    

Ajax 作用 ： 当向服务器提交少量的数据时， 可以让整个页面没有刷新 （局部刷新）   ，  服务器处理数据速度快  减少带宽 (页面无刷新)

异步和同步

异步 ： 同时执行  (生活中的同步)
       非阻塞模式  （后面代码的执行不受前面代码影响）    
同步 ： 按顺序执行  （生活中的异步）
       阻塞模式  （后面的代码收到前面代码的影响）

ajax提交数据方式


```
<form method = "post/get">
```
   
get :  通过url传递数据   操作的数据量小    安全性较低    
post :  非url传值     操作数据量大        安全性较高

## ajax实现过程

ajax从服务器获取数据 ：    
ajax请求过程：    
1. 创建一个ajax对象
2. 向服务器发出请求  （建立和服务器连接）   
open();    
三个参数：   
请求方式  get/post  
请求的url  
请求异步  默认 为异步
3. 发送请求  
 send();     
4. 服务器响应客户端请求  并返回请求的结果   
服务器返回到客户端的结果 在  ajax.responseText  中存放
异步执行 ， 通过 onreadystatechange 实现

## 缓存    

当浏览器通过ajax请求数据时，多次请求路径没有发生变化，数据就会存入到浏览器的缓存上   多次请求ajax时，路径相同直接从缓存上拿数据    
解决缓存问题 ： 在路径上带一个随机参数    
Math.random()    
new Date().getTime()

如果向服务器发送中文 ， 可以通过    encodeURI("李小枫")  解决编码问题(ie浏览器)

## 同源策略

同源 ： 同一个来源    
来源 ： ajax请求数据时，保证 协议  、 域名 、端口号 完全一致才可以请求数据
http://127.0.0.1:80   
https://127.0.0.1:80   协议不一致  就不是同一个来源   

同源策略 ： 是浏览器的一个安全机制  保证用户数据的安全性   （浏览器的行业标准）
受同源策略的影响，ajax不能实现跨域请求数据    
跨域 ： 在一个目录下访问另一个目录的数据   

## 跨域请求数据  jsonp 

json  padding  数据的一种访问格式   
jsonp跨域原理：
- 通过动态的创建script标签   添加到body中
- 设置script标签的src属性值为接口路径
- 接口路径上有一个回调函数，通过回调函数获取服务器的数据

jsonp接口：百度搜索
jsonp格式接口：
    接口路径：https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd="+txt+"&cb=fn
- wd 参数值为用户搜索的数据值
- cb 为callback回调函数

## 服务器上实现跨域请求 cors跨域

设置一个请求头 header("Access-Control-Allow-Origin:*");    
含义 ： 表示该接口文件在任何域下都可以访问

## ajax的post 

如果从服务器获取数据，和get方式没有区别    
如果向服务器发送数据， 和服务器建立连接后，需要设置一个请求头 setRequestHeader()

## 异步编程  Promise对象

Promise对象的三种状态 ：   
pending  进行中   
resolved 完成      执行 success()方法 ，成功后 继续执行 then方法   
rejected 失败      执行 failed() 方法 ,失败后 继续执行 catch方法    
promise对象特点 ：不可逆的

## 原生JS封装Ajax

```
var Ajax = {
    get : function(url, callback, data){
        var ajax = null;
        if (window.XMLHttpRequest) {
            ajax = new XMLHttpRequest();
        } else {
            ajax = new ActiveXObject("Microsoft.XMLHTTP");
        }
        if (data) { // data has value
            url = url + "?" + data;
        }
        ajax.open("GET", url, true);
        ajax.send();
        ajax.onreadystatechange = function() {
            if (ajax.readyState == 4 && ajax.status == 200) {
                callback(ajax.responseText);
            }
        }
    },
    post : function(url, callback, data){
        var ajax = new XMLHttpRequest();
        ajax.open("POST", url, true);
        ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        ajax.send(data);
        ajax.onreadystatechange = function() {
            if (ajax.readyState == 4 && ajax.status == 200) {
                callback(ajax.responseText);
            }
        }
    },
    getByPromise : function(url, data){
        var promise = new Promise(function(success, failed) {
            var ajax = null;
            if (window.XMLHttpRequest) {
                ajax = new XMLHttpRequest();
            } else {
                ajax = new ActiveXObject("Microsoft.XMLHTTP");
            }
            if (data) { // data has value
                url = url + "?" + data;
            }
            ajax.open("GET", url, true);
            ajax.send();
            ajax.onreadystatechange = function() {
                if (ajax.readyState == 4 && ajax.status == 200) {
                    success(ajax.responseText);
                }
            }
            setTimeout(function() {
                failed("服务器响应无效");
            }, 5000);
        });
    return promise;
    }
}
```





