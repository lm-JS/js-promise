# js-promise

### Promise/A+规范
> Promise表示一个异步操作的最终结果。主要用于延迟(deferred) 计算和异步(asynchronous ) 计算.。一个Promise对象代表着一个还未完成，但预期将来会完成的操作。（[es6定义][2]）与Promise最主要的交互方法是通过将函数传入它的then方法从而获取得Promise最终的值或Promise最终最拒绝（reject）的原因。



### 术语
> * promise是一个包含了兼容promise规范then方法的对象或函数，
* thenable 是一个包含了then方法的对象或函数。
* value 是任何Javascript值。 (包括 undefined, thenable, promise等).
* exception 是由throw表达式抛出来的值。
* reason 是一个用于描述Promise被拒绝原因的值。

###  Promise状态
> 一个Promise必须处在其中之一的状态：pending（初始状态）, fulfilled（成功的操作） 或 rejected（失败的操作）.  
  *1. 如果是pending状态,则promise：*  
　　　　可以转换到fulfilled或rejected状态。  
  *2. 如果是fulfilled状态,则promise：*  
    　　　不能转换成任何其它状态。  
    　　　必须有一个值，且这个值不能被改变。  
  *3. 如果是rejected状态,则promise可以：*  
    　　　不能转换成任何其它状态。  
    　　　必须有一个原因，且这个值不能被改变。  
”值不能被改变”指的是其identity（引用）不能被改变，而不是指其成员内容不能被改变。

### then方法
　　一个Promise必须提供一个then方法来获取其值或原因。
> promise.then(onFulfilled, onRejected)　　
　　1. 它必须在promise fulfilled或rejected后调用， 且promise的value或reason为其第一个参数。
　　2. 它不能在promise fulfilled或rejected前调用。
　　3. 不能被多次调用。

### 作用
> 1. 能够帮助我们控制代码的流程，避免函数的多层嵌套[实例参考][1]
2. 解决了异步函数中实现同步的特征，它能够实现函数的返回与异常的抛出（冒泡直到被捕获）。

### 应用场景


  
### 其他实现promise框架
> Query等流行的js库都已经实现了这个对象，ES6原生实现Promise



 >Promise 模式  
   *符合 Promise 模式的函数必须返回一个 promise。*  
   参考：https://segmentfault.com/a/1190000000767545

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
[2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise
