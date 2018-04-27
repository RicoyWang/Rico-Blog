>之前对数组迭代方法只是一个很笼统的认识，抽了个时间把整个都过一遍，把迭代方法用图形的方法介绍一下，让大家有更清晰的认识，也方便知识的整理。由于水平有限，如果有错误的地方请大家指正。

 ####快速查看数组迭代方法的默认参数

***一个例子☂***：```
[1,2,3,4,52].forEach(console.log)```
打印处的结果分别是```value,index,array```，
  - **```value```**为数组子项的值等同于```array[i]```,
  - **```index```** 为数组子项的下标等同于 ```i```
  - **```array```** 为数组本身及上面例子```[1,2,3,4,52]```整个数组。
其他```map```,```filter```等，也可以用此方法查看默认参数

  - ***♨```forEach```***
```
####   arr.forEach()   = >数组里的子项做一件事，没有返回值。
      [ 1 ,  2 ,  3 ,   4 ,  52 ]
                  ∏      
  forEach(function(item,index){console.log(item)})
         ↓   ↓    ↓    ↓     ↓
        1 ,  2 ,  3 ,   4 ,  52     <－非数组，console.log()打印出的结果。
```
```jQuery```的```$.each()```方法，默认传参和```forEach```是不一样的，```$.each(function(index,item,array){})```。
  - ***♨```map```***
```
####  arr.map()  = > 将数组子项进行操作，并重新组合为一个新的数组。
      [ 1 ,  2 ,  3 ,   4 ,  52 ]
                  ∏      
      map(function(item,index){return item*2})
         ↓   ↓    ↓    ↓     ↓
      [ 2 ,  4 ,  6 ,   8 ,  104 ]   <···生成的新数组并返回
```
如果没有```return ```，新产生的数组将会是一个多个```undefined```的集合 如：```[undefined, undefined, undefined, undefined, undefined]```
  - ***♨```filter```***
```
####  arr.filter()   = >  筛选符合条件的子项，组成新的数组。
[ 1 ,        2 ,      3 ,      4 ,       52 ] 
                  ∏      
  forEach(function(item,index){return item > 3})
   ↓         ↓        ↓        ↓          ↓
[ false ,  false ,  false ,   true ,    true ]     <－子项大于3则为true,小于3则为false,
   ↓         ↓        ↓        ↓          ↓
[                              4     ,    52 ]     <－将满足条件的子项集合组成新数组
```
返回值将会和```Boolean值```进行对比，是通过```==```进行比对，而不是```====```，返回值如果不是```Boolean值```，在对比前是会进行类型转换的,如下：
```
var arr = [0, 1, 2, 3];
var arrayFilter = arr.filter(function(item,index) {
    return item;
});
console.log(arrayFilter); // [1, 2, 3]
```
返回的```0```将作为```false```对待，新数组里将不会有对应子项。
  - ***♨```reduce```***
```
####  arr.reduce()  = > 将数组子项进行操作，并重新组合为一个新的数组。
  [ 0,  1 ,         2 ,        3  ,  ]
                 ∏      
reduce(function(prev,curr,index){return prev+next })
    ▽   ▽
    0 + 1  →  1
              ↓     ▽
              1  +  2  →  3
                          ↓     ▽
                          3  +  3  →   6
                                       ↓        ▽
                                       6  + undefined 如果(curr ===undefined 则跳出操作)
                                       ↓  
                                       6    <···· 将迭代求和的结果返回。
```
**用法**：```array.reduce(callback[, initialValue])``` 
  -  ```initialValue```参数为可选，如果设置则该值为第一次遍历的```prev```，```curr```将会从```arr[0]```开始赋值遍历，总共遍历次数为```arr.length ```及数组长度。
  - 如果```initialValue```缺省，```reduce```开始遍历数组时，第一次会设置```prev``` 为```arr[0]```，```curr```为```arr[1]```，之后在遍历数组每一项的时候都会获取当前遍历子项为```curr```参数，上一次遍历获取的结果为```prev```。当遍历完数组最后一项之后，```curr ===undefined```则结束遍历。开始遍历是从```arr[1]```开始的，所以总共遍历的次数是```arr.length -1```。
  - ```reduce```不仅可以处理简单的一维数组，也可以处理多维数组
     - 对数组对象属性进行求和☂

    ```
    var result = [
      {
        subject: 'math',
        score: 88
      },
      {
        subject: 'chinese',
        score: 95
      },
      {
        subject: 'english',
        score: 80
      }
    ];
    var sum = result.reduce(function(prev, cur) {
      return cur.score + prev;
      }, 0);
    ```
     - 合并二维数组☂
    ```
    var matrix = [
      [1, 2],
      [3, 4],
      [5, 6]
    ];

    // 二维数组扁平化
    var flatten = matrix.reduce(function (previous, current) {
      return previous.concat(current);
    });
    console.log(flatten); // [1, 2, 3, 4, 5, 6]
    ```
  - ***♨```every```***
```
####  arr.every()   = > 检测每一项是否都符合条件，返回Boolean值
[ 1 ,        2 ,      3 ,      4 ,       52 ] 
                  ∏      
  every(function(item,index){return item > 3})
   ↓         ↓        ↓        ↓          ↓
[ false ,  false ,  false ,   true ,    true ]     <－子项大于3则为true,小于3则为false,
   ↓         ↓        ↓        ↓          ↓
                     ↘↓↙   
                    false    <－最后必须每一项遍历返回的都必须是true 才返回true,
                                否则最终返回false
```
  - ***♨```some```***
```
####  arr.some()   = >  检测是否有某一项符合条件，返回Boolean值。
[ 1 ,        2 ,      3 ,      4 ,       52 ] 
                  ∏      
  some(function(item,index){return item > 3})
   ↓         ↓        ↓        ↓          ↓
[ false ,  false ,  false ,   true ,    true ]     <－子项大于3则为true,小于3则为false,
   ↓         ↓        ↓        ↓          ↓
                     ↘↓↙   
                     true    <－最后只要有一项遍历返回的是true 才则最终返回true,
                                如果每一项遍历返回的都是false，那么没得救了最终返回false。
```
由于```map``` ,```filter```返回的是一个新数组，所以也可以进行链式操作，
```
arr.map(item => item*2)
    .filter(item = > item>4)
    .reduce((prev,curr) => prev+curr)
```
总结✎：
  - ➹```map``` ,```filter```返回的一个新的数组
  - ➹```every```,```some```返回的是```Boolean值```
  - ➹```forEach```则是对数组进行某一些操作。没有返回结果
通过```forEach```方法可以自己封装实现 ```map``` ,```filter```,```every```,```some```

[一张图看懂JavaScript中数组的迭代方法：forEach、map、filter、reduce、every、some](http://www.cnblogs.com/shuiyi/p/5058524.html)


