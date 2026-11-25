---
title: "JavaScript 单线程执行顺序"
date: 2018-07-17
categories: [技术]
tags: [JavaScript, 前端]
draft: false
---

JavaScript是一种单线程的语言。同一个时间只能做一件事情，JavaScript的一些类似多线程操作通过回调实现，类似于单片机的中的中断。回调的操作包括有：定时器（setTimeout、setInterval）、onclik、onmouseover、onload等。

JavaScript的所有任务可以分为同步任务和异步任务。执行机制如下：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。

JavaScript会先执行执行栈里面的任务，当执行栈任务空掉后，会把任务队列中的任务按照先后顺序放到执行栈中执行。回调函数是放入任务队列中的。

下面的代码，当while执行时候，setTimeout永远不会执行。
```
var isEnd = true;
window.setTimeout(function () {
    isEnd = false;//1s后，改变isEnd的值
}, 1000);     //这个while永远的占用了js线程，所以setTimeout里面的函数永远不会执行
            while (isEnd);
            //alert也永远不会弹出     alert('end');
```
因为上述死循环一直在执行栈中执行，任务队列中的回调函数无法被执行（执行栈没有空），所以alert一直不会弹出。

JavaScript在预处理时会通过JavaScript hoisting变量提升：
javascript会先执行var和function 但是function的声明是高于var。
说人话就是：先预先var的变量声明，再执行function的函数声明和定义，再对变量进行赋值，再顺序执行。

写代码过程中的一点小结，暂时不保证正确性。

参考：

[http://www.ruanyifeng.com/blog/2014/10/event-loop.html](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
JavaScript 运行机制详解：再谈Event Loop - 阮一峰的网络日志

[https://www.cnblogs.com/Mainz/p/3552717.html](https://www.cnblogs.com/Mainz/p/3552717.html)
Javascript是单线程的深入分析 - Mainz - 博客园

[https://www.jb51.net/article/36755.htm](https://www.jb51.net/article/36755.htm)
js 程序执行与顺序实现详解_javascript技巧_脚本之家

[https://www.zhihu.com/question/22949631](https://www.zhihu.com/question/22949631)
JavaScript函数解析的顺序是什么？ - 知乎
