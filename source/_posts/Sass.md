---
title: SASS-CSS扩展语言
date: 2017-12-15 21:14:46
categories: Front End
tags: 
- SASS
description: “Sass 是一款强化 CSS 的辅助工具“
---
# SASS入门

Sass 是一款强化 CSS 的辅助工具，它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能。    
Sass 有两种语法格式。首先是 SCSS (Sassy CSS) —— 也是本文示例所使用的格式 —— 这种格式仅在 CSS3 语法的基础上进行拓展，所有 CSS3 语法在 SCSS 中都是通用的，同时加入 Sass 的特色功能。

# 安装 Ruby

如果你使用的是 Windows， 就需要先安装 [Ruby](http://rubyinstaller.org/downloads)   

> 记得勾选（一般默认勾选）添加到系统环境变量中
> - [x] Add Ruby executables to your PATH

检测 Ruby 安装是否成功

```
C:\Users\LEE>ruby -v
ruby 2.4.2p198 (2017-09-14 revision 59899) [x64-mingw32]
```

在命令行中运行Sass安装命令
```
gem install sass
```
检测 Sass 安装是否成功

```
C:\Users\LEE>sass -v
Sass 3.5.4 (Bleeding Edge)
```
安装 Sublime Text 插件

Sublime Text    
--- Ctrl + Shift + P     
--- Package Control:Install Package     
--- Enter      
--- Sass    
--- SASS Build    
--- New File test.scss    
> 快捷键：Ctrl + B // 编译(同目录生成.css文件)

# 基本语法

## 变量 $  
变量用 $ 定义    

```
$radius : 4px;
$side : left;
body {
	border-#{$side}-radius:$radius;
}
```
---

```
body {
  border-left-radius: 4px; }

/*# sourceMappingURL=222.css.map */
```
> 如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中    

## 运算 
```
p{
  font: 10px/8px;             // 纯 CSS，不是除法运算
  $width: 1000px;
  width: $width/2;            // 使用了变量，是除法运算
  width: round(1.5)/2;        // 使用了函数，是除法运算
  height: (500px/2);          // 使用了圆括号，是除法运算
  margin-left: 5px + 8px/2px; // 使用了加（+）号，是除法运算
}
```
---

```
p {
  font: 10px/8px;
  width: 500px;
  width: 1;
  height: 250px;
  margin-left: 9px; }

/*# sourceMappingURL=222.css.map */
```

## 嵌套

```
p{
  a{
  	color:pink;
  	&:hover{
  		color:red;
  	}
  }
  border:{
  	color:red;
  }
}
```
---


```
p {
  border-color: red; }
  p a {
    color: pink; }
    p a:hover {
      color: red; }

/*# sourceMappingURL=222.css.map */
```
> 属性嵌套 : border后面必须加上冒号    
在嵌套的代码块内，可以使用&引用父元素。比如a:hover伪类

## 注释

SASS共有两种注释风格。

```
// 单行注释
/* CSS标准注释 */
/*!
	重要注释
*/
```
---

```
/*!
	重要注释
*/
/*# sourceMappingURL=222.css.map */
```
> 在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。

## 代码重用

### @import

引入外部文件

```
@import "foo.scss";
```
example.scss 内容
```
.example {
  color: red;
}
```

```
#main {
  @import "example";
}
```

```
#main .example {
  color: red;
}
```
### @extend

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

```
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

.seriousError {
  border-width: 3px;
}
```
### @mixin

@includ e命令，调用 @mixin

```
@mixin rounded($vert, $horz, $radius: 10px){//指定参数和缺省值
　　border-#{$vert}-#{$horz}-radius: $radius;
　　-moz-border-radius-#{$vert}-#{$horz}: $radius;
　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
}
#footer{
	@include rounded(top, left, 5px);
}
```

```
#footer {
　　border-top-left-radius: 5px;
　　-moz-border-radius-top-left: 5px;
　　-webkit-border-top-left-radius: 5px; }

/*# sourceMappingURL=222.css.map */
```
---
```
@mixin box-shadow($shadows...) {
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}

.shadows {
  @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```

```
.shadows {
  -moz-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  -webkit-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
}
```
---
```
@mixin colors($text, $background, $border) {
  color: $text;
  background-color: $background;
  border-color: $border;
}

$values: #ff0000, #00ff00, #0000ff;
.primary {
  @include colors($values...);
}
```

```
.primary {
  color: #ff0000;
  background-color: #00ff00;
  border-color: #0000ff;
}
```

### @if

```
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
```

```
p {
  border: 1px solid; }
```
### @else if

```
$type: monster;
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```

```
p {
  color: green; }

/*# sourceMappingURL=222.css.map */
```
### @for

```
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
```

```
.item-1 {
  width: 2em; }

.item-2 {
  width: 4em; }

.item-3 {
  width: 6em; }

/*# sourceMappingURL=222.css.map */
```
### @each

```
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

```
.puma-icon {
  background-image: url("/images/puma.png"); }

.sea-slug-icon {
  background-image: url("/images/sea-slug.png"); }

.egret-icon {
  background-image: url("/images/egret.png"); }

.salamander-icon {
  background-image: url("/images/salamander.png"); }

/*# sourceMappingURL=222.css.map */
```
### @while

```
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

```
.item-6 {
  width: 12em; }

.item-4 {
  width: 8em; }

.item-2 {
  width: 4em; }

/*# sourceMappingURL=222.css.map */
```
## 函数

内置函数

```
p {
  color: hsl(0, 100%, 50%);
}
```

```
p {
  color: #ff0000; }
```
自定义函数

```
$grid-width: 40px;
$gutter-width: 10px;

@function grid-width($n) {
  @return $n * $grid-width + ($n - 1) * $gutter-width;
}

#sidebar { width: grid-width(5); }
```

```
#sidebar {
  width: 240px; }

/*# sourceMappingURL=222.css.map */
```


























