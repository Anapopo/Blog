---
title: 获取学校图书馆大约50万藏书信息
date: 2017-08-04 20:57:41
tags:
 - php
---
### 事件起因
因为我在图书馆借书后总是会忘记还书日期，有一次还因为忘记还书欠了图书馆20多软妹币 (此处捂脸QWQ)  
于是就想着怎么地 {%ruby 好好学习一下|干他一票 %} 图书馆官网  
之后发现图书馆竟然用的是 [OPAC](https://zh.wikipedia.org/wiki/OPAC)，然后我看到了 [这一篇文章](http://blog.nudtcat.org/WebSecurity/%E6%B1%87%E6%96%87opac%E5%9B%BE%E4%B9%A6%E9%A6%86%E5%90%8E%E9%97%A8%E5%AF%BC%E8%87%B4%E6%BA%90%E7%A0%81%E6%B3%84%E6%BC%8F/)  
然后就~~大费周章~~轻而易举地 `dump` 出了学校图书馆管理软件的一些信息，并拿到了 OPAC 的源码，嗯是用~~世界上最棒的语言~~PHP写的。

<!-- more -->

### 实验过程
随便找了本书，先访问一下网址看看 `http://<学校图书馆地址>/opac/item.php?marc_no=0000478102`  
先标准般地右键审查一下，当分析 `HTTP Header` 之后，发现不需要 `Cookie` 就可以访问书籍的介绍页面。然后瞥见一个偌大的二维码！
![qrcode](https://moe-static.b0.upaiyun.com/img/how-to-get-50w-books-info/qrcode.png)  

```html
<img src="ajax_qr.php?qrcode=6aKY5ZCN77yaR2l054mI5pys5o6n5Yi2566h55CGDQrotKPku7vogIXvvJoo576OKSBKb24gTG9lbGlnZXIsIE1hdHRoZXcgTWNDdWxsb3VnaOiRlw0K6aaG6JeP5L%2Bh5oGvKOe0ouS5puWPty%2Fppobol4%2FlnLAv5Zyo6aaGL%2BWPr%2BeUqCnvvJoNClRQMzExLjU2LzY1MSDnp5HmioDlm77kuablgJ%2FpmIXihaHlrqQgMC8yDQo%3D" border="0" width="165" height="162">
```

嗯`~ o(*￣▽￣*)o`，看来这是一个用php生成的二维码，内容就藏在`qrcode`参数里面  
我们先把它小心翼翼地取出来呢，然后使用 php 也好，python 也好, 总之把它解码了任务就完成  
我~~一眼就看出来了~~思虑良久，尝试先 `urldecode` 之后再 `base64_decode`, 最后内容就乖乖地出现了(￣▽￣)"233

```text
题名：Git版本控制管理
责任者：(美) Jon Loeliger, Matthew McCullough著
馆藏信息(索书号/馆藏地/在馆/可用)：
TP311.56/651 科技图书借阅Ⅱ室 0/2
```

### 项目
[Node.JS实现查询已借阅书籍](https://github.com/Anapopo/OPACHelper)

### 后记
-  实现 Cookie 过期后自动获取新 Cookie
-  抓取图书借阅信息
-  抓取50万本藏书存入数据库
-  通过识别验证码直接登陆图书馆
-  实现已借阅书籍的一键续借

### 总结

万事开头难, 当你发现了一本书的奥秘的时候, 那么50万当然就不成问题了, 直接`for`循环就可以了, 需要的只是时间。
