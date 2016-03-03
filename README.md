# js-promise

###what
> Promise是CommonJS的规范之一，拥有resolve、reject、done、fail、then等方法，能够帮助我们控制代码的流程，避免函数的多层嵌套[实例参考][1]。jQuery等流行的js库都已经实现了这个对象，年底即将发布的ES6也将原生实现Promise

###同步函数最重要的两个特征
> 1. 能够返回值
2. 能够抛出异常
#Promise 模式解决了异步函数中实现同步的特征，它能够实现函数的返回与异常的抛出（冒泡直到被捕获）。
  
  *符合 Promise 模式的函数必须返回一个 promise。*

###How
>目前的ES6标准之前还未支持Promise对象，那么我们就自己动手，丰衣足食吧。思路大致是这样的，用2个数组(doneList和failList)分别存储成功时的回调函数队列和失败时的回调队列[参考][1]
* state: 当前执行状态，有pending、resolved、rejected3种取值
* done: 向doneList中添加一个成功回调函数
* fail: 向failList中添加一个失败回调函数
* then: 分别向doneList和failList中添加回调函数
* always: 添加一个无论成功还是失败都会调用的回调函数
* resolve: 将状态更改为resolved,并触发绑定的所有成功的回调函数
* reject: 将状态更改为rejected,并触发绑定的所有失败的回调函数
* when: 参数是多个异步或者延迟函数，返回值是一个Promise兑现，当所有函数都执行成功的时候执行该对象的resolve方法，反之执行



[1]: https://segmentfault.com/a/1190000000684654#articleHeader1
