###1 概述
JavaScript提供2个方法在浏览器端储存数据：sessionStorage 和 localStorage。

- `sessionStorage`：保存的数据用于浏览器的一次会话，当会话结束（通常是该窗口关闭），数据被清空；
- `localStorage`：保存的数据长期存在，下一次访问该网站的时候，网页可以直接读取以前保存的数据。除了保存期限的长短不同，这两个对象的属性和方法完全一样。

它们很像`cookie`机制的强化版，能够动用大得多的存储空间。目前，每个域名的存储上限视浏览器而定，Chrome是2.5MB，Firefox和Opera是5MB，IE是10MB。其中，Firefox的存储空间由一级域名决定，而其他浏览器没有这个限制。也就是说，在Firefox中，a.example.com 和 b.example.com 共享5MB的存储空间。另外，与cookie一样，它们也受同域限制。某个网页存入的数据，只有同域下的网页才能读取。

通过检查window对象是否包含` sessionStorage` 和 `localStorage `属性，可以确定浏览器是否支持这两个对象。

```
function checkStorageSupport()
{
    // sessionStorage
    if (window.sessionStorage) {
        return true;
    } else {
        return false;
    }
   
    // localStorage
    if (window.localStorage) {
        return true;
    } else {
        return false;
    }
}
```
 ###2 操作方法

`sessionStorage` 和 `localStorage` 保存的数据，都以“键值对”的形式存在。也就是说，每一项数据都有一个键名和对应的值。所有的数据都是以文本格式保存。
其中value需为可转化为字符串的对象
`sessionStorage` 操作
```
sessionStorage.setItem("key","value");                 // setItem方法，存储变量名为key，值为value的变量
var valueSession = sessionStorage.getItem("key");      // getItem方法，读取存储变量名为key的值
sessionStorage.removeItem('key');                      // removeItem方法，删除变量名为key的存储变量
sessionStorage.clear();                                // clear方法，清除所有保存数据
```

localStorage 操作

```
localStorage.setItem("key","value");                   // 存储变量名为key，值为value的变量
localStorage.key = "value"                             // 同setItem方法，存储数据
var valueLocal = localStorage.getItem("key");          // 读取存储变量名为key的值
var valueLocal = localStorage.key;                     // 同getItem，读取数据
localStorage.removeItem('key');                        // removeItem方法，删除变量名为key的存储变量
localStorage.clear();                                  // clear方法，清除所有保存的数据

// 利用length属性和key方法，遍历所有的数据
for(var i = 0; i < localStorage.length; i++)
{
    console.log(localStorage.key(i));
}

// 存储 localStorage 数据为 Json 格式
value = JSON.stringify(jsonValue);                     // 将 JSON 对象 jsonValue 转化成字符串
localStorage.setItem("key", value);                    // 用 localStorage 保存转化好的的字符串

// 读取 localStorage 中 Json 格式数据
var value = localStorage.getItem("key");              // 取回 value 变量
jsonValue = JSON.parse(value);                        // 把字符串转换成 JSON 对象
```
###3 `storage` 事件
当储存的数据发生变化时，会触发 storage 事件。我们可以指定这个事件的回调函数。

```
window.addEventListener("storage",onStorageChange);  //监听事件
```

除了`key`属性，`event`对象的属性还有三个：
- oldValue：更新前的值。如果该键为新增加，则这个属性为null。
- newValue：更新后的值。如果该键被删除，则这个属性为null。
- url：原始触发storage事件的那个网页的网址。

值得特别注意的是，该事件不在导致数据变化的当前页面触发。如果浏览器同时打开一个域名下面的多个页面，当其中的一个页面改变`sessionStorage`或`localStorage`的数据时，其他所有页面的storage事件会被触发，而原始页面并不触发`storage`事件。可以通过这种机制，实现多个窗口之间的通信。所有浏览器之中，只有IE浏览器除外，它会在所有页面触发`storage`事件。
未完，待填坑，反正也没人看··············
