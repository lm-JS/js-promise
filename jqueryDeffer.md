# jquery 中的deffered对象

## 一、什么是deferred对象？
> * 开发网站的过程中，我们经常遇到某些耗时很长的javascript操作。其中，既有异步的操作（比如ajax读取服务器数据），也有同步的操作（比如遍历一个大型数组），它们都不是立即能得到结果的。通常的做法是，为它们指定回调函数（callback）。即事先规定，一旦它们运行结束，应该调用哪些函数。
* 但是，在回调函数方面，jQuery的功能非常弱。为了改变这一点，jQuery开发团队就设计了deferred对象。
* 简单说，deferred对象就是jQuery的回调函数解决方案。在英语中，defer的意思是"延迟"，所以deferred对象的含义就是"延迟"到未来某个点再执行。
它解决了如何处理耗时操作的问题，对那些操作提供了更好的控制，以及统一的编程接口。它的主要功能，可以归结为四点。

## 实例：

   //ajax操作的链式写法
    $.ajax("test.html")
    .done(function(){ alert("哈哈，成功了！"); })
    .fail(function(){ alert("出错啦！"); });
    
    //指定同一操作的多个回调函数
    $.ajax("test.html")
    .done(function(){ alert("哈哈，成功了！");} )
    .fail(function(){ alert("出错啦！"); } )
    .done(function(){ alert("第二个回调函数！");} );
    
    //为多个操作指定回调函数
    $.when($.ajax("test1.html"), $.ajax("test2.html"))
    .done(function(){ alert("哈哈，成功了！"); })
    .fail(function(){ alert("出错啦！"); });
  
## deferred.resolve()方法和deferred.reject()方法

> jQuery规定，deferred对象有三种执行状态----未完成，已完成和已失败。如果执行状态是"已完成"（resolved）,deferred对象立刻调用done()方法指定的回调函数；如果执行状态是"已失败"，调用fail()方法指定的回调函数；如果执行状态是"未完成"，则继续等待，或者调用progress()方法指定的回调函数;例如：

    var dtd = $.Deferred(); // 新建一个deferred对象
  　　    var wait = function(dtd){
  　　　   　var tasks = function(){
  　　　　　　alert("执行完毕！");
  　　　　　　dtd.resolve(); // 改变deferred对象的执行状态
  　　　 //　　dtd.reject(); // 改变Deferred对象的执行状态
  　　　　};
  　　　　setTimeout(tasks,5000);
  　　　　return dtd;
  　};
  　$.when(wait(dtd))//$.when()的参数只能是deferred对象；
  　.done(function(){ alert("哈哈，成功了！"); })
  　
    
    
