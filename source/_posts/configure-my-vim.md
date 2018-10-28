---
title: 「VIM」编辑器的配置
date: 2018-03-28 12:22:22
tags:
 - linux
 - vim
---
## 引言
记录一下个人的宇宙第一编辑器「VIM」的配置过程，以防下次折腾的时候还要翻阅官方文档。
一般而言，Linux 系统都会默认安装 VIM ，就跳过安装了。着重记录安装Vundle作为插件管理器这部份。

<!-- more -->

## 安装
第一步，先把这个[项目](https://github.com/VundleVim/Vundle.vim.git)拉回家，安置在 `~/.vim` 目录下。每个用户在 `/home` 目录下都有一个以用户名命名的文件夹，`.vim` 表明该文件夹不可见。  
接下来上拉取的代码：

```bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

## 编写vim的配置文件
一些注意点：
* Vundle 支持通过 Git 仓库引入插件
* 插件的引入必须在 begin 和 end 之间

```bash
" 基础配置
set nocompatible
filetype plugin indent on
set number
set encoding=utf-8
set shiftwidth=2
set tabstop=2
set autoindent
set ambiwidth=double
set expandtab
syntax on

" 引入Vundle
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" 使用Vundle作为插件管理器
Plugin 'VundleVim/Vundle.vim'

" 个人使用的一些插件
Plugin 'mattn/emmet-vim'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'bling/vim-airline'
Bundle 'luochen1990/rainbow'
call vundle#end()

" 以下省略了各个插件的配置
```
## 安装插件
只要打开 vim 窗口，执行指令 `:PluginInstall` ，插件们就会乖乖地跑进你的文件夹啦。
