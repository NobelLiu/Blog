---
title: WWDC 2015 学习笔记 － Advance NSOperations
date: 2016-05-30 20:34:28
category: Study
tags:
- Objective-C
- WWDC
- NSOperationQueue
---

说来惭愧，眼看着 WWDC 2016 就要开了，我才开始看 15 年份的...
## NSOperationQueue
### 与 GCD 的差别    
`GCD`（[Grand Central Dispatch](https://en.wikipedia.org/wiki/Grand_Central_Dispatch)）是底层的C语言构成的 API，而`NSOperation`看名字包含`NS`就知道是 Objective-C 的对象，是一个一个面向对象的解决办法，拥有高等级的 API。  
	Q：为什么要用高等级的 API？  
	A：因为可以用更简单的方式去完成更复杂的事情。  
在`GCD`中，在队列中执行的是由 block 构成的任务，这是一个轻量级的数据结构；而`NSOperation`作为一个对象，为我们提供了更多的选择。
<!-- more --> 
### 为什么选 NSOperationQueue
* 更易用的高等级的 API
* 可以取消执行
* 可以设置`maxConcurrentOperationCount`  

什么是`maxConcurrentOperationCount`呢

### maxConcurrentOperationCount
`maxConcurrentOperationCount`是指能并行运行的操作的最大值。设置为**默认或者大于1**的值就可以并行执行操作。

## NSOperation
`NSOperation`是更高等级对`dispatch block`的封装。它的执行也要比`block`慢一些。原话是：
{% cq %} 
NSOperations run for a little bit longer than you would expect a block to run, so blocks usually take a few nanoseconds, maybe at most a millisecond, to execute. NSOperations, on the other hand, can be much longer, for anywhere from a couple of milliseconds to even several minutes, as we will talk about later.
{% endcq %}  
`NSOperation`也可以方便的设置依赖关系，甚至可以是不同队列的 Operation，这个后面会说用途。  
### 子类化
试着将你的任务切割为一个个 operation 吧，代码将很容易复用，也可以很容易的在在不同 operation 中建立关系。
### 生命周期
`NSOperation`的生命周期分为四个状态：`Pending`、`Ready`、`Executing`、`Finished`。  
其中除`Finished`以外的状态均可以被取消，状态变为`Cancelled`。  
![NSOperation 的生命周期](https://ww2.sinaimg.cn/large/006tKfTcgy1fcozuiivcoj30zd0cqwf9.jpg) 
`Cancelled`状态：  
```Swift
var cancelled: Bool { get }
```  
子类决定意义？
将一个 operation cancel 只需调用其方法：
```Swift
operationA.cancel()
```   
`Ready`状态：  
```Swift
var ready: Bool { get }
```	  
`Ready`状态代表 operation 已经准备好了，可以被执行。在队列中，先执行的 operation 为先将状态改变为`Ready`的 operation，并不是插入的顺序。
### 依赖（Dependency）
依赖简单来说就是“做完这个，在做那个”，并且会被严格执行。  
在确认依赖的 operation 结束后，才将自身的状态更改为`Ready`。  
依赖没有相同 queue 的限制。    
```Swift
let operationA = ...  
let operationB = ...  
operationB.addDependency(operationA)
```	
需要注意的是 operationB 依赖设置为 operationA 后，不要将 operationA 的依赖再设置为 operationB，会造成 operation 死锁（Operation deadlock）。  
![Operation deadlock](https://ww2.sinaimg.cn/large/006tKfTcgy1fcp06fl6ngj30zf0f3t92.jpg)