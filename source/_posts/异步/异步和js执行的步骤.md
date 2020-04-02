---
title: 异步和js执行的步骤
top: false
cover: false
toc: true
mathjax: true
date: 2020-03-26 19:00:42
password:
summary:
tags:
categories:
- 异步
---

**前端使用异步的场景**
1、定时任务：setTimeout，setlnverval
2、网络请求：ajax请求，动态加载
3、事件绑定

**javascrip执行顺序**

1、代码自上而下执行
2、逐行执行代码
3、有代码异步操作
4、异步代码插入异步队列(异步队列分：宏任务和微任务)
5、执行完成（查看异步队列是否为空 不为空执行异步队列）

**异步队列的执行顺序**
1、在执行下一次宏任务之前会先执行完所有微任务 
2、只要有微任务 宏任务就会被执行
3、同步 => 微任务 => 宏任务

**微任务** promise , process nextTick
**宏任务** 整体代码 script settimeout  setInterval

````js
console.Log(100) 
setTimeout(function(){ 
    console.Log(200) 
    }) 
    console.1og(300)
    // 异步和单线程·执行第一行，打印100
// 执行setTimeout后，传入setTimeout的函数会被暂存起来，不会立即执行（单线程的特点，不能同时干两件事）
// 执行最后一行，打印300
// 待所有程序执行完，处于空闲状态时，会立马看有没有暂存起来的要执行
// 发现暂存起来的setTimeout中的函数无需等待时间，就立即来过来执行
````

**同步或异步的区别是什么**
1、同步会阻塞代码执行，而异步不会
2、alert是同步，setTimeout是异步

**异步和单线程的关系**
js 是单线程所以 在同一时间内只能干一件事
异步 可以解决js 这种弊端