---
layout:     post
title:      "写在最前面"
subtitle:   "Welcome to coderyyy's blog."
date:       2018-11-14 14:00:00
author:     "coderyyy"
header-img: "img/post-bg-2015.jpg"
tags:
    - Other Life
---

## 前言
写一个自己的blog这件事一直断断续续了几年，一是期间各种客观因素导致没能成行，比如密码忘了/公司倒了/电脑毁了之类；二是比较懒，也没遇到比较好的轮子，各种三方的UI实在不忍直视/要么就是连code都无法插入（好吧其实就是懒。

但是！现在都8102年了，没有一个blog怎么好意思在这个世界呼吸空气。

这里先介绍一下这个blog的搭建过程，以及小部分技术实现。
<p id = "pre" ></p>
___

## 正文

搭建过程其实并不算太复杂，但是由于github.io偏DIY的技术向，还是遇到了一些问题，稍作记录。
<p id = "pages"></p>
之前就有被安利过 [GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/) 的基于**github.io**的blog实现，一直没有去做，正好借这个机会拿过来吃一吃。

稍作了解后不难发现，优点还是比较明显的：

* **Markdown** 的语法熟悉又美丽
* 每天的Git workflow ，**Git Commit 即 Blog Post**
* 利用 GitHub Pages 的域名和免费无限空间，不用自己折腾服务器
* [Jekyll](https://github.com/jekyll/jekyll) 的高度定制化

--

#### 1. 创建一个repo
* 首先申请一个[GitHub](https://github.com/)账号并登录，github.io完全是基于github实现，github pages的介绍点[这里](https://pages.github.com/)。

* 进入账号后，从[这里](https://github.com/new)新建一个自己的repo，记住命名格式**必须**为：[github-username].github.io。
* 将创建的repo clone到本地：

 ```$ git clone https://github.com/tobiasalin/tobiasalin.github.io```

#### 2. 编写简单的首页
```
$ cd tobiasalin.github.io
$ echo "Hello World!" > index.html
$ git add index.html
$ git commit -m "Init commit"
$ git push origin master
```

执行完毕，打开博客首页`https://<github-username>.github.io`，不出意外的话就可以看到属于你自己的Hello World博文了，出了意外刷新治百病。

#### 3. 选择Jekyll主题
此部分**前端**技巧较多，前期可以先fork一个现成的国人开发的主题，毕竟 [Jekyll](https://github.com/jekyll/jekyll) 已经吃下了3w+的star，拥有很成熟的社区，重复造轮子很累且不值得。

```$ git clone git@github.com:Huxpro/huxblog-boilerplate.git```

clone该repo后，直接将内容copy至自己的repo下，覆盖掉自己repo内容。

**Note：**这里注意下，需要将_config.yml这个文件内容修改成自己想要展示的内容，否则会有一定的麻烦，后面需要从代码里面找，相当麻烦。

#### 4. 使用git命令发布blog
Jekyll对于blog的位置以及文件命名有严格的[规定](https://jekyllrb.com/docs/posts/#creating-post-files)，亦即：blog的位置必须位于文件夹**_posts**下，命名必须为**YYYY-mm-dd-title-you-want.md**

在_posts文件夹下编辑好blog后，使用以下命令提交blog：

```
$ git add _posts/2018-11-14-first-blog.md
$ git commit -m "My first github.io blog"
$ git push origin master
```
稍作等待，再访问自己的blog主页，即可看到提交的blog。

#### 5. 本地预览blog
如果只做了一个小小的改动就去push master显然是有点不合理的，这里介绍一下使用Jekyll构建本地网站预览的方法：

* 安装Ruby( [What is Ruby?](https://www.ruby-lang.org/en/) )，可以参照[官网教程](https://www.ruby-lang.org/en/downloads/)。如果你是Mac，那么：
	
	```$ brew install ruby```

* 安装GitHub Pages (简介见[上文](#pages))，安装完成Ruby后执行： 
	
	```$ gem install github-pages```
* 开启Jekyll本地服务：
	
	```
	$ cd <your_repo_name>.github.io
	$ jekyll serve --watch
	```

完成以上步骤后，Jekyll会监听本地的`4000`端口（这里小概率会遇到pid冲突，通过系统命令查看Jekyll监听的端口号），打开`http://127.0.0.1:4000`就可以本地查看自己的博文效果了。
## 写在后面
搭建过程其实很快，但是如果想要自己去做出其他blog/酷炫的一些效果来，还是需要在前端技巧上进行深耕的。

轮子造好，后面的路应该要好好走下去才是（figh...ting。


* 如果你不了解git，[戳这里](https://git-scm.com/doc)
* 如果你对markdown的语法不熟悉或者想多了解，[这里](https://www.jianshu.com/p/b03a8d7b1719)有一个比较详尽的说明
* 如果你希望提升blog的逼格，或者对前端感兴趣，[转这里](http://www.w3school.com.cn/h.asp)

