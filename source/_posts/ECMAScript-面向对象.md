---
title: ECMAScript-面向对象
date: 2016-11-30 21:14:46
categories: Front End
tags: 
- 面向对象
description: “无序属性的集合，其属性可以包含基本值、对象或者函数。“
---
## 面向对象

面向对象编程 ： 是一种编程思想    

面向对象好处：
- 功能独立
- 防止全局变量的污染
- 便于项目后期的维护   


> 所有的对象类型都可以动态的添加属性和方法

> 类 : 具有相同的属性和行为的一类事物的总称   
> （js中没有类的概念  类--> 构造函数）   

> 对象和类的关系 ： 
> 类是对象的抽象化  对象是类的具象化 

## 对象创建

最简单的构造函数式自定义对象创建 

```
var obj =  new Object();
obj.name = "wiolem";
obj.age = 16;
```
最便捷的字面量式自定义对象的创建

```
var obj{
    name : "wiolem",
    age : 16
}
```
> 创建的对象是唯一，如果创建多个同类的对象，代码的重复率很高

解决方案 ：工厂模式创建对象

```
function createObj(name,age){
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    return obj;
}
var obj1 = createObj("wiolem",16);
var obj2 = createObj("wiolem2",16);
```
> 不符合对象的创建规范   我喜欢new一个对象   
> 不能确定某个对象属于某个具体的类

解决方案 ：构造函数创建对象

原生构造函数创建

```
var obj = new Object();
var arr = new Array();
var str = new String();
```
自定义构造函数创建

```
function Person(name,age){
    this.name = name;
    this.age = age;
    this.say = function(){
        console.log(this.name);
    }
}
var person1 = new Person("wiolem",16);
var person2 = new Person("wiolem2",16);
```
为了与普通函数区别 ：构造函数名称格式为大驼峰命名 首字母大写    
创建新实例必须使用 new 操作符 此过程经历四个步骤 ：
1. 创建一个新对象;
2. 将构造函数的作用域赋给新对象(因此this 就指向了这个新对象)
3. 执行构造函数中的代码(为这个新对象添加属性);
4. 返回新对象.

```
console.log(person1 instanceof Object);//true
console.log(person1 instanceof Person);//true
console.log(person2 instanceof Object);//true
console.log(person2 instanceof Person);//true
```
创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型    
> 以这种方式定义的构造函数是定义在Global 对象（在浏览器中是window 对象）
> 中的

1. 自定义构造函数调用方式
```
// 构造函数式调用
var person1 = new Person("wiolem",16);
person1.say();//wiolem
// 普通函数调用
Person("wiolem",16);//添加到window
window.say();//wiolem
// 另一个对象的作用域中调用
var obj = new Object();
Person.call(obj,"wiolem",16);
obj.say();//wiolem
```
> 当在全局作用域中调用一个函数时，this 对象总是指向Global 对象（在
> 浏览器中就是window 对象 window 对象就拥有了所有属性和方法   
> 同理：使用call()（或者apply()）在某个特殊对象的作用域中
> 调用Person()函数.在对象 obj 的作用域中调用的，因此调用后 obj 
> 就拥有了所有属性和方法.
2. 构造函数实例方法问题  

万物皆对象 

> 每个Person 实例都包含一个不同的Function 实例（以
> 显示name 属性）的本质。以这种方式创建函数，会导致不同的作用域链和标识符解析，但创建Function 新实例的机制仍然是相同的

```
function Person(name,age){
    this.name = name;
    this.age = age;
    this.say = new Function("console.log(this.name)");
}
console.log(person1.say() == person2.say())//false
```
可以通过在全局作用域中创建函数来解决这个两个函数解决一个功能的问题
```
function Person(name,age){
    this.name = name;
    this.age = age;
    this.say = sayName;
}
function sayName(){
    console.log(this.name);
}
console.log(person1.say() == person2.say())//true
```
但是很明显如果多个实例函数需要多个全局函数 方法欠妥

解决方案 ：原型模式创建对象

```
function Person(){
}
Person.prototype.name = "wiolem";
Person.prototype.age = 16;
Person.prototype.say = function(){
    console.log(this.name);
}
var person1 = new Person("wiolem",16);
var person2 = new Person("wiolem2",16);
console.log(person1.say() == person2.say())//true
```
> 新对象的这些属性和方法是由所有实例共享的.   
> person1 和person2 访问的都是同一组属性和同一个say()函数.

原型对象   

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象

```
person1.name = "wiolem3";
console.log(person1.name);//wiolem3---来自实例
console.log(person1.hasOwnProperty("name"));//true---判断是否来自实例
console.log(person2.name);//wiolem2---来自原型
console.log("name" in person1);//true
console.log("name" in person2);//true
delete person1.name;//delete操作符删除实例属性
console.log(person1.name);//wiolem1---来自原型
```
in --- 在对象或者原型上能访问到 返回true    
同时使用 hasOwnProperty() 方法和 in 操作符，就可以确定该属性存在哪里
```
function hasPrototypeProperty(object, name){
    return !object.hasOwnProperty(name) && (name in object);
}
```
原型语法   
字面量对象形式

```
Person.prototype = {
    name : "wiolem",
    age : 16,
    say : function(){
        console.log(this.name);
    }
};
```
> constructor 属性不再指向 Person    
> constructor 属性也就变成了新对象的 constructor 属性（指向Object 构造函数）

