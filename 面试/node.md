##### 什么是nodejs

nodejs是服务器端的一门技术，基于google v8 javascript引擎开发，用来开发可扩展的服务端程序。

##### nodejs优点

执行快速。

永远不会阻滞。

javascript是通用的编程语言。

异步处理机制。

避免并行所带来的问题。

##### nodejs的特点，与其他服务器端对比

特征：单线程、事件驱动、非阻塞I/O

对比：

无法直接渲染静态页面，提供静态服务

没有根目录的概念

必须通过路由程序指定文件才能渲染文件

比其他服务端性能更好，速度更快

###### set immediate马上执行，set time out时间参数为0时没有前者快

##### 更新版本

```
npm install npm -g
```

##### 单线程

nodejs通过异步处理的方式，可以处理大量的数据吞吐量，从而有更好的性能和可扩展性。

##### 回调函数

指用一个函数作为参数传入另外一个函数，这个函数会被在某个时机执行

##### 回调地狱

由嵌套的回调函数导致的。这样的机制会导致某些函数无法到达并且很难维护

##### 如何阻止回调地狱

对每个错误都要处理到

保证代码的贯通

程序代码模块化

##### repl

Read evaluate print loop,用于测试，调试和实验用

##### API函数

阻滞型函数：等待操作完成以后再进行下一步

非阻滞型函数：使用回调函数来处理当前函数获取的结果

##### 回调函数的第一个参数是什么

通常是错误对象，如果为空表示没有错误

##### NPM的作用

node package manager，主要两个功能

* 网端模块的存储介质

* 安装程序依赖和版本管理

##### node和ajax的区别

ajax是设计用来动态更新页面的某个区域，不需要刷新整个页面

node是用来开发客户服务器类型应用的。

##### streams（流）

流的概念是不间断，不间断的从某个地方读取数据或者向某个地方不断的写入数据

4个类型：

* 可读
* 可写
* 即可读又可写
* 转化

##### 如何判断当前脚本运行在浏览器还是node环境

通过判断Global对象是否为window

##### Global

global代表的是最上层的命名空间，用来管理所有其他的全局对象

process是一个全局对象，可以把异步函数转化为异步回调，它可以在任何地方被访问，它主要用来返回系统的应用信息和环境信息

Buffer，用来处理二进制数据的类

##### 同步异步的区别，如何避免回调地狱

同步回调是必须等方法调用返回才能进行下一步

异步回调方法调用会立即返回，调用者可以继续后续操作。异步方法在另一个线程不会阻碍调用者工作。

###### 避免回调地狱：

Promise

Async/await

gennerator

事件发布/监听模式