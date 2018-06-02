---
title: Java基础知识
date: 2016-01-15 21:14:46
categories: Java
tags: 
- Java
description: Java基础知识准备一下，搜集了很多。
---
## Java数据类型
- 基本数据类型
    - 数值型
        - 整数类型(byte,short,int,long)
        - 浮点类型(float,double)
    - 字符型(char)
    - 布尔型(boolean)
- 引用数据类型
	- 类(class)
    - 接口(interface)
    - 数组　

## switch参数可以是哪些
- byte
- short
- int
- char
- enum
- String

## int和Integer的区别
  - Integer是int提供的封装类，而int是Java的基本数据类型；
  - Integer默认值是null，而int默认值是0；
  - 声明为Integer的变量需要实例化，而声明为int的变量不需要实例化；
  - Integer是对象，用一个引用指向这个对象，而int是基本类型，直接存储数值。
  
## 什么是值传递和引用传递？
  - 值传递是对基本型变量而言的,传递的是该变量的一个副本,改变副本不影响原变量. 
  - 引用传递一般是对于对象型变量而言的,传递的是该对象地址的一个副本, 并不是原对象本身 。 
  - 一般认为,java内的传递都是值传递. java中实例对象的传递是引用传递
  
## "static"关键字是什么意思？Java中是否可以覆盖(override)一个private或者是static的方法？
  - “static”关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例变量的情况下被访问。 
  - Java中static方法不能被覆盖，因为方法覆盖是基于运行时动态绑定的，而static方法是编译时静态绑定的。static方法跟类的任何实例都不相关，所以概念上不适用。 
  java中也不可以覆盖private的方法，因为private修饰的变量和方法只能在当前类中使用，如果是其他的类继承当前类是不能访问到private变量或方法的，当然也不能覆盖。

## 是否可以在static环境中访问非static变量？
  - static变量在Java中是属于类的，它在所有的实例中的值是一样的。当类被Java虚拟机载入的时候，会对static变量进行初始化。如果你的代码尝试不用实例来访问非static的变量，编译器会报错，因为这些变量还没有被创建出来，还没有跟任何实例关联上。

## String和StringBuffer、StringBuilder
  - 可变性
    - String类是用字符数组保存字符串，即private final char value[]，所以String对象不可变
    - StringBuffer类与StringBuilder类都是继承AbstractStringBuilder类，AbstractStringBuilder是用字符数组保存字符串，即char value[]，所以StringBuffer与StringBuilder对象是可变的
  - 线程安全性
    - String类对象不可变，所以理解为常量，线程安全
    - StringBuffer类对方法加了同步锁，线程安全
    - StringBuilder类对方法未加同步锁，线程不安全
  - 性能
    - String类进行改变的时候，都会产生新的String对象，然后将指针指向新的String对象
    - StringBuffer进行改变的时候，都会复用自身对象，性能比String高

## Synchronized如何使用
  - Synchronized是Java的关键字，是一种同步锁，它可以修饰的对象有以下几种
  - 修饰代码块：该代码块被称为同步代码块，作用的主要对象是调用这个代码块的对象
  - 修饰方法：该方法称为同步方法，作用的主要对象是调用这个方法的对象
  - 修饰静态方法：作用范围为整个静态方法，作用的主要对象为这个类的所有对象
  - 修饰类：作用范围为Synchronized后面括号括起来的部分，作用的主要对象为这个类的所有对象

## Synchronized和Lock的区别
  - 相同点：Lock能完成Synchronized所实现的所有功能 
  - 不同点：
      - Synchronized是基于JVM的同步锁，JVM会帮我们自动释放锁。Lock是通过代码实现的，Lock要求我们手工释放，必须在finally语句中释放。
      - Lock锁的范围有局限性、块范围。Synchronized可以锁块、对象、类
      - Lock功能比Synchronized强大，可以通过tryLock方法在非阻塞线程的情况下拿到锁
	  
## volatile关键字
  - 用volatile修饰的变量，线程在每次使用变量的时候，都会读取变量修改后的值，可以简单的理解为volatile修饰的变量保存的是变量的地址。volatile变量具有synchronized的可见性特性，但是不具备原子特性。可见性指的是在确保释放锁之前，对共享数据做出的更改，可以在随后获得该锁的另一个线程中是可见的。要使volatile变量提供理想的线程安全，必须同时满足下面两个条件
      - 对变量的写操作不依赖于当前值
      - 该变量没有包含在具有其他变量的不变式中
  - 比如增量操作（x++）看上去类似一个单独操作，实际上它是一个由读取－修改－写入操作序列组成的组合操作，必须以原子方式执行，而volatile不能提供必须的原子特性。实现正确的操作需要使 x 的值在操作期间保持不变，而 volatile 变量无法实现这点

## 序列化与反序列化
  - 对象的序列化：是把对象转换成字节序列的过程 
  - 对象的反序列化：是把字节序列恢复为对象的过程
  - 对象序列化的主要用途：
      - 可以将字节序列永久的保存在硬盘中，通常放在文件中
      - 可以在网络上传送字节序列
      - 两个线程在进行远程通信时，彼此可以发送各种类型。发送方需要把这个Java对象转换为字节序列，接收方则需要把字节序列再恢复为Java对象

## 自动装箱与拆箱
  Java采用了自动装箱和拆箱机制，节省了常用数值的内存开销和创建对象的开销，提高了效率
  - 装箱：将基本数据类型包装成它们的引用类型
  - 拆箱：将包装类型转换成基本数据类型

## Error和Exception区别
  Error类和Exception类的父类都是throwable类
  - Error：一般指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止
  - Exception：表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常

## Unchecked Exception和Checked Exception区别
  - Unchecked Exception
      - 指的是不可被控制的异常，或称运行时异常，主要体现在程序的瑕疵或逻辑错误，并且在运行时无法恢复
      - 包括Error与RuntimeException及其子类，如：OutOfMemoryError，IllegalArgumentException， NullPointerException，IllegalStateException，IndexOutOfBoundsException等
      - 语法上不需要声明抛出异常也可以编译通过
  - Checked Exception
      - 指的是可被控制的异常，或称非运行时异常
  除了Error和RuntimeException及其子类之外，如：ClassNotFoundException, NamingException, ServletException, SQLException, IOException等
      - 需要try catch处理或throws声明抛出异常

## 编译和运行
  - 编译时和运行时
      - 编译时：将Java文件编译成.class文件的过程，不涉及到内存的分配
      - 运行时：将虚拟机执行.class文件的过程，涉及到内存的分配
  - 编译时类型和运行时类型
      - 编译时类型：由声明该变量时使用的类型决定
      - 运行时类型：由实际赋给该变量的对象决定
  - 编译时和运行时的动态绑定
      - 在编译时，调用的是声明类型的成员方法
      - 在运行时，调用的是实际类型的成员方法
      - 对于调用引用实例的成员变量，无论是编译还是运行时，均是编译时类型的成员变量

## 反射
  - 什么是反射
      - 在运行状态中，对于任意一个类，都可以获得这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性
  - 反射使用步骤
      - 获取类的字节码（getClass()、forName()、类名.class）
      - 根据类的方法名或变量名，获取类的方法或变量
      - 执行类的方法或使用变量，如果不使用，也可以创建该类的实例对象（通过获取构造函数执行newInstance方法）

# 集合框架

## 集合框架简介
  - Collection  
    - List：key可重复
        - LinkedList：(底层双向链表) 有序,非同步,允许null
        - ArrayList：(底层可变数组) 有序,非同步,容量10,允许null,容量1.5倍增长
        - Vector：(底层可变数组) 有序,同步,容量10,允许null,容量2倍增长
    - Set：key不可重复,若key重复,替换旧value
        - HashSet：(基于HashMap) 无序,非同步
        - TreeSet：(排序二叉树)
        - LinkedHashSet：(基于LinkedHashMap) 有序,非同步
  - Map：key唯一,value可重复;若key重复,替换旧value
    - Hashtable：(底层数组+单链表) 无序,同步,容量11, 不允许null
    - HashMap：(底层数组+单链表) 无序,非同步,容量16, 允许null,容量2倍增长
        - LinkedHashMap：(底层数组+双向链表) 有序,非同步,容量16,允许null
    - TreeMap：(排序二叉树) 有序,非同步,不允许null

