---
title: Dagger2
toc: true
date: 2016-1-16 14:16:00
categories: Android
tags:
- Dagger2
- 第三方开源库
- 依赖注入
description: Dagger 是为 Android 和 Java 平台提供的在编译时进行依赖注入的框架。编译时，是指编译时自动生成所需对象注入的代码。依赖注入，是指不用 new 对象，而是靠函数参数传入对象或者函数返回值返回对象。
---

## 依赖注入

### 什么是依赖，什么是依赖注入？

如果 A 类中使用到 B 类的对象，就叫做 A 类依赖于 B 类，这就好像人依赖于大脑一样，如果人的大脑不存在，那么这个人也不会存在；如果 B 类不存在，那么 A 类也不会存在，首先编译都通不过！
依赖注入就是 B 类对象不是在 A 类中创建的，而是其它地方创建的，然后传递给 A 类使用，一般有 3 种传递方式，1.通过A类构造函数传递给A类对象；2.通过A类方法参数传递给A类对象；3.通过其它类的方法的返回值传递给A类对象。
依赖可能导致一个问题，就是变化的传递，如果 B 类发生啦变化，可能，导致 A 类也发生变化，发生这种情况，就叫做A类和B类耦合性非常高。刚刚我们写的 MVP 模式中就有这种依赖，MVP 模式中谁依赖于谁，对， V 和 P 是相互依赖的，P 和 M 是相互依赖的。

### 解决构造函数导致耦合的方式

```
1. 配置文件 ＋ 反射
2. 单例模式、工厂模式 等等
```

### Dagger2 解决构造函数耦合的实现步骤

```
1. new Presenter
2. 把 Presenter 设置给 Activity 的成员
```

![](img/architecture004.png)

## Dagger2 简介
### 什么是Dagger2
Dagger 是为 Android 和 Java 平台提供的在编译时进行依赖注入的框架。
编译时，是指编译时自动生成所需对象注入的代码。
依赖注入，是指不用 new 对象，而是靠函数参数传入对象或者函数返回值返回对象。

### 为什么使用Dagger2
Dagger2解决了基于反射带来的开发和性能上的问题。

### 做什么工作
Dagger2主要用于做界面和业务之间的隔离。

## Dagger2使用步骤
我们打算不修改 MainActivityPresenter 代码，把 MainActivityPresenter 对象注入 MainActivity 中

![](img/architecture005.png )

### 完成 Dagger2 的使用环境配置
#### 在 Project 的 build.gradle 文件添加 apt 工具的 gradle 插件

```java
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.2'
        // 添加 apt
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    }
}
```

#### 在 Module 的 build.gradle 文件引入 apt 插件和 dagger2

```java
apply plugin: 'com.android.application'
// 引入 apt 插件
apply plugin: 'com.neenbedankt.android-apt'
......
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:appcompat-v7:24.2.1'
    // 添加 dagger2
    compile 'com.google.dagger:dagger:2.6'
    apt 'com.google.dagger:dagger-compiler:2.6'
}
```

- `compile 'com.google.dagger:dagger:2.6'`是导入依赖的 dagger 相关的类
- `apt 'com.google.dagger:dagger-compiler:2.6'`是使用 apt 工具在编译的时候自动产生依赖注入相关代码，apt 会在编译的时候运行 dagger-compiler 里面的代码，来自动产生依赖注入相关的代码
- `apply plugin: 'com.neenbedankt.android-apt'`应用 apt gradle 插件
- `classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'`添加 apt 插件

### 使用 @inject 注解，标识要注入的成员

在 MainActivity 中使用 @inject 注解，标识要注入的成员

```java
public class MainActivity extends AppCompatActivity {
    @Inject
    MainActivityPresenter presenter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    ......
}
```

### 创建 XXXModule 工厂类

创建 MainActivityPresenter 类的工厂类 MainActivityPresenterModule，该工厂类可以创建 MainActivityPresenter 实例

1. 使用 @Module 注解表示该类是一个 Module 类，可以提供一些实例用于注入
2. 使用 @Provides 注解指明哪一个方法可以创建实例对象

```java
/**
* @Module注解表示这个类提供生成一些实例用于注入
*/
@Module
public class MainActivityPresenterModule {
    private MainActivity activity;

    public MainActivityModule(MainActivity activity) {
        this.activity = activity;
    }

    /**
     * @Provides 注解表示这个方法用来创建某个实例对象，方法名自己定，但一般用provideXXX结构，方法可以有参数，但还需要其它 provide 方法来给参数注入对象，这里不讲，可以参考拓展文档
     * @return 返回注入对象
     */
    @Provides
    public MainActivityPresenter provideMainActivityPresenter(){
        return new MainActivityPresenter(activity);
    }
}
```

### 创建连接器接口

创建 MainActivity 和 MainActivityPresenterModule 之间的连接器 MainActivityComponent 接口

1. 使用 @Component 注解标记一个接口，表示要是使用哪些 module 类来进行依赖注入
2. 给接口添加一个方法，用来给参数对象注入对象

```java
/**
*用 @Component 注解标识这个接口是一个连接器，能用 @Component 注解的只
*能是接口或者抽象类
*/
@Component(modules = MainActivityPresenterModule.class)
public interface MainActivityComponent {
    /**
     * 这里 in 是 inject 的缩写表示注入的意思，这个方法名可以随意更改，但建议就
     * 用in或者inject。
     */
    void in(MainActivity activity);
}
```

### 注入 MainActivityPresenter 对象

先编译项目，然后在 MainActivity 中调用 DaggerMainActivityComponent 的方法注入 MainActivityPresenter 对象

```java
 DaggerMainActivityComponent.builder()
    .mainActivityPresenterModule(new MainActivityPresenterModule(this))
    .build()
    .in(this);
```

1. DaggerMainActivityComponent类是 apt 工具自动产生的
2. DaggerMainActivityComponent.builder() 后调用 mainActivityPresenterModule 方法是给 Builder 类设置 Module 对象，由于我们的 Module 类的构造方法带有参数，所以需要我们自己 new Module 对象，否则 Module 对象是可以自动产生



## Dagger2实现注入的原理

![](img/Dagger2注入对象原理.png )



## 拓展和参考资料
- [Dagger2 使用详解](http://www.jianshu.com/p/94d47da32656)
- [带着疑惑走进Dagger2](http://blog.csdn.net/ghost_programmer/article/details/52416432)
