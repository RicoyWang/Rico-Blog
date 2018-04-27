>用更简洁更形象的语言去描述陌生的知识，一直是我坚持在做，也乐于其中的事。高中喜欢把生物知识的各种循环体系想象成一台机器运作，现在又喜欢把代码想象成活生生的人，就是'作'。```「bind,aplly,call」```就像是代码里的侠盗，把一个对象的方法盗取借给其他对象使用，使其获取想要的结果。那么接下来，就不掀这位侠盗的小裤裤了😂


##✎前言
我们知道，在JavaScript 中一切皆对象，而函数其实是一种对象. 而作为对象, 函数是可以有方法的, 包括非常强大的 ```「bind,aplly,call」方法```.而```「bind,aplly,call」方法```**其实是函数的另外一种调用方式，功能上等同于```fn()```**，只是使用者由 ```「bind,aplly,call」```决定，也就是 ```「bind,aplly,call」```指定函数```执行上下文```。

##✎函数的另一种调用方式
我们在声明一个函数后，基本上调用的方式都是：
```
//方法定义
var obj = {
    todo : function(){
        console.log("嘟嘟嘟")
        }
    }
//调用方式
obj.todo();//嘟嘟嘟
//函数声明
function todo() {
   console.log("嘟嘟嘟")
}
//调用方式
todo();//嘟嘟嘟
//函数表达式
var todo = function() {
   console.log("嘟嘟嘟")
}
//调用方式
todo();//嘟嘟嘟
/**还有匿名函数，箭头函数，和函数构造器，比较特殊，**/
```
从上面可以看出，无论是方法定义、函数声明、函数表达式这几个常用的创建方式，调用函数的方式基本上都是是一种函数名加`()`进行调用。前言中我也说到```「bind,aplly,call」```是函数的另一种调用方式，那么接下我们使用`「bind,aplly,call」`对上面的代码片段进行改写。
```
//方法定义
var obj = {
    todo : function(){
        console.log("嘟嘟嘟")
        }
    }
//调用方式
obj.todo.apply(); //嘟嘟嘟
obj.todo.call();  //嘟嘟嘟
obj.todo.bind()();//嘟嘟嘟
//函数声明
function todo() {
   console.log("嘟嘟嘟")
}
//调用方式
todo.apply(); //嘟嘟嘟
todo.call();  //嘟嘟嘟
todo.bind()();//嘟嘟嘟
//函数表达式
var todo = function() {
   console.log("嘟嘟嘟")
}
//调用方式
todo.apply(); //嘟嘟嘟
todo.call();  //嘟嘟嘟
todo.bind()();//嘟嘟嘟
/**bind和apply,call有些不一样，在后面单独讲解他们的区别**/
```
##✎ 可指定的窃取受益者 -- 允许我们明确指定方法中的 this 指向的对象
在函数调用过程中，这个函数的执行上下文为`otherObj`，及函数执行过程中的this指向`otherObj`
```
fn.apply(otherObj)
fn.call(otherObj)
fn.bind(otherObj)()
```
##✎ 盗亦有道 -- 函数调用内部需要`this`关键字才能体现效果
我们的侠盗也是很有讲究的，`this`关键字就是他眼中的道，当然不是因为他真的是一位有原则的侠士，和其他偷盗者一样，没有财富的人家盗无可盗，而`this`在他眼中就是财富，可以让其他人使用的财富。
```
##带有财富标签 this的偷盗
var dudu = "嘟嘟嘟嘟嘟嘟"
var obj = {
    dudu :"嘟嘟嘟",
    fn:function(){
        console.log(this.dudu)
    }
}
obj.fn()//嘟嘟嘟
//this指向window 获得想要的window上的属性
obj.fn.apply(window)//嘟嘟嘟嘟嘟嘟
obj.fn.call(window)//嘟嘟嘟嘟嘟嘟
obj.fn.bind(window)()//嘟嘟嘟嘟嘟嘟
```
```
##没有带财富标签的偷盗
var dudu = "嘟嘟嘟嘟嘟嘟"
var obj = {
    dudu :"嘟嘟嘟",
    fn:function(){
        console.log(obj.dudu) // this 改为 obj 
    }
}
obj.fn()//嘟嘟嘟
//this指向window 却没有获得window上的属性
obj.fn.apply(window)//嘟嘟嘟
obj.fn.call(window)//嘟嘟嘟
obj.fn.bind(window)()//嘟嘟嘟
```
两段代码的区别就是调用函数中是否使用了`this`，其实两段代码中，偷盗的行为已经发生了，也就是函数本身的`this`已经发生了改变，第二段代码片段由于调用函数中没有使用`this`关键字，在结果上没有发生变化。
**bind,aplly,call允许我们明确指定方法中的 this 指向**，使用`bind,aplly,call`调用函数其实就已经根据你的需要改变了`this`指向，而为了让这种特殊调用产生移花接木的效果，函数内的一些关键位置需要使用`this`来引用方法或者属性。

##✎ 可指定的窃取物品 -- 传参可明确
`bind,aplly,call`可以接收两类参数，第一个是受益者(修改this指向)，第二个为函数调用传入的参数`arguments`，在函数执行过程中，作为默认参数被使用。
##✎ `bind,aplly,call`的区别
  - `apply` 和 `call` 的用法几乎相同, 唯一的差别在于当函数需要传递多个变量时, `apply` 可以接受一个数组作为参数输入, `call` 则是接受一系列的单独变量。
  - 在上面的代码片段中，我们可以看出bind使用方法和`apply`， `call` 不一样，多了一个执行`()`。那是因为`bind`在函数调用后，实际上会返回一个新函数。
  - `bind,aplly,call`中只有`apply`接收一个数组，可以通过`apply`和`array`两个单词尾部都是`y`记忆。`bind,call`则都是接收一系列单独变量。


##✎总结
  - `bind,aplly,call`其实就相当于`fn()`，一种新的函数调用方式，只是他们能够改变函数执行的上下文 `this`，也能改变传参`arguments`。
  - 可以使用 bind() 方法来实现函数的柯里化，感兴趣的可以看一下文末的参考文章
  - `Array.prototype.slice.apply.(obj)`，可以让一个类数组对象使用数组的方法，类数组主要是指通过DOM 操作获取的DomList 和`arguments`，都是具有`length`属性的对象。
  - `Object.prototype.toString.call(obj).slice(8, -1)` 方法获取对象属性，比`typeof()`更可信。
  - 在 ES6 的箭头函数下, call 和 apply 的失效
  - 不可以当作构造函数, 也就是说不可以使用 new 命令, 否则会抛出一个错误;




[[译] JavaScript 中至关重要的 Apply, Call 和 Bind](https://hijiangtao.github.io/2017/05/07/Full-Usage-of-Apply-Call-and-Bind-in-JavaScript/)