### 什么是Iterator
  一些集合类提供了内容遍历的功能，通过java.util.Iterator接口。这些接口允许遍历对象的集合。依次操作每个元素对象。当使用 Iterators时，在获得Iterator的时候包含一个集合快照。通常在遍历一个Iterator的时候不建议修改集合本省。

### Iterator与ListIterator有什么区别？
  Iterator：只能正向遍历集合，适用于获取移除元素。ListIerator：继承Iterator，可以双向列表的遍历，同样支持元素的修改。

## HashMap

### HashMap的工作原理？HashMap的get()方法的工作原理？
  HashMap是基于hash算法实现的，通过put(key,value)存储对象到HashMap中，也可以通过get(key)从HashMap中获取对象。当我们使用put的时候，首先HashMap会对key的hashCode()的值进行hash计算，根据hash值得到这个元素在数组中的位置，将元素存储在该位置的链表上。当我们使用get的时候，首先HashMap会对key的hashCode()的值进行hash计算，根据hash值得到这个元素在数组中的位置，将元素从该位置上的链表中取出

### 当两个对象的hashcode相同会发生什么？
  hashcode相同，说明两个对象HashMap数组的同一位置上，接着HashMap会遍历链表中的每个元素，通过key的equals方法来判断是否为同一个key，如果是同一个key，则新的value会覆盖旧的value，并且返回旧的value。如果不是同一个key，则存储在该位置上的链表的链头

### 如果两个键的hashcode相同，你如何获取值对象？
  遍历HashMap链表中的每个元素，并对每个key进行hash计算，最后通过get方法获取其对应的值对象

### 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？
  负载因子默认是0.75，HashMap超过了负载因子定义的容量，也就是说超过了（HashMap的大小*负载因子）这个值，那么HashMap将会创建为原来HashMap大小两倍的数组大小，作为自己新的容量，这个过程叫resize或者rehash

### 你了解重新调整HashMap大小存在什么问题吗？
  当多线程的情况下，可能产生条件竞争。当重新调整HashMap大小的时候，确实存在条件竞争，如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的数组位置的时候，HashMap并不会将元素放在LinkedList的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了

### 我们可以使用自定义的对象作为键吗？
  可以，只要它遵守了equals()和hashCode()方法的定义规则，并且当对象插入到Map中之后将不会再改变了。如果这个自定义对象时不可变的，那么它已经满足了作为键的条件，因为当它创建之后就已经不能改变了。

### 什么时候使用Hashtable，什么时候使用HashMap
  基本的不同点是Hashtable同步HashMap不是的，所以无论什么时候有多个线程访问相同实例的可能时，就应该使用Hashtable，反之使用HashMap。非线程安全的数据结构能带来更好的性能。  
  如果在将来有一种可能—你需要按顺序获得键值对的方案时，HashMap是一个很好的选择，因为有HashMap的一个子类 LinkedHashMap。所以如果你想可预测的按顺序迭代（默认按插入的顺序），你可以很方便用LinkedHashMap替换HashMap。反观要是使用的Hashtable就没那么简单了。同时如果有多个线程访问HashMap，Collections.synchronizedMap（）可以代替，总的来说HashMap更灵活。

### HashMap与HashSet的区别
HashMap	|HashSet
---|---
HashMap实现了Map接口|HashSet实现了Set接口
HashMap储存键值对|HashSet仅仅存储对象
使用put()方法将元素放入map中|使用add()方法将元素放入set中
HashMap中使用键对象来计算hashcode值|HashSet使用成员对象来计算hashcode值
HashMap比较快，因为是使用唯一的键来获取对象|HashSet较HashMap来说比较慢

### Hashtable与HashMap的区别
Hashtable|HashMap
---|---
方法是同步的|方法是非同步的
基于Dictionary类|基于AbstractMap，而AbstractMap基于Map接口的实现
key和value都不允许为null，遇到null，直接返回 NullPointerException|key和value都允许为null，遇到key为null的时候，调用putForNullKey方法进行处理，而对value没有处理
hash数组默认大小是11，扩充方式是old*2+1|hash数组的默认大小是16，而且一定是2的指数

