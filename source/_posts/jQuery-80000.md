---
title: jQuery-80000
date: 2016-12-10 21:14:46
categories: Front End
toc: true
tags: 
- jQuery
description: “JavaScript的一个简单类库“
---
# jQuery
js的一个类库，使用js语法实现对某些功能的封装 ，提高开发效率   

解决 JS 兼容性差 代码量大 

jquery的引入   外部文件引入jquery库   通过 $()   

> $ === jQuery

## jQuery的页面加载   和  js的页面加载区别

jQuery的页面加载内容比js的执行速度快   
JS的onload是需要页面内容全部加载完成后才执行  
jQuery的页面加载在项目中可以出现多个，js的onload只能有一个
Js的页面加载 ：

```
$(document).ready(function(){ ... }) 
=== 
$(function(){ ... })
```
jQuery  选择器(查找页面元素)

- 基本选择器    

```
$("#id")   
$(".class")   
$("div")  
$("#id,html")  
$("*")
```

- 简单选择器

```
:first
:last
:eq() //下标从0开始
:not() //除了...之外
:odd  //查找下标为奇数的元素（偶数行）
:even //查找下标为偶数的元素（奇数行）
:has() //包含选择
```
 
- 查找过滤选择器(方法)

```
.eq()
.first() 
.last() 
.parent() // 查找父元素
.next()   // 如果没有参数 查找下一个兄弟 如果有参数，查找指定的下一个兄弟元素
.nextAll() // 如果没有参数 查找下面所有兄弟 ；如果有参数，查找指定的下面的所有兄弟
.prev()  // 前一个
.prevAll() // 以前所有
.children()  // 有参数，指定孩子元素，没有参数就查找所有孩子就
.siblings() // 查找除了自己之外的所有兄弟
.find() // 查找所有的后代元素 
//---参数必须加 .find("*") 查找所有后代
.end() //结束离end最近的前面的那个选择器
.filter() // 过滤
//用法一
$("p").filter(".p1").css("color","red") 
//过滤含有类名为p1 的p元素
//用法二
$res = $("p").filter(function(index){
    // 函数返回一个布尔值  
    // 如果值为真 说明查找到满足某个条件的p元素   
    // 如果为假  没查找到任何元素
    console.log(index);
    return index == 3;
});
$res.css("color","red")
```
- 层级选择器

```
// 空格   >   +   ~
$("div p") // 查找div中所有的后代元素p
$("div>p") // 查找div中所有的子代p
$("div+p") // 等价  $("div").next("p")
$("div~p") // 等价  $("div").nextAll("p")
```
- 表单选择器   

```
:text  
:button  
:password      
:radio:ehecked
:checkbox:checked
:selected //被选中的下拉列表
```

- 属性选择器

```
$("input[name]") //表示查找含有name属性的input   
$("input[name=qx]") //表示查找name值为qx的input   
$("input[name^=qx]") //表示查找name值以qx开始的input    
$("input[name$=qx]") //表示查找name值以qx结束的input    
$("input[name*=qx]") //表示查找name值包含qx的input   
$("input[name！=qx]") //表示查找name值不包含qx的input
```

## jQuery元素和JS元素的区别

使用jQuery方式查找的页面元素  必须通过jQuery的方法操作 样式 属性 内容.....   
使用JS方式查找的页面元素  必须通过JS的方法操作 样式 属性 内容.....  

```
// jQuery元素和JS元素互换
// jQuery对象 => JS对象
$().get(下标)
$()[下标]
// JS对象 => jQuery对象
$(obj)
```
## 常用基本操作

### CSS()

样式操作

```
$().css() //获取
$().css({}) //设置
```
### index()

获取下标

index()  表示查找某个元素在同辈元素中的位置    
如果参数是一个对象  表示该对象在原集合中的索引    
如果参数是一组对象  表示该组对象中的第一个对象在原集合中的位置

### 样式类的操作

```
addClass() //添加样式类   
removeClass() //删除样式类   
toggleClass() //切换样式类   判断某个元素是否含有某个样,有就删，没有就填   
hasClass() //判断是否含有某个样式类
```
### attr()

属性操作

attr() 获取 或设置 某元素的属性 也可以为某个元素添加自定义属性 

> 不能操作属性值为布尔类型的属性  checked  ，用 prop()   

获取 ：$().attr()   
设置 ：$().attr({}) 

### 内容操作

html()
```
$().html("<b>")
```
> 识别标签 获取内容时 只能获取第一个元素内容    

text()  
```
$().text("<b>")
```
> 不识别标签 获取查找到的所有的元素内容 

val() 操作表单
```
val() //内容获取;
val(内容) //内容设置 
```
### 文档操作    

```
append() // 向某个元素的内部追加新内容 父节点.append(子节点)
appendTo() // 子节点.appendTo( 父节点 )

prepend() // 向某个元素的内部前置新内容 用法同append()
prependTo()

after() // 向某个元素的下面追加新内容 原有页面元素.after( 新元素 )   元素之间兄弟关系
insertAfter() // 新元素.insertAfter(原有页面元素)

before() // 向某个元素的前面追加新内容 用法同after
insertBefore()
```
### 动画

显示/隐藏

```
show()
//第一个参数 : 时间  毫秒值
//第二个参数 ：运动方式  swing linear
//第三个参数 ：运动完成后的回调函数
hide()
toggle()
```
上拉/下拉

```
slideUp() //通过高度的改变动态的隐藏某个元素
slideDown() //通过高度的改变动态的显示某个元素
slideToggle() //切换
```
淡入/淡出

```
fadeIn()  //淡入（显示）
fadeOut() //淡出 （隐藏）
fadeToggle()
```
透明度

```
fadeTo()
//第一个参数 ：时间  不能省 没有过度时间 写 0 
//第二个参数 ：透明度值 0.5
```
自定义动画

animate     
//第一个参数 ： 动画执行的方式  {  }     
//第二个参数 ： 动画完成时间     
//第三个参数 ： 回调函数   

```
animate({
	width : 400,
	height : 300,
	left : 300,
	top : 300,
	opacity : 0.3
},2000,
function(){
    //do something
})

stop() //停止动画
```
自定义动画可操作的CSS属性

```
* backgroundPosition
* borderWidth
* borderBottomWidth
* borderLeftWidth
* borderRightWidth
* borderTopWidth
* borderSpacing

* marginBottom
* marginLeft
* marginRight
* marginTop
* outlineWidth

* paddingBottom
* paddingLeft
* paddingRight
* paddingTop
* height
* width
* maxHeight
* maxWidth
* minHeight
* maxWidth
* fontSize
* bottom
* left
* right
* top
* letterSpacing
* wordSpacing
* lineHeight
* textIndent
* opacity

```
### 遍历 each()

```
$().each(function(index,ele){
//index 表示遍历的元素下标
//ele 表示dom元素
})
//each()遍历找到页面中所有 role 属性值为 title 的元素
var $res = [];
$("body").find("*").each(function(index,dom){
	if ($(dom).attr("role") == "title") {
		$res.push($(dom));
	}
});
console.log($res);
// filter 方法
var $res = $("body").find("*").filter(function(index,dom){
	return $(dom).attr("role") == "title";
});
console.log($res);
```
### offset 以及 宽和高的操作

width() | outerWidth() | innerWidth()
---|---|---
内容宽度 | 内容+边框+补白 | 内容 + 补白

```
offset() 
//结果是一个对象,对象中有两个属性left,top   
//距离文档顶部和左侧的偏移
position() 
//获取离他最近的具有定位的父级元素的左偏移和上偏移  
//如果该元素中没有含有定位的父级元素，找 body
```
### 事件

```
scroll ： 
$(window).scroll(function(){
    //滚动条事件,获取页面滚走的距离
    $(document).scrollTop() //获取页面滚走的距离
})
//设置页面滚走距离：
$(document).scrollTop(0)    

//以动画的方式回到顶部
$("body,html").animate({"scrollTop":0},1000);

```
事件绑定 bind()

```
$(obj).bind("event", function(){

})

$(obj).bind("event1 event2",function(){

})

$(obj).bind({
    event1 ： function(){ },
    event2 ：function(){ },
    .....
})

// 解除绑定 unbind 
$(obj).unbind("event")
```
事件委托（事件代理）

