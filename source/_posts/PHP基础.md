---
title: 世界上最好的语言-PHP
date: 2016-11-20 21:14:46
categories: PHP
tags: 
- PHP
- 后端
description: “PHP是世界上最好的语言“
---
# PHP	

PHP（外文名:PHP: Hypertext Preprocessor，中文名：“超文本预处理器”）是一种通用开源脚本语言.

# 分界标示符

PHP分界标示符确定PHP脚本的开始和结束位置
1.	PHP标准分界符：<?php ?>
2.	PHP简写分界符：<?    ?>

# 标识符/关键字/数据类型

## 标识符

标识符：由字符，数字，下划线组成，首字母必须是字符或下划线。 
1.	变量以$开头。   $score = 10;
2.	PHP是区分大小写的
3.	语句以分号结束

## 数据类型

1.	字符串(string)
2.	整数(integer)
整数规则：
- 整数必须有至少一个数字(0-9)
- 整数不能包含逗号或空格
- 整数不能有小数点
- 整数正负均可
- 可以用一种格式规定整数：十进制、十六进制（前缀是0x）或八进制（前缀是0）
3. 浮点数(double):是有小数点或指数形式的数字。 
4. 布尔(boolean)：是true或false
- 常用于条件测试。
5. 数组(array)：在一个变量中存储多个值。
6. 对象(object)：是存储数据和有关如何处理数据的信息的数据类型。
7. NULL：特殊的NULL值表示变量无值。
- NULL值标示变量是否为空。也用于区分空字符串与空值数据库。
> 定义文件头，防止出现乱码：   
> //定义文件头   
> 	header("content-type:text/html;charset=utf-8");

# 函数

PHP的真正力量来自它的函数：它拥有超过1000个内建的函数

除了内置函数 在PHP创建用户定义函数
1.	用户定义的函数声明以function开头：
2.	语法

```
function fnName(){
    //被执行的代码;
}
```

> 函数名能够以字母或下划线开头（而非数字）。    
> 函数名对大小写不敏感。

PHP函数参数

1.	可以通过参数向函数传递信息。
2.	参数被定义在函数名之后，括号内部。可以添加任意多参数，只要用逗号隔开即可。


```
function fn($a,$b){
	return $a > $b ? $a : $b;
}
echo fn(2,3);
```


PHP函数的返回值

1.	如需使函数返回值，使用 return 语句

# 数组

## 在PHP中创建数组

1.	在PHP中，array()函数用于创建数组：

```
array();
```

2.	在PHP中，有三种数组类型：
1)	索引数组：带有数字索引的数组
2)	关联数组：带有指定键的数组
3)	多维数组：数组嵌套数组

### 索引数组

1.	索引是自动分配的（索引从0开始）：

```
$cars = array("Volvo","BMW","BYD");
```

2.	手动分配索引：

```
$cars[0] = "Volvo";
$cars[1] = "BMW";
$cars[2] = "BYD";
```

3.	获得数组的长度：count()函数

count()函数用于返回数组的长度（元素数）

```
<?php
$cars = array("Volvo","BMW”,"BYD");
echo count($cars);
?>
```
4. 遍历数组


```
$arr = array(1,2,3,4,5);
echo count($arr);
for( $i = 1 ; $i <= count($arr) ; $i ++ ){
    if( $i % 2 ){
	    echo $i." "; //  .  拼接字符串
	}
}
foreach( $arr as $val ){
    echo $val . " ";
}
```


### 关联数组

使用分配给数组的指定键的数组

```
$age = array("Mary" => "18","Tom" => "19","Jack" => "20");
$age["Mary"] = "18";
$age["Tom"] = "19";
$age["Jack"] = "20";
```

数组的打印    

```
print_r(数组名)
```

# 页面输出

使用 print 只能输出一个字符串，并始终返回1

```
<?php
print "<h2>PHP is fun!</h2>"";
print "Hello world!<br/>"";
print "I’m about to learn PHP!"";
?>
```

使用 echo 能够输出一个以上的字符串

```
<?php
echo "<h2>PHP is fun!</h2>"";
echo "Hello world!<br/>"";
echo "This", " string"," was"," made"," with multiple parameters.";
?>
```
> 注：echo比print稍快，因为它不返回任何值。


