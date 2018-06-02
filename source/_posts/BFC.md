---
title: BFC
date: 2016-10-30 21:14:46
categories: Front End
tags: 
- BFC
description: Box、Formatting（格式化） Context（上下文）==块级格式化上下文。
---
# BFC是什么？ 
在解释 BFC 是什么之前，需要先介绍 Box、Formatting（格式化） Context（上下文）==块级格式化上下文 的概念。
BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

# BFC布局规则：
1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
3. 每个元素的margin box的左边， 与包含块border box的左边相接触
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
6. 计算BFC的高度时，浮动元素也参与计算

# 哪些元素会生成BFC?
- 根元素
- float属性不为none
- position为absolute或fixed
- display为inline-block, table-cell, table-caption, flex, inline-flex
- overflow不为visible

# BFC的应用
## 防止margin值上下重叠

```
<style>
    *{margin:0;padding:0;}
    div{width:300px;height:200px;}
    .top{background:orange;margin-bottom:100px;}
    .bottom{background:pink;margin-top:50px;}
    .box{overflow:hidden;}
</style>
<body>
    <div class="top"></div>
    <div class="box">
        <div class="bottom"></div>
    </div>
</body>

```
## 清除内部浮动

```
<style>
    *{margin:0;padding:0;}
    .box{width:800px;background:pink;overflow:hidde;}
    p{width:200px;height:300px;background:orange;float:left;}
</style>
<body>
    <div class="box">
        <p></p>
	<p></p>
    </div>
</body>

```
## 自适应两栏布局

```
 <style>
    *{margin:0;padding:0;}
    html,body{height:100%;}
    .left{width:200px;height:100%;background:#d00;float:left;}
    .right{height:100%;background:#dd0;overflow:hidden;}
</style>
<body>
    <div class="left"></div>
    <div class="right"></div>
</body>


```