```
$(parent).delegate("child", "event", function(){
    
})

//解除委托 undelegate  
$(obj).undelegate("event")
```
事件绑定 + 事件委托

on 如果实现事件绑定,用法和 bind 相同;    
on 如果事件事件代理:

```
$(parent).on("event", "child", function(){
    
})
//解除绑定或委托   
off
```
鼠标悬停事件  hover

```
$(obj).hover(function(){
    //鼠标移入
},function(){
    //鼠标移出
})
```
## jQuery插件

插件的定义：

### 第一种方式:（无法操作页面元素,全局调用）

```
$.extend({
    "fun" : function([形参]){
        //功能实现~~
    }
})
//调用 
$.fun([实参])
```
### 第二种方式: (调用时通过jquery选择器来调用 )
          
```
$.fn.extend({
    fun : function(){},
    fun2 : function(){},
    fun3 : function(){},
    .....
})
```
### 拖拽插件 $(ele).drag()
```
$.fn.extend({
	drag : function(){
		$(this).mousedown(function(event){
			var evt = event || window.event;
			var X = evt.offsetX;
			var Y = evt.offsetY;
			$(document).mousemove(function(event){
				var evt = event || window.event;
				$(this).offset({
					left : evt.pageX - X,
					top : evt.pageY - Y
				});
			}.bind(this));
			$(document).mouseup(function(){
				$(this).unbind('mousemove');
			});
		});
	}
})
$("div").drag();
```

## Ajax的异步请求

### load()

load() 方法是一个局部的方法，通过jquery的选择器作为开始调用load方法

load()方法是异步的，加载的信息中如果需要事件响应，必须通过回调函数来实现，也就是说所有load加载的数据中的事件都要写在回调函数中

load()方法如果想要请求不同结构的内容，可以把这些内容写到一个文件中，请求数据时，在url后加一个请求容器的选择器名称即可（这里选择器建议使用基本选择器）


```
load()方法的参数有三个：
//第一个： 请求的url
//第二个： 发送的数据  json格式  {}
//第三个参数 ： 回调函数  
//回调函数中有三个参数，分别是：
function(res,type,xhr){
    console.log(res);//请求数据
    console.log( type );//请求的状态
    console.log(xhr);//deffered 对象（类似于js中promise对象）
}
```
### $.get || $.post

以 $. 开始的ajax请求方式是全局方法

```
$.get(url, {}, callback)  //以get方式请求数据
//第一个参数：请求的路径
//第二个参数：请求数据 json对象 {}
//第三个参数：回调函数 （回调函数中也有三个参数，同load）

$.post()  //以post方式请求数据  用法同$.get()
```
### $.getJson() || $.getScript()


```
$.getJson("xxx.json"，data, callback) 
//通过这种方式请求json数据   
//三个参数  ： url    data     function(){}
$.getScript("xxx.js", callback) 
//通过这种方式请求js脚本数据 ，立即执行请求的脚本代码
```
### $.ajax  

jQuery 的 ajax 请求服务器传递过来的数据一般都是 object 对象（请求的数据是 object 类型）


```
$.ajax()   //返回一个deffered 对象

//用法：
$.ajax({
    type:"get",
    url:"http://127.0.0.1/jqAjx1706/data.json",
    success:function(){
        console.log("成功");
    }
});
     
//常用方法一：
$.ajax({
    type:"get",
    url:"http://127.0.0.1/jqAjax1706/data.json",
    datatype:"json",//指定请求数据的类型
    data:{"name":"admin"},//向服务器发送数据
    success:function(res){
      //alert(typeof res);
      //此处处理服务器返回数据的业务逻辑
    }
});

//常用方法二、
var deffered = $.ajax({
    url:"http://127.0.0.1/jqAjax1706/data.json"
});
deffered.done(function(res){//成功之后做的事情
    console.log(typeof res);
})

//jsonp 跨域：
//https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd="+txt+"&cb=fn
$.ajax({
    type:"get",
    url:"https://api.douban.com/v2/book/search?q=css&callback=fn&start=0&count=10",
    dataType:"jsonp",
    jsonCallback:"fn"//设置回调函数
});
function fn(res){
	console.log( res );
}
```





















