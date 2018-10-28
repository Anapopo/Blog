---
title: 「MoriColor」一款移植自 Typecho 的 Hexo 主题
date: 2018-04-21 22:24:18
thumbnail: https://i.loli.net/2018/04/21/5adb51d4cf633.png
tags:
- hexo
---
最近看到一款 Typecho 主题 [Moricolor](https://github.com/txperl/Moricolor-for-Typecho) ，很是喜欢，但我的博客是基于 Hexo 构建的，自然是不能通用的，于是学习了一下 Hexo 主题开发，简单地制作了这个主题样式的 Hexo 版本。

<!-- more -->

> 当然你现在看到的博客主题就是这个 Moricolor 了，不过目前还处于 {%ruby 兼容性优化|小白学习%} 阶段，~~暂不公开 Github 项目，等到 TODO 差不多完成的时候就开源。~~已经开源[戳我](https://github.com/Anapopo/Moricolor-for-Hexo)
### To-Do List

+ PWA化
+ 封面图
+ 修改部分静态资源的 CDN ，比如字体、Material Icon 等
+ 首页分页的样式

### 优化说明
+ 修改「Archive」和「Tag」页面的标签颜色
+ 修改文章页面上方标签和时间显示样式
+ Zoom.js 图片缩放
+ 文章上一页、下一页显示

更加详细的开发日志参见[此处](https://blog.kiko.space/2018/05/17/moricolor-development/)

### 参考资料
+ [Hexo 变量](https://hexo.io/zh-cn/docs/variables.html)
+ [Hexo 辅助函数](https://hexo.io/zh-cn/docs/helpers.html)
+ [EJS 模板语言](https://ejs.bootcss.com/)

### Credits
> 本程序源代码可任意修改并任意使用，但禁止商业化用途。
> 一旦使用，任何不可知事件都与原作者无关，原作者不承担任何后果。
> 如果您喜欢，希望可以在页面某处保留原作者 [Trii Hsia](https://github.com/txperl) 版权信息。
> 感谢。
