---
title: Gulp 前端自动化工具
date: 2016-12-15 21:14:46
categories: Front End
tags: 
- Gulp
description: “Gulp.js 是一个自动化构建工具,开发者可以使用它在项目开发过程中自动执行常见任务“
---
# Gulp.js

Gulp.js 是一个自动化构建工具,开发者可以使用它在项目开发过程中自动执行常见任务。

Gulp.js 是基于 Node.js 构建的，首先安装 [Node.js](https://nodejs.org/en/) 。

查看 node 版本 

```
C:\WINDOWS\system32>node -v
v6.11.0
```
安装 Gulp.js 

首先全局安装 Gulp

```
C:\WINDOWS\system32>npm  install gulp -g
```
查看 Gulp 版本

```
C:\WINDOWS\system32>gulp -v
[14:58:11] CLI version 3.9.1
```
在 Gulp 项目文件下局部安装 Gulp 

```
C:\WINDOWS\system32>npm install gulp --save-dev
```
配置package.json文件  

```
C:\WINDOWS\system32>npm init
```
# Gulp 方法

在项目根目录下创建一个名为 gulpfile.js 的文件
> 作用：布置一些任务，通过 gulp 实现任务的调度

## task()

```
task("任务名称",[依赖的其他任务],function(){
    
})
```

```
// gulp 模块引入
var gulp = require('gulp');

//布置一个任务  say
gulp.task('say',function(){
    console.log("say任务执行");
});

//default
gulp.task('default',['say'],function(){
    console.log( "默认任务执行....." );
});
```
运行 gulp

```
D:\PHPnow\htdocs\www\demo>gulp
[15:16:56] Using gulpfile D:\PHPnow\htdocs\www\demo\gulpfile.js
[15:16:56] Starting 'say'...
say任务执行
[15:16:56] Finished 'say' after 203 μs
[15:16:56] Starting 'default'...
默认任务执行.....
[15:16:56] Finished 'default' after 145 μs
```
## src(),dest(),pipe()
src([])   源文件路径   
dest()  目标文件路径  (可以自动创建目录)    
pipe()  管道方法 表示输送(下一步)

```
//任务 : 将js目录下的所有js文件 和 css目录下的所有css文件 复制到 all目录下
gulp.task( "copy",function(){
    return gulp.src(["js/*.js","css/*.css"])
               .pipe(gulp.dest("all"));
});
//default
gulp.task('default',function(){
    gulp.start('copy');
});
```
```
D:\PHPnow\htdocs\www\demo>gulp
[15:27:22] Using gulpfile D:\PHPnow\htdocs\www\demo\gulpfile.js
[15:27:22] Starting 'default'...
[15:27:22] Starting 'copy'...
[15:27:22] Finished 'default' after 16 ms
[15:27:22] Finished 'copy' after 177 ms
```
单独执行特定的任务（task）

```
gulp <task> <othertask>
```
```
D:\PHPnow\htdocs\www\demo>gulp say copy
[15:31:54] Using gulpfile D:\PHPnow\htdocs\www\demo\gulpfile.js
[15:31:54] Starting 'say'...
say任务执行
[15:31:54] Finished 'say' after 323 μs
[15:31:54] Starting 'copy'...
[15:31:54] Finished 'copy' after 59 ms
```
## watch()

watch(监听的文件,[任务名])  监听 

```
//任务 : 实现将 index.html 复制到 html目录下
gulp.task("copyHTML",function(){
	return gulp.src("index.html")
			   .pipe( gulp.dest("html") );
});

//任务 : 监听index.html的文件内容的改变 
//如果index.html内容改变了，就自动更改 html 目录下的 index.html
gulp.task("watch",function(){
	return gulp.watch("index.html",["copyHTML"]);
});
```
```
D:\PHPnow\htdocs\www\demo>gulp watch
[15:38:57] Using gulpfile D:\PHPnow\htdocs\www\demo\gulpfile.js
[15:38:57] Starting 'watch'...
[15:38:57] Finished 'watch' after 12 ms
[15:39:08] Starting 'copyHTML'...
[15:39:08] Finished 'copyHTML' after 46 ms
[15:39:08] Starting 'copyHTML'...
[15:39:08] Finished 'copyHTML' after 15 ms
```
# Gulp 插件

插件安装：  npm install   --save-dev   插件名称
> 淘宝镜像 ：   
> npm install cnpm -g  --registry=https://registry.npm.taobao.org    
> npm指令  改为  cnpm     

常用插件

```
var concat = require("gulp-concat"); //文件合并
var cssmin = require("gulp-cssmin");//压缩css文件
var imgmin = require("gulp-imagemin");//压缩图片
var sass = require("gulp-sass"); //将scss文件转成 css文件
var uglify = require("gulp-uglify");//压缩js文件
var rename = require("gulp-rename"); //重命名
// ...
```
> TIP : 一次安装

```
npm install gulp-util gulp-imagemin gulp-ruby-sass gulp-minify-css gulp-minify-html gulp-load-plugins gulp-jshint gulp-uglify gulp-rename gulp-concat gulp-clean --save-dev
```
gulpfile.js
```
//引用包
var gulp = require('gulp'),
    cssmin = require('gulp-minify-css'),
    concat = require('gulp-concat'),
    rename = require('gulp-rename'),
    uglify = require('gulp-uglify'),
    babel = require('gulp-babel'),
    es2015 = require('babel-preset-es2015'),
    imgmin = require('gulp-imagemin'),
    htmlmin = require('gulp-htmlmin'),
    processhtml = require('gulp-processhtml'),
    eslint = require('gulp-eslint');

//编译 es6 --- 压缩 JS
gulp.task('js',function(){
    gulp.src("js/*.js")
        .pipe(babel({presets:[es2015]}))
        .pipe(rename({suffix: '.min'}))
        .pipe(uglify())
        .pipe(gulp.dest("handumin/jsmin"));
});

//压缩 CSS
gulp.task('css',function(){
    gulp.src("css/*.css")
        .pipe(cssmin())
        .pipe(concat('main.min.css'))
        .pipe(gulp.dest("handumin/cssmin"));
});

//压缩 img
gulp.task('img',function(){
    gulp.src("images/*")
        .pipe(imgmin())
        .pipe(gulp.dest("handumin/imgmin"));
});

//压缩 html
gulp.task('html',function(){
    var options = {
        removeComments: true,
        collapseWhitespace: true,
        collapseBooleanAttributes: false,
        removeEmptyAttributes: false,
        removeScriptTypeAttributes: true,
        removeStyleLinkTypeAttributes: true,
        minifyJS: true,
        minifyCSS: true
    };
    gulp.src('html/*.html')
        .pipe(processhtml())
        .pipe(htmlmin(options))
        .pipe(gulp.dest('handumin/htmlmin'));
});

//default
gulp.task('default',function(){
    gulp.start('js','css','img','html');
});
```
package.json
```
{
  "name": "lee",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "gulp": "^3.9.1"
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-preset-es2015": "^6.24.1",
    "gulp-babel": "^7.0.0",
    "gulp-clean": "^0.3.2",
    "gulp-concat": "^2.6.1",
    "gulp-cssmin": "^0.2.0",
    "gulp-eslint": "^4.0.0",
    "gulp-htmlmin": "^3.0.0",
    "gulp-imagemin": "^4.0.0",
    "gulp-jshint": "^2.0.4",
    "gulp-load-plugins": "^1.5.0",
    "gulp-minify-css": "^1.2.4",
    "gulp-minify-html": "^1.0.6",
    "gulp-processhtml": "^1.1.0",
    "gulp-rename": "^1.2.2",
    "gulp-ruby-sass": "^2.1.1",
    "gulp-sass": "^3.1.0",
    "gulp-uglify": "^3.0.0",
    "gulp-util": "^3.0.8"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```


















