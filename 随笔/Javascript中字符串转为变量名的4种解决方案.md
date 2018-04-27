>Js编程过程中，或多或少会出现需要动态生成一系列变量名，数量比较大时也不方便提前声明，这时候可能就需要先进行字符串拼接，然后将拼接的字符串转换为变量名。

当我们需要生成很多的变量，但是变量的名称是根据参数的不同而区分的，如 date_1,date_2,datet_3... （后面的数字是根据参数来的），那我们就需要写一个```var name = "test_"+num;```，这样的函数来生成变量名。

- 1、最简便的方法，通过```var name = eval('test_'+num)```这样就可以了，但并不推荐使用```eval```函数。
- 2、使用```window[name]  ```  等价于```window.name```可以改造为```var name = window['test_'+num]```，缺点一目了然，污染了全局变量，不过可以将其挂在到局部变量上。
- 3、使用new Function
>贡献自FCC-星空
```
function strToVar(str) {
        var json = (new Function("return " + str))();
        return json;
    }
strToVar("name")
console.log(name)//true,变量已生成，但未赋值。
```
eval()，new Function() 性能安全性并不好，不推荐使用
- 4、采用数组的形式
```
var arr = [];
for (var i = 0,var len = some.length; i < len; i++){
    arr[i]['test_'  +i]= null;
}
```
变量在数组中都有对应的下标，赋值和调用都不是很方便，但可能在特殊的使用环境中有奇效。
总结：最佳使用方法通过```var name = obj[strname]```实现字符串转为变量。
广告：希望了解更多前端非常规知识的可以查看我长期更新的[前端非常规知识总结](http://www.jianshu.com/p/364e5f2a009a)
