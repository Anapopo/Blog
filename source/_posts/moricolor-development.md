---
title: MoriColor-For-Hexo 开发手记
date: 2018-05-17 23:03:47
tags:
- theme
---
这里记载了 `MoriColor For Hexo` 的详细参数、开发日志和感谢列表等。
{% ruby 超電磁砲|レールガン %}

<!-- more -->

### 文章页参数
| 元素 | 字号 |
| --- | --- |
| 正文 | 1.7rem |
| 引用 | 1.5rem |
| 代码块 | 1.4rem |
| 版权 | 1.5rem |

### Changelog

#### 2018-07-28
+ 逐渐去 JQuery 化，添加 `thumbnail`
#### 2018-07-14
+ 修改文章目录样式
+ 调整 {% ruby 文章页|Post %} 结构
#### 2018-05-28
+ 添加 {% ruby 时间线|Timeline %}
+ 模仿 `VSCode` 重写代码块
+ 实验性地为代码块添加 {% ruby 工具栏|Toolbar %}
+ 添加 {% ruby 友情链接|Friends Link %} 页面

#### 2018-05-25
+ 调整 {% ruby 主页|Home %} 和{% ruby 文章页面|Post %} 布局
+ 优化 `bottom-tools` 元素布局
+ 将文章页面内标签移至左侧
+ 实验性地添加文章目录，目前处于隐藏状态
+ 调整 {% ruby 引用块|Blockquote %} 样式

#### 2018-05-24
+ 调优 {% ruby 主页|Home %} 元素布局

#### 2018-05-21
+ 更换`Ruby`插件为`hexo-tag-ruby`
+ 修复代码高亮异常的{% ruby 渲染|Render %}问题
+ 解决代码高亮丢失行首空格的问题

#### 2018-05-20
+ 重写代码块样式，添加官方 HighLight.js 支持

#### 2018-05-18
+ 使代码块适应代码宽度
+ 添加文章出处声明
+ 自动在中文与英文字符中插入一个半角空格
+ 使用 `hexo-ruby-character` 为 {% ruby 中文|Chinese %} 添加注音功能
+ 添加 {% ruby 表格|Table %} 样式
+ 添加 Chrome 标签栏主题色

#### 2018-05-17
+ 引入 `Zoom.js` 用于图片缩放
+ 添加上一篇、下一篇文章跳转
+ 添加灰度背景图像，`/* 灰度背景样式来自于 https://imjad.cn/ */`
+ 部分透明化文章页面中的代码`pre`部分
+ 微调文章正文、引用、代码块的字号

### Credits

+ [森の色](https://yumoe.com/)
+ [猫与向日葵](https://imjad.cn/)
+ [neoFelhz's Blog](https://blog.nfz.moe/)
+ [LWL Hitokoto API](https://blog.lwl12.com/read/hitokoto-api.html)
+ [text-autospace](http://mastermay.github.io/text-autospace.js/)
+ [Markdown Ruby 标签拓展插件测试](https://imjad.cn/archives/code/markdown-ruby-tag-extension-plugin-test/)
+ [hexo-ruby-character](https://github.com/JamesPan/hexo-ruby-character/)
+ [从零开始制作 Hexo 主题](http://www.ahonn.me/2016/12/15/create-a-hexo-theme-from-scratch/)
+ [制作Hexo主题详细教程](http://blog.geekaholic.cn/2017/03/06/%E5%88%B6%E4%BD%9CHexo%E4%B8%BB%E9%A2%98%E8%AF%A6%E7%BB%86%E6%95%99%E7%A8%8B%EF%BC%882%EF%BC%89/)
