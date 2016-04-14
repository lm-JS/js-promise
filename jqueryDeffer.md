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
          //dtd.reject(); // 改变Deferred对象的执行状态
  　　　};
  　　　setTimeout(tasks,5000);
  　　　return dtd;
    };
    $.when(wait(dtd))//$.when()的参数只能是deferred对象；
    .done(function(){ alert("哈哈，成功了！"); })
   
## deferred.promise()

> * 上面这种写法，还是有问题。那就是dtd是一个全局对象，所以它的执行状态可以从外部改变。
* 为了避免这种情况，jQuery提供了deferred.promise()方法。它的作用是，在原来的deferred对象上返回另一个deferred对象，后者只开放与改变执行状态无关的方法（比如done()方法和fail()方法），屏蔽与改变执行状态有关的方法（比如resolve()方法和reject()方法），从而使得执行状态不能被改变。

    var dtd = $.Deferred(); // 新建一个Deferred对象
    var wait = function(dtd){
   　　　var tasks = function(){
   　　　　　alert("执行完毕！");
   　　　　　dtd.resolve(); // 改变Deferred对象的执行状态
   　　　};
   　　　setTimeout(tasks,5000);
   　　　return dtd.promise(); // 返回promise对象
   　};
    var d = wait(dtd); // 新建一个d对象，改为对这个对象进行操作
   　$.when(d)
   　.done(function(){ alert("哈哈，成功了！"); })
   　.fail(function(){ alert("出错啦！"); });
   　d.resolve(); // 此时，这个语句是无效的
   
    wait()函数返回的是promise对象。然后，我们把回调函数绑定在这个对象上面，而不是原来的deferred对象上面。
    这样的好处是，无法改变这个对象的执行状态，要想改变执行状态，只能操作原来的deferred对象。 
    //在wait函数内部，新建一个Deferred对象,可以防止全局对象
   
## $.Deferred()

> 另一种防止执行状态被外部改变的方法，是使用deferred对象的构建函数$.Deferred()。
jQuery规定，$.Deferred()可以接受一个函数名（注意，是函数名）作为参数，$.Deferred()所生成的deferred对象将作为这个函数的默认参数。

    var wait = function(dtd){
      var tasks = function(){
         alert("执行完毕！");
         dtd.resolve(); // 改变Deferred对象的执行状态
      };
      setTimeout(tasks,5000);
      return dtd.promise();
    };
    $.Deferred(wait)
    .done(function(){ alert("哈哈，成功了！"); })
    .fail(function(){ alert("出错啦！"); });
    
    
    直接在wait对象上部署deferred接口。
    var dtd = $.Deferred(); // 生成Deferred对象
    var wait = function(dtd){
      var tasks = function(){
         alert("执行完毕！");
         dtd.resolve(); // 改变Deferred对象的执行状态
      };
      setTimeout(tasks,5000);
    };
    dtd.promise(wait);
    wait.done(function(){ alert("哈哈，成功了！"); })
    .fail(function(){ alert("出错啦！"); });
    wait(dtd);

## deferred.then()

> 有时为了省事，可以把done()和fail()合在一起写，这就是then()方法

    $.when($.ajax( "/main.php" )).then(successFunc, failureFunc );
   
## deferred.always()

> 不管调用的是deferred.resolve()还是deferred.reject()，最后总是执行。

    $.ajax( "test.html" ).always( function() { alert("已执行！");} );