```
console.log(person1 instanceof Object); //true
console.log(person1 instanceof Person); //true
console.log(person1.constructor == Person); //false
console.log(person1.constructor == Object); //true
```

```
function Person(){
    
}
var person1 = new Person();
Person.prototype = {
    constructor : Person,
    name : "wiolem",
    age : 16,
    say : function(){
        console.log(this.name);
    }
};
person1.say();//error
```
> 实例中的指针仅指向原型，而不指向构造函数   

原生对象的原型   

```
Array.prototype.uniq = function(){
	return Array.from(new Set(this));
}
console.log([1,1,1,3,3,4].uniq());
String.prototype.strip = function(){
	return this.replace(/^\s+|\s+$/g,"");
}
console.log("    dd dd d   ".strip());//dd dd d
```
原型对象虽解决了多个同类对象创建时，方法共享（方法不会被重新创建，节省了内容空间）的问题，但是由于原型中的所有属性都是共享的，这样创建多个对象的属性值相同 , 引用类型值更麻烦。

```
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.prototype = {
    constructor : Person;
    friends : ["Tom","Jack"];
    say : function(){
        console.log(this.name);
}
var person1 = new Person("wiolem",16);
var person2 = new Person("wiolem2",16);
person1.friends.push("Mary");
console.log(person1.friends);//"Tom","Jack","Mary"
console.log(person2.friends);//"Tom","Jack","Mary"
```

解决方案 ：组合使用构造函数和原型模式创建对象   

构造函数模式用于定义实例属性   
原型模式用于定义方法和共享的属性

```
function Person(name,age){
    this.name = name;
    this.age = age;
    this.friends = ["Tom","Jack"];
}
Person.prototype = {
    constructor : Person;
    say : function(){
        console.log(this.name);
}
var person1 = new Person("wiolem",16);
var person2 = new Person("wiolem2",16);
person1.friends.push("Mary");
console.log(person1.friends);//"Tom","Jack","Mary"
console.log(person2.friends);//"Tom","Jack"
console.log(person1.say == pperson2.say);//true
```
## 继承

ECMAScript 只支持实现继承，而且其实现继承主要是依靠原型链来实现的。

原型链 ：实例对象和原型之间的链接   
> 表示：_ _ proto _ _

原型继承

```
function Person(){
    this.isPerson = true;
}
Person.prototype.getSuperValue = function(){
    return this.isPerson;
}
function Child(){
    this.isOld = false;
}
Child.prototype = new Person();
Child.protoytype.getChildValue = function(){
    return this.isOld;
}
var child = new Child();
console.log(child.getSuperValue());//true
```

构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针

原型模式的执行流程 ：
首先在实例中查找，如果实例中没有，就去原型中查找  如果原型中还没有，就去  Object.prototype  查找 如果有，就返回.

确定原型与实例关系

除了instanceOf 操作符还可以用原型中 isPrototypeOf 方法

```
console.log(Object.prototype.isPrototypeOf(child)); //true
console.log(Person.prototype.isPrototypeOf(child)); //true
console.log(Child.prototype.isPrototypeOf(child)); //true
```
定义方法

```
function Child(){
    this.isOld = false;
}
Child.prototype = new Person();
Child.protoytype.getChildValue = function(){
    return this.isOld;
}
// 重写父类方法
Child.prototype.getSuperValue = function(){
    return false;
}
var child = new Child();
console.log(child.getSuperValue());//false
var person = new Person();
console.log(person.getSuperValue());//true
```
> 原型添加方法的代码一定要放在替换原型的语句之后    
> 无法使用字面量形式创建原型方法

```
Child.prototype = new Person();
Child.protoytype = {
    getChildValue : function(){
        return this.isOld;
}
var child = new Child();
console.log(child.getSuperValue());//error
```
原型继承同样也会有引用类型共享的麻烦,并且无法向父类传参

解决方案 ：构造函数继承

```
function Person(name){
    this.name = name;
    this.types = ["man","woman","child"];
}
function Child(){
    //继承
    Person.call(this,"wiolem");
}
var child1 = new Child();
child1.types.push("old");
console.log(child1.types);//"man","woman","child","old"
var child2 = new Child();
console.log(child2.types);//"man","woman","child"
```
虽然解决了共享和传参的问题，但是方法都是构造函数中定义，无法复用   

解决方案 ：组合继承   

使用原型链实现对原型属性和方法的继承    
通过借用构造函数来实现对实例属性的继承

```
function Person(name){
    this.name = name;
    this.types = ["man","woman"];
}
Person.prototype.sayName = function(){
    console.log(this.name);
}
function Child(name,age){
    Person.call(this,name);
    this.age = age;
}
Child.prototype = new Person();
Child.prototype.constructor = Child;
Child.prototype.sayAge = function(){
    console.log(this.age);
}
var child1 = new Child("wiolem1",16);
child1.types.push("child");
console.log(child1.types);//"man","woman","child"
console.log(child1.sayName());//wiolem1
console.log(child1.sayAge());//16

var child2 = new Child("wiolem2",18);
console.log(child2.types);//"man","woman"
console.log(child2.sayName());//wiolem2
console.log(child2.sayAge());//18
```
























