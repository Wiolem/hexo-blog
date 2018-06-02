---
title: 前端模块化（AMD和CMD）
date: 2016-12-20 21:14:46
categories: Front End
tags: 
- 模块化
description: “前端模块规范有三种：CommonJs,AMD和CMD。“
---

## 前端模块化

前端模块规范有三种：CommonJs,AMD和CMD。

CommonJs用在服务器端，AMD和CMD用在浏览器环境 

入口文件 : 操作页面元素 , 并调用模块中的某些功能  

模块化开发 :   

三层   

一层：界面表现   

二层（中间层）：入口 , 操作页面元素 , 调用（底层）模块中的功能 

三层（底层）：具体功能实现 （不会直接操作页面元素）

## requirejs（AMD） 

AMD ：requirejs所倡导的就是AMD开发方式
     是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。

requirejs 采用异步方式加载模块，模块的加载不影响它后面语句的运行。

所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。     

模块化开发 ，可以一次性引入多个模块文件 ，同时也没有引入的顺序。

require.js 加载的模块，采用AMD规范。也就是说，模块必须按照 AMD 的规定来写。      

具体来说，就是模块必须采用特定的 define() 函数来定义。如果一个模块不依赖其他模块，那么可以直接定义在 define() 函数之中。

AMD推崇的是依赖前置

AMD:提前执行（异步加载：依赖先执行）+ 延迟执行

## require.js 使用

下载 [require.js](http://requirejs.org/docs/download.html#requirejs) 

1. 在页面中引入：
     
   ```
   <script src="require.js" data-main="main"></script>
   ```

   > data-main的值是一个入口文件 , main 实际上就是 main.js

2. 入口函数实现：

   通过cinfig方法为模块指定别名

   ```
   requirejs.config({//为引入的模块指定别名
       paths:{
           jquery:"jquery-1.11.1.min",//为jquery库文件指定一个别名，方便后期调
           vd : "validate"
       }
   })
   ```
   > 引入时：需要注意，引入顺序，频繁引入    
   > jquery.js   
   > ajax.js     
   > 如果后续 .js 文件用到之前的 .js ,就在其后面引入
3. 通过 requirejs() 方法将写好的模块引入,并根据他们编写子代码

   有两个参数   
   第一个参数是一个==数组==：引入的每个模块名称，   
   第二个参数是一个==回调函数==，函数中就是页面要实现某个功能的子代码
   
   ```
   requirejs(["jquery","vd"],function($,vd){
        //操作页面元素 , 并调用功能
    	$("body").css("background","pink");
    	//调用 outer.js 方法
    	console.log(vd.equal(1,2);
   })
   ```

4. define() 用它编写模块（功能），在相应的地方进行引入   

   有两个参数   
   第一个参数是该模块要依赖的其它模块，是一个==数组==  （可以省略）   
   第二个参数是一个==回调函数==  函数中实现该模块的功能
   
    ```
    define(function(){
    	return {
    		equal : function(a,b){
			    return a > b ? a : b;
		    }
    	}
    })
    ```
## 关于 requirejs

1. 整个项目中一般不会出现全局变量（防止全局变量的污染）
2. 整个项目中所有模块的项目加载顺序不固定（顺序任意）
3. 整个项目中模块之间的执行是异步的
4. 整个项目实现代码均是面向对象的书写方式，便于模块的管理和维护

# Seajs（CMD）

## Seajs(CMD)

CMD： Seajs 所倡导的都是CMD模块定义规范。 所有的JavaScript都遵循这个规范。

CMD（Common Module Definition）   该规范明确了模块的基本书写格式和基本交互规则。

CMD推崇的是依赖就近

CMD:延迟执行（运行到需加载，根据顺序执行）

## Seajs 使用

下载 [Seajs](https://seajs.github.io/seajs/docs/#downloads)

```
<script src="sea.js"></script>
<script>
	seajs.use(["main"]);//引入入口文件
</script>
```
main.js

```
define(function(require){
	//引入外部模块 , 并起别名
	var $ = require("jquery-3.2.1.min");
	var out = require("outer");
	//操作页面元素 , 并调用功能
	$("body").css("background","pink");
	//调用 outer.js 方法
	console.log(out.equal(1,2);
})
```
jquery.js

```
define(function(){
    //jquery 完整代码包裹在 define 中
    return $.noConflict();
})
```

outer.js

```
define(function(require,exports,module){
	//exports , module 作用 ：负责模块的暴露
	//具体功能实现
	var obj = {
		equal : function(a,b){
			return a > b ? a : b;
		}
	};
	module.exports = obj;//模块暴露  
})
```


