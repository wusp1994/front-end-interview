# 异步操作概述

## 1.单线程模型

> 单线程模型指的是，javascript只在一个线程上运行。也就是说，JavaScript同时只能执行一个任务，其他任务都必须在后面排队等待。

:warning::warning::warning:

注意, javascrip只在一个线程上运行，不代表JavaScript引擎只有一个线程。事实上，JavaScript引擎有多个线程，单个脚本只能在一个线程上运行（称为主线程），其他线程都是在后台配合。

:warning::warning::warning:

JavaScript 之所以采用单线程，而不是多线程，跟历时有关系。JavaScript从诞生起就是单线程，原因是不想让浏览器变得太复杂，因为多线程需要共享资源，且有可能修改彼此的运行结果，对于网页脚本语言来说太复杂了。如果JavaScript同时拥有两个线程，一个线程在网页DOM节点上添加内容，另一个线程删除这个节点，这是浏览器应该以哪个线程为准？是否还要有锁机制？所以为了避免复杂性，JavaScript一开始就是单线程，这已经成为这门语言的核心特征，将来也不会改变。

单线程的模式好处就是：比较简单，执行环境相对单纯；

坏处就是:只要有一个任务耗时很长，后面的任务都必须排队等待，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段JavaScript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。JavaScript语言本身并不慢，慢的是读写外部署数据，比如等待ajax请求返回结果。这个时候，如果对方服务器迟迟没有响应，或者网络不通畅，就会导致脚本的长时间停滞。

如果排队是因为计算量大，CPU忙不过来，也就算了，但是很多还是 CPU是闲着的，因为IO操作（输入输出）很慢（比如ajax操作从网络中读取数据），不得不等着结果出来，再往下执行。JavaScript语言设计者意识到，这时的CPU完全可以不管IO操作，挂起处于等待中的任务，先运行排在后面的任务。等到IO操作返回了结果，再回过头，把挂起的任务继续执行下去。这种机制就是JavaScript内部采用的“事件循环”机制`（Event Loop）`。

单线程模型虽然对JavaScript构成了很大的限制，但也因此使它具备了其他语言不具备的优势。如果用得好，JavaScript程序是不会出现阻塞的，这就是为什么Node可以用很少的资源，应付大流量访问的原因。

为了利用多核CPU计算能力，HTML5提出web Worker 标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以这个标准并没有改变 JavaScript单线程的本质。

## 2，同步任务和异步任务

程序里面所有的任务，可以分为两类：同步任务（synchronous）和异步任务（asynchronous）。

同步任务是在`主线程上排队`执行的，特点：前一个任务执行完，后一个任务才能执行。

异步任务是被引擎放在一边，`不进入主线程`，而是进入任务队列的任务。只有引擎任务某个异步任务可以执行了（比如ajax操作从服务器得到了结果），该任务（采用异步回调函数的形式）才会进入主线程执行。排在异步任务后面的代码，不用等待异步任务结束会马上执行 ，也就是说异步不具有阻塞效应。

## 3，任务队列和事件循环

JavaScript运行时，除了一个正在运行的主线程，引擎还提供一个任务队列（task queue）,里面是各种需要当前程序处理的异步任务。（实际上，根据异步任务的类型，存在多个任务队列，为了方便理解，这里假设只存在一个队列）




## callback，promise，generator，async-await

首先我们回顾一下 `javascript异步` 的发展历程。

ES6以前：

​			回调函数（callback ）: nodejs express 中常用，ajax 中常用。

ES6:

​			promise 对象：nodejs 最早有 bluebird,（promise的雏形），axios中常用。

​			generator函数：nodejs koa框架使用率很高。

ES7:

​			async/await语法：针对generator的封装，当前最常用的异步语法，nodejs koa2 完全使用该语法。

## async-await

