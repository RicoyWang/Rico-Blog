
>AOP在职责模式中也有使用，初学设计模式过程中，AOP是性价比最高的，在实际开发中的作用也很重要，学习AOP你可以在不改动原来大量代码功能模块的前提下对功能进行改造，添加插件，添加功能。比如提交ajax请求提交前，添加验证功能，或者修改params。同时也不会修改原有代码。
###如何理解AOP（切面编程）
AOP中文名叫面向切面编程。打一个通俗易懂的比喻，现在我们手上有一摞扑克牌，发牌的过程可以看做是JS的执行过程，一张扑克牌就是一个功能，可能是一个简单的console.log，也可能是复杂的http请求，这张卡牌是♥️7，如果我想在发牌过程中连续拿到♥️6、♥️7、♥️8。再不打乱整副牌的前提下，只需要在♥️7前加入♥️6，♥️7后面插入♥️8。当然你可以重写♥️7，直接在牌面上加上新的程序，不过改变了原有的程序代码，并不优雅；
```
原来的♥️7
 var ♥️7 = function(){
    console.log(♥️7);
}
作弊：改写♥️7
var ♥️7= function(){
    console.log(♥️6);
    console.log(♥️7);
    console.log(♥️8);
}
理想的：添加新的卡牌(函数)
var ♥️6 = function(){
    console.log(♥️6);
}
var ♥️8 = function(){
    console.log(♥️8);
}
function(){
    ♥️6()
    ♥️7()
    ♥️8()
}
```
###JS中AOP的核心内容
```
Function.prototype.after = function(fn){
    var _this = this;
    return function(){
        var res = _this.apply(this,arguments);
        fn.apply(this,arguments);
        return res;
    }
}
Function.prototype.before = function(fn){
    var _this = this;
    return function(){
        fn.apply(this,arguments);  
        return    _this.apply(this,arguments);
    }
}
```
通过重写原型链，我们在调用函数的时候，就可以使用原型链上的before 和after方法。
下面我们通过一个代码片段看一看，代码是怎么运行的，可以将代码在浏览器中运行一下，跟着```debugger;```，多试几次，更容易理解。
####测试代码讲解
```
Function.prototype.before = function(fn){
    var _self = this;
	console.log(this)
	debugger;
    return function () {

        //返回值判断,如果为false那么不执行,表示业务逻辑执行失败
        if(fn.apply(this,arguments) === false){
            return false
        }
		console.log(this)
		debugger;
        return _self.apply(_self,arguments)
    }
}

Function.prototype.after = function(fn){
    var _self = this;
	console.log(this);
	debugger;
     return function () {
        var result = _self.apply(_self,arguments)
		console.log(this)
		debugger;
        fn.apply(this,arguments)
        return result
    }
}


function test(val){
	debugger;
    console.log(val)
}

// 编写初始处理
function fInter(val){
    console.log(val-1)
}
//编写后续处理
function fOuter(val){
    console.log(val+1)
}
console.log('before')
test.before(fInter)  //返回一个为执行函数
console.log('before()')
test.before(fInter)(8);//传入参数并将上面返回的函数执行
console.log('after()')
test.after(fOuter)(8)//传入参数并执行返回的函数
test.before(fInter).after(fOuter)(8)//分为两步，test.before(flnter)首先返回一个函数，
                //这个函数里是需求的顺序，然后将这个函数的作为作用域传到
                //.after(fOuter)中去，作为一个单独的函数，及_self.apply(_self,arguments)进行执行。
```
###  Function.prototype.after中的result  作用
```
Function.prototype.after = function(fn){
    var _self = this;
	console.log(this);
	debugger;
     return function () {
        var result =  _self.apply(_self,arguments)
		console.log(this)
		debugger;
         fn.apply(this,arguments)
        return result 
    }
}

test.before(fInter).after(fOuter)(7)
```
result是为了记录之前添加好的函数的集合
```
打印结果:
//test(val){
    console.log(val)
}
//function(){ 返回加工后的函数：可以称为addFnbefore()
    fnBefore()
    text()
}
//funtion(){  返回最终加工后的函数也就是接下来要运行的函数：endFn()
    addFnbefore()
    fnAfter()
}
最后执行endFn() 
//6
//7
//8
```
### 如何打断
例如我们在提交请求加入了一个```fnBefore```用来判断用户输入参数是否有效，就需要在错误的时候打断提交操作
```
Function.prototype.before = function(fn){
    var _self = this
    return function () {
        //返回值判断,如果为false那么不执行,表示业务逻辑执行失败
        if(fn.apply(this,arguments) === false){
            return false
        }
        return _self.apply(_self,arguments)
    }
}

Function.prototype.after = function(fn){
    var _self = this;
     return function () {
        var result = _self.apply(_self,arguments)
        //after有返回值判断,如果为false那么不执行,表示业务逻辑执行失败
        if(result === false){
            return false;
        }
        fn.apply(this,arguments)
        return result
    }
}
```
如果```fn```，为```false```函数将停止执行。
###如何不修改```Function```的原型链前提下实现AOP
持续更新


[学学AOP之装饰者模式](https://segmentfault.com/a/1190000004351323)
[js 面向切面编程](http://m.blog.csdn.net/qq_17347575/article/details/78039216)
[AOP PPT讲解](https://wenku.baidu.com/view/cf5d7f766294dd88d1d26b29.html)









