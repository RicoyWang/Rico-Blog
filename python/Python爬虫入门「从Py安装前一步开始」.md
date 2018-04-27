>记录Pyhon进行爬虫的时候，遇到了问题，写一些教程建议给和我一样想玩一些好玩的事情的人，感谢Gay海的悉心指导。

```
pyhon -version 3.6版本
###关键字前要指南>>>>>
编辑器： pycharm 首推必备，错误提示，代码检查等功能都很赞。
pyenv:  python 版本管理器
pip : python 包安装器
Scrapy: python爬虫框架，易上手功能强大
Request: 模拟发送请求的库
Xpath: HTML解析器
Beautifu soup: HTML解析器
```
####开始前
 本文整体来说内容是很详细的，针对安装部分由于不同系统安装可能不一样，网站上都会有很多教程，在这里就不水文字了，按照阅读顺序进行实践是没有问题的，一些要注意的坑文章也有指出。
- pyenv 版本管理安装和切换python版本
  ```
  install: 安装指定软件,例如 pyenv install 3.5.2
  loca，global l: 在全局或者局部下切换到指定版本,例如pyenv local | global 3.5.2
  pyenv versions: 查看已安装版本
  ```
- Pycharm 破解
  - 1、打开激活窗口
  - 2、选择 Activate new license with: License server （用license server 激活）
  - 3、在 License sever address 处填入 `http://xidea.online`
  - 4、点击 Activate 进行认证。
  - 5、认证完成就可以使用了。

 -  pip
    ```
    pip install SomePackage 安装版本
    pip show --files SomePackage 查看已安装
    pip install --upgrade SomePackage 升级
    pip uninstall SomePackage 卸载
    ```

 - Scrapy
  按照Scrapy官网上的例子可是大概了解Scrapy是怎么进行网络爬虫简单的说就是Scrapy按照提供的URL将页面下载下来，下载下来的文件将视为一个对象，在经过Xpath对页面结构进行解析，提取需要的文字内容，图片视频等文件。实操注意事项：
    - 在各位决定独自爬去一个网站之前，先通过一下操作确认页面是否能够无碍操作，
    ```
    scrapy shell
    fetch('https://www.baidu.com')
    ```
    开启shell命令行尝试连接将要爬取的网站，因为，有的页面可能做了反爬虫处理，在开始学爬虫前，尽量选择难度小的网站爬取，在了解爬虫后，再摸索如何通过添加request headers 或者模拟登陆等操作爬取更有难度的网站。报错不要着急，可能你只是被丑拒了···
     
   - 如果页面下载成功接下来就需要对下载下来的页面进行解析，常用的有两个选择，```beautifulsoup```,```Xpath```,```beautifulsoup```语法更像是前端的```Jquery```操作:```soup.find_all('a')```，而```Xpath```则是类似文件目录形式 : 
 ```xpath('//div/p/a/text()')```，我比较推荐使用```Xpath```语法更简单，最重要的是在firefox 和Chrome 浏览器上可以查看元素的```Xpath```语法
![](http://upload-images.jianshu.io/upload_images/6095375-dc36478e85427c1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
   - Scrapy将筛选的url添加到队里中等待处理： 
```yield scrapy.Request("http://www.baidu.html', callback=self.parse)```
  [Scrapy官网](http://scrapy-chs.readthedocs.io/zh_CN/0.24/intro/tutorial.html#id9)

  - Xpath语法
     - ```xpath('//div/p/a/text()')```处理得到的结果是一个可以再次进行```xpath()```解析的对象，如果想要获取一个HTML字符串需要在对象后面加```.extract()```方法如：```image_name = item.xpath('div[@class="mala-text"]/div[@class="mtitle"]/a/text()').extract()```，如果想要再次进行筛选解析，不加即可
    - 基础语法
    ```
    question      # 选取所有 question 元素的所有子节点
    /question     #选取根元素 question
    question/a    # 选取 question 元素下所有为 a 的子元素
    //div         # 选取所有的 div 元素，不论其出现在文档的任何地方
    question//div # 选取 question 元素下所有的 div 后代元素 (/ 选取的是直接子    元素，这里是所有的后代元素)
    question//span/text() #选取 question 元素下所有 span 元素中的文本值
    question//a/@href     #选取 question 元素下所有 a 元素中的 href 属性值。 @ 后面可以是任意属性名，均可以取到值
    ```
    - 带有限定性质的语法
    ```
    /question/div[1]        # 选取 question 的第一个 div 子元素。 注意这里第一个是从索引 1 开始的
    /question/div[last()]   # 选取 question 第最后一个 div 子元素
    /question/div[last()-1] # 选取 question 的倒数第二个 div 子元素
    //div[@lang]            # 选取所有拥有lang 属性的 div 元素
    //div[@class="mala-text"]      # 选取所有 class 属性为 mala-text 的 div 元素
    ``` 
    - 需明确```//|/|.|..|```的用法
        - ```// ``` 匹配文档所以元素，不考虑所在位置。
        - ```.```选取当前节点，然后在该节点下匹配
        - ```..```选取当前节点的父节点。
        - ```@```选取属性。```div[@class="mala-text"] ```
    - 一个例子实践以上内容
    ```
    items = response.xpath('//div[@class="ui-module"]')
            for item in items:
                image_name = item.xpath('div[@class="mala-text"]/div[@class="mtitle"]/a/text()').extract()
                image_url = item.xpath('div[@class="mala-text"]//img/@src').extract()
    ```
    - 注意，在写筛选规则时，最好在命令行处打印出将要被筛选的对象(```.extract()```***过的HTML***)，查看具体的位置再写规则，
[Xpath语法大全](http://blog.csdn.net/u014689510/article/details/51629221)


###最后
  筛选出自己想要的内容，比如文字内容，图片，视频，存到本地或者存到数据库都可以。当然性```♂```趣是王道，我存的是2G的图片，你懂的  →_→