### LinkedHashMap与HashMap的区别
LinkedHashMap|HashMap
---|---
有序的，有插入顺序和访问顺序|无序的
内部维护着一个运行于所有条目的双向链表|内部维护着一个单链表

### TreeMap和HashMap的区别
TreeMap|HashMap
---|---
有序的|无序的
不允许null|允许null
实现Comparable接口|不实现Comparable接口
底层是红黑二叉树，线程不同步|底层是哈希表，线程不同步

## TreeSet和HashSet的区别
TreeSet|HashSet
---|---
有序的|无序的
不允许null|允许null
底层是红黑二叉树，线程不同步|底层是哈希表，线程不同步
HashSet是通过HashMap实现的，用的只是Map的key	|TreeSet是通过TreeMap实现的，用的只是Map的key

## LinkedList与ArrayList的区别
LinkedList|ArrayList
---|---
底层是双向链表|底层是可变数组
不允许随机访问，即查询效率低|允许随机访问，即查询效率高
插入和删除效率快|插入和删除效率低
  - 对于随机访问的两个方法，get和set，ArrayList优于LinkedList，因为LinkedList要移动指针
  - 对于新增和删除两个方法，add和remove，LinedList比较占优势，因为ArrayList要移动数据

## Vector和ArrayList的区别
Vector|ArrayList
---|---
同步、线程安全的|异步、线程不安全
需要额外开销来维持同步锁，性能慢|性能快
可以使用Iterator、foreach、Enumeration输出|只能使用Iterator、foreach输出

# 面向对象

## 面向对象与面向过程
  ### 面向过程 
  - 优点：性能比面向对象高，因为类的调用需要实例化，开销比较大
  - 缺点：没有面向对象的易维护、易复用、易拓展
  ### 面向对象
  - 优点：易维护、易复用、易拓展，由于面向对象有封装、继承、多态的特性，可以设计出低耦合的程序
  - 缺点：性能比面向过程低

## 面向对象的特征
  - 抽象：把现实生活某一类东西提取出来，成为该类东西的共有特性。抽象一般分为数据抽象和过程抽象，数据抽象是对象的属性，过程抽象是对象的行为特征
  - 封装：把客观事物进行封装成抽象类，该类的数据和方法只让可信的类操作，对不可信的类隐藏。封装分为属性的封装和方法的封装
  - 继承：从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力
  - 多态：同一个行为具有多个不同表现形式或形态的能力，多态的前提是类与类之间必须存在关系，要么继承，要么实现
  - StringBuilder行改变的时候，都会复用自身对象，相比StringBuffer能获得10%~15%左右的性能提升，但是得承担多线程的不安全的风险

## 抽象类abstract和接口interface的区别
  - 抽象类和接口分别给出了不同的语法定义（包括方法体、成员变量修饰符、方法修饰符）
    - Java接口中声明的变量默认都是final的。抽象类可以包含非final的变量。 
    - Java接口中的成员函数默认是public的。抽象类的成员函数可以是private，protected或者是public。
  - 抽象是对类的抽象，接口是对行为的抽象
    - 接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法。 
  - 抽象所体现的是继承关系，是一种”is-a”的关系，接口仅仅实现接口定义的契约，是一种”like-a”的关系
     - 类可以实现很多个接口，但是只能继承一个抽象类 
类可以不实现抽象类和接口声明的所有方法，当然，在这种情况下，类也必须得声明成是抽象的。 
  - 抽象是自底向上抽象的，接口是自顶向下设计出来的

## 构造方法
  - 当新对象被创建的时候，构造函数会被调用。
  - 每一个类都有构造函数。在程序员没有给类提供构造函数的情况下，Java编译器会为这个类创建一个默认的构造函数。 
  - Java中构造函数重载和方法重载很相似。可以为一个类创建多个构造函数。每一个构造函数必须有它自己唯一的参数列表。 
  - Java不支持像C++中那样的复制构造函数，这个不同点是因为如果你不自己写构造函数的情况下，Java不会创建默认的复制构造函数。

