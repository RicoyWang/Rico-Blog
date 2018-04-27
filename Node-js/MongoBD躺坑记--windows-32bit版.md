>最近在按照搭博客的教程学习express，需要用到MongoBD,教程地址与MongoBD相关地址在尾部。

系统：windows 32bit 
下载使用时间：2017.9.27
#### 一、下载地址
官网下载地址：[https://www.mongodb.org/downloads#production](https://www.mongodb.org/downloads#production)
坑：官网上都是64位的，32位苦逼电脑没有，下面是32位系统下载地址，直接点击最新版本下载即可。
[MongoDB for Windows 32-bit ](https://www.mongodb.org/dl/win32/i386)
####  二、安装
默认安装地址为```C:\Program Files\MongoDB```
#### 三、mongodb.exe闪退或mongod命令不是内部或外部的命令
安装后要配置环境变量计算机->属性->高级系统设置->环境变量->底部变量中找到 path->编辑->在输入框最后加入分号->分号后面加 mongodb中bin的目录 
如果是默认安装地址则为```C:\Program Files\MongoDB\Server\3.2\bin;```
注意与之前的变量要用```;```隔开，英文的。
如果开始打开了cmd，需要关闭后重新打开才会生效
####四、新建data文件夹
文件夹需要放在安装目录根目录下，及默认安装地址为```C:\Program Files\MongoDB```，那么就要新建```C:\data\db```

####五、32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
针对32bit操作系统的坑，输入一下命令
```mongod --dbpath C:\data --journal --storageEngine=mmapv1```
####六、需要打开两个CMD窗口，
一个用于建立链接，不能关闭，关闭则断开链接，一个窗口用于操作数据库。建立链接后可以在网页上输入```http://localhost:27017/```如果页面显示```It looks like you are trying to access MongoDB over HTTP on the native driver port.```则为链接成功。

###思路整理
安装好后，新建了data文件夹，首次安装操作，第一次执行```mongod --dbpath C:\data ```是为了确认data文件夹地址同时生成日志，第二次输入```mongod --dbpath C:\data ```是为了启动mongodb，第三次新建CMD窗口输入```mongo```就可以在这个窗口下操作数据库了。
之后如果在需要使用，只需启动，和新建窗口输入```mongo```
参考文章
[使用express和mongodb搭建一个极简博客](http://www.yi-jy.com/2016/04/09/a-light-blog-with-express-mongodb/)--很详细的文章，手摸手教学。
[同上，很多干货](https://github.com/nswbmw/N-blog)
[windows安装mongodb](http://www.jianshu.com/p/4e6e4f24c40d)
[32-bit servers don't have journaling解决方案](http://jingyan.baidu.com/article/76a7e409e1bb49fc3b6e1516.html)--这个相对靠谱，google其他答案完全不知道干嘛的。
[mongoDB数据库操作方法](https://github.com/qianjiahao/MongoDB/wiki/%E9%85%8D%E7%BD%AE%E3%80%81%E5%90%AF%E5%8A%A8MongoDB)
