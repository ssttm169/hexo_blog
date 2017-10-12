# Hexo开启post_asset_folder后, 安装hexo-asset-image,不起作用的问题

使用Hexo和github搭建博客，真是挺有趣的事情，因为对于我们来说，随便敲几行代码，就可以拥有一个非常漂亮的博客。 但在写作的过程中，使用Typora(一个markdown编辑器)插入图片的时候，非常不顺手。后来了解到用hexo-asset-image可以提升我的写作效率,,所以开始吧。

<!-- more -->



<br>





#### 安装插件

在nodejs里边安装

```
npm install hexo-asset-image --save
```

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/1.png)

<br>






把在hexo根目下的_config.yml设置

```xml
post_asset_folder: true
```



<br>



#### 新建文章

安装完了之后，新建一篇名为test-img的文章

```
hexo new [layout] test-img
```


![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/2.png)






现在可以看到source/_posts里边，除了有test-img.md，还多一个文件夹 test-img


![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/3.png)



<br>

在里边我放一张名为me.jpg的图片, 插入图片的时候，输入  

```
![](test-img/me.jpg)
```



如图所示：

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/4.png)



#### 发现问题

当我回到nodejs的时候，发现不对劲~

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/5.png)



然后，我去页面看，图片不显示， chrome-devtools显示，找不到图片。

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/6.png)




当我用hexo g生成文件的时候，发现报错， 如下：

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/7.png)

FATAL something's wrong. May be you can find the solution here.....

Error: ENOENT: no such file or directory, open 'D:\nodejs6.5\hexo_blog\public\20
17-09-29-test-img.html\me.jpg'  

仔细一看路径是

```html
\public\2017-09-29-test-img.html\me.jpg
```


在public文件夹里边也找不到2017-09-29-test-img.html文件夹。

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/8.png)



百思不得期解，我了解别人的博客都是的链接都是       域名/年份/月份/文件名  的路径， 比如：http://www.54tianzhisheng.cn/2017/09/23/Guava-limit/

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/9.png)


而且我的路径是 http://www.itomtan.com/2017-09-27-how-to-use-wechat-debug.html， 所有的html都是放到

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/10.png)



#### 找到原因

为了满意的我好奇心~~，最终我找到了原因。原来，之前我把hexo根目下的_config.yml， 把permalink 设置为 year-:month-:day-:title.html，hexo就会在根目下生成html, 而导致 生成 相关的文件夹失效。

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/11.png)



是 permalink 和 post_asset_folder 这两个参数是相互影响的。



#### 解决方案

把hexo根目下的_config.yml， 把permalink 设置为 :year/:month/:day/:title/

```xml
permalink: :year/:month/:day/:title/
```

好，重新hexo clean && hexo g , 得到的就是我们要的那种方路径，而且图片显示正常了。

![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/12.png)

<br>

温馨提示一下：

用Typora插入图片时  输入

```
![](文件名.png)
```

的时候，虽然在图片网页上能够正常显示，但是在Typora编辑时无法显示，那如何才能让Typora正常显示呢? 这里分享一个小技巧：

在Typora中, 选择Edit > Image Tools >  Use Image Root Path， 然后选择你要的相关文件夹，选择之后，在md文件头上 会多一个 typora-root-url: xxxx, 好了，现在可以正常显示了。具体操作看下图。



![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/13.png)


![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/14.png)


![image](http://www.itomtan.com/2017/09/29/the-problem-when-use-post-asset-folder/15.png)