## 构造方法是否可被override
  - 构造方法不能被重写
  - 构造方法不能被static修饰
  - 构造方法只能用private、protected、public修饰
  - 构造方法不能有返回值

# 线程

## 创建线程有几种不同的方式
  - 继承Thread类
  - 实现Runnable接口 
      - 实现Runnable接口这种方式更受欢迎，因为这不需要继承Thread类。在应用设计中已经继承了别的对象的情况下，这需要多继承（而Java不支持多继承），只能实现接口。同时，线程池也是非常高效的，很容易实现和使用。
  - 应用程序可以使用Executor框架来创建线程池
  - 实现Callable接口

## 如何停止一个线程
  - 创建一个标识（flag），当线程完成你所需要的工作后，可以将标识设置为退出标识
  - 使用Thread的stop()方法，这种方法可以强行停止线程，不过已经过期了，因为其在停止的时候可能会导致数据的紊乱
  - 使用Thread的interrupt()方法和Thread的interrupted()方法，两者配合break退出循环，或者return来停止线程，有点类似标识（flag）
  - （推荐）当我们想要停止线程的时候，可以使用try-catch语句，在try-catch语句中抛出异常，强行停止线程进入catch语句，这种方法可以将错误向上抛，使线程停止事件得以传播

## 线程的状态转换
  -  新建(new)：创建了一个线程对象
  - 可运行(runnable)：线程对象创建后，线程调用start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取cpu的使用权 
  - 运行(running)：可运行状态(runnable)的线程获得了cpu使用权，执行程序代码 
  - 阻塞(block)：线程因为某种原因放弃了cpu使用权，即让出了cpu使用权，暂时停止运行，直到线程进入可运行(runnable)状态，才有机会再次获得cpu使用权转到运行(running)状态。阻塞的情况分三种：
      - 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中
      - 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中
      - 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态
  - 死亡(dead)：线程run()、main()方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期，且死亡的线程不可再次复生

## 什么是线程安全
  - 在多线程访问同一代码的时候，不会出现不确定的结果

## 如何保证线程安全
  - 对非安全的代码进行加锁操作
  - 使用线程安全的类
  - 不要跨线程访问共享变量
  - 使用final类型作为共享变量
  - 对共享变量加锁操作

## 多线程中的死锁
  - 死锁：指两个或两个以上的线程在执行的过程中，因抢夺资源而造成互相等待，导致线程无法进行下去
  - 产生死锁的4个必要条件
      - 循环等待：线程中必须有循环等待
      - 不可剥夺：线程已获得资源，再未使用完成之前，不可被剥夺抢占
      - 资源独占：线程在某一时间内独占资源
      - 申请保持：线程因申请资源而阻塞，对已获得的资源保持不放

## 如何确保N个线程可以访问N个资源同时又不导致死锁？
  - 使用多线程的时候，一种非常简单的避免死锁的方式就是：指定获取锁的顺序，并强制线程按照指定的顺序获取锁。因此，如果所有的线程都是以同样的顺序加锁和释放锁，就不会出现死锁了。

## 什么叫守护线程，用什么方法实现守护线程
  - 守护线程：指为其他线程的运行提供服务的线程，可通过setDaemon(boolean on)方法设置线程的Daemon模式，true为守护模式，false为用户模式

## 多线程的等待唤醒主要方法
  - void notify()：唤醒在此对象监视器上等待的单个线程
  - void notifyAll()：唤醒在此对象监视器上等待的所有线程
  - void wait()：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法
  - void wait(long timeout)：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者超过指定的时间量
  - void wait(long timeout, int nanos)：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量

## sleep和wait的区别
sleep()|wait()
---|---
是Thread类的方法|是Object类中的方法
调用sleep()，在指定的时间里，暂停程序的执行，让出CPU给其他线程，当超过时间的限制后，又重新恢复到运行状态，在这个过程中，线程不会释放对象锁|	调用wait()时，线程会释放对象锁，进入此对象的等待锁池中，只有此对象调用notify()时，线程进入运行状态