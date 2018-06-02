---
title: JavaScript 运行机制之Promise、setTimeout
date: 2018-2-10 21:14:46
categories: Front End
tags:
- Promise
description:
---
```
new Promise(resolve => {
  resolve(1);
  Promise.resolve().then(() => console.log(2))
}).then(t => console.log(t))
console.log(3)
```
这是 [Jake Archibald](https://twitter.com/jaffathecake) 在 twitter 上发布的一条推文，之后发了个投票 1000 多人中有 27% 的人选择了正确的输出结果为

```
3
2
1
```
# why ?

首先我们来看一个 JavaScript 的运行机制的问题

## JavaScript 运行机制

JavaScript 最大的特点就是单线程，单线程就意味着任务执行需要排队。但是不能一直等着上个任务，所以出现了异步，同步任务。

### 任务队列

JavaScript 的任务队列分为两种：
- 宏任务队列(macro tasks)：script（全局任务）, setTimeout, setInterval, setImmediate, I/O, UI rendering.
- 微任务队列(micro tasks)：process.nextTick, Promise, Object.observer, MutationObserver.

当stack空的时候，就会从任务队列中，取任务来执行。共分3步：

- 取一个宏任务来执行。执行完毕后，下一步。
- 取一个微任务来执行，执行完毕后，再取一个微任务来执行。直到微任务队列为空，执行下一步。
- 更新UI渲染。

Event Loop 会无限循环执行上面3步，这就是Event Loop的主要控制逻辑。其中，第3步（更新UI渲染）会根据浏览器的逻辑，决定要不要马上执行更新。毕竟更新UI成本大，所以，一般都会比较长的时间间隔，执行一次更新。

这就是 JavaScript 的运行机制

> 栈先入后出，队列先入先出

### 案例

现在我们来分析文章最初那段代码执行顺序：

```
new Promise(resolve => {
  resolve(1);
  Promise.resolve().then(() => console.log(2))
}).then(t => console.log(t))
console.log(3)
```
首先全局任务 script 进栈 ，接着 promise executor 进栈，然后发现了微任务 (inner promise callback) 进入微任务队列

```
- JS stack: promise executor || Script
- micro tasks: inner promise callback
- log:
```

执行完 promise executor 出栈，由于执行 resolve() ，执行成功回调，发现了微任务 (outer promise callback) 放入微任务队列， console.log(3) 进栈并执行输出

```
- JS stack: Script
- micro tasks: inner promise callback || outer promise callback
- log: 3
```

清空 JS 栈 ，微任务(inner promise callback)进栈，执行 console.log(2)

```
- JS stack: promise callback
- micro tasks:inner promise callback || outer promise callback
- log: 3 2
```

清空 JS 栈 ， 微任务(outer promise callback)进栈
，执行 console.log(1)

```
- JS stack: promise callback
- micro tasks: outer promise callback
- log: 3 2 1
```

如果我们想要输出 3 1 2 怎么办呢？

答案也很简单，就是需要控制微任务队列的顺序即可

```
new Promise(resolve => {
  Promise.resolve().then(() => {
    resolve(1);
    Promise.resolve().then(() => console.log(2))
  })
}).then(t => console.log(t))
console.log(3)
```
```
3
1
2
```
首先全局任务 script 进栈 ，接着 promise executor 进栈，然后发现了微任务 (executor promise callback) 进入微任务队列

```
- JS stack: promise executor || Script
- micro tasks: executor promise callback
- log:
```
executor promise callback 出栈 ，console.log(3) 进栈执行并输出

```
- JS stack: Script
- micro tasks: executor promise callback
- log: 3
```
清空 JS 栈 ，微任务(executor promise callback)进栈

```
- JS stack: promise callback
- micro tasks: executor promise callback || outer promise callback || inner promisr callback
- log: 3
```

清空 JS 栈 ， 微任务(outer promise callback)进栈，console.log(1) 进栈执行并输出

```
- JS stack: promise callback
- micro tasks: outer promise callback || inner promisr callback
- log: 3 1
```

清空 JS 栈 ， 微任务(inner promise callback)进栈

```
- JS stack: promise callback
- micro tasks: inner promise callback
- log: 3 1 2
```

### setTimeout

当然我们也可以用 setTimeout 来实现输出 3 1 2

```
new Promise(resolve => {
  setTimeout(function() {
    resolve(1);
    Promise.resolve().then(() => console.log(2));
  }, 0)
}).then((t) => console.log(t));
console.log(3);
```
```
3
1
2
```

but ...

```
new Promise(resolve => {
  setTimeout(function() {
    resolve(1);
  }, 0)
  Promise.resolve().then(() => console.log(2));
}).then((t) => console.log(t));
console.log(3);
```
```
3
2
1
```


### Link
- [JavaScript 异步、栈、事件循环、任务队列](https://segmentfault.com/a/1190000011198232)
- [Jake Archibald: In The Loop - JSConf.Asia 2018(YouTube-Video)](https://www.youtube.com/watch?v=cCOL7MC4Pl0&list=PLjXJXJuLhBIvX7RphQDmDmyAGJJd6UvAo)
