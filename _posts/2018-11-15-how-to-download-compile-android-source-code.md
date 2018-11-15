---
layout:     post
title:      "Mac系统下编译Android系统源代码"
subtitle:   "Android source code."
date:       2018-11-15 09:55:00
author:     "coderyyy"
header-img: "img/post-bg-2015.jpg"
tags:
    - Android
---

## 前言
我们知道，Android OS是基于Linux内核的移动操作系统，主要分成了

* **Application**
* **ApplicationFramework(Java)**
* **System Libraries(Native C/C++)/Android Runtime**
* **HAL(Hardware Abstraction Layer)**
* **Linux Kernel**

这几个部分。

作为一个Android Developer，熟练使用Android系统API是必不可少的（Application），但是在进阶的道路上，揭开API的『面具』，直面API背后的Android FW/Linux Kernel同样是一条必经之路。

今天就介绍一下，如何在Mac OSX上进行Android系统源代码的编译。

---

## 正文
[AOSP](https://android.googlesource.com/) 是Google领导的Android系统开源项目，旨在为开发者创建定制的Android堆栈版本提供源代码以及相关信息。AOSP的相关简介可以在[这里](https://source.android.com/source/index.html)查看。

下面就分步进行Android系统源码的编译：

#### 1. 创建区分大小写的磁盘映像
AOSP项目使用git进行版本控制，mac默认的磁盘对大小写是非敏感的，在这类文件系统中，可能导致git的一些命令失效(比如：**git status**)，因此需要创建出一个Case-sensitive的磁盘映像来存储AOSP项目代码。

* **创建磁盘空间：**

	```
	hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 40g ~/	android.dmg
	```
执行完成后，会在用户根目录(命令行cd ~)创建出一个未挂载的磁盘映像android.dmg（也可能是android.dmg.sparseimage）  

* **调整磁盘大小：**  
创建完成后，可对磁盘空间的大小进行调整
  
	```
	hdiutil resize -size <new-size-you-want>g ~/android.dmg.sparseimage
	```  

这里如果想要完整编译master分支，resize的大小建议120G左右，否则空间不足会导致编译重置，并且mac存储空间中会多一部分无法删除的占用。

双击用户目录的android.dmg.sparseimage即可挂载磁盘空间。

#### 2. 安装JDK
需注意，AOSP项目必须基于[Open JDK](http://openjdk.java.net/)的版本。

* 对于5.0.x的Android版本，需要下载安装[JDK7](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u71-oth-JPR)
* 对于4.4及以下的版本，需要安装[JDK6](https://support.apple.com/kb/dl1572?locale=zh_CN)
* 对于master分支或者6.0+的版本，安装[最新的JDK版本](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html#jdk-8u45-oth-JPR)

#### 3. 安装Xcode
* 从App store下载xcode安装，完成后需要  

	```
	$ xcode-select --install
	```

	**Note：**这里最好再运行一下  
	
	```
	sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
	``` 
 
	以防后面的步骤出现warning：  
	
	```
	Warning: Xcode appears to be installed but xcodebuild is unusable; some ports will likely fail to build.
	```

#### 4. 安装MacPorts
* 根据自己的系统来选择 [MacPorts的下载地址](https://www.macports.org/install.php)  
  
* 编辑.bash_profile文件（如果没有此文件，vi会自动创建）
  
	```
	vi ~/.bash_profile
	```

* 复制`export PATH=/opt/local/bin:$PATH`到.bash_profile文件中。

* 激活MacPorts命令：
  
	```
	source ~/.bash_profile
	```

#### 5. 通过MacPorts安装Make、Git、GPG

```
POSIXLY_CORRECT=1 sudo port install gmake libsdl git gnupg
```  

同时，由于mac中允许同时打开的文件描述符数较低，复制以下文本至.bash_profile文件中：  

```
ulimit -S -n 1024
```

#### 6. 下载源代码
可以参阅 [[官方教程]](https://source.android.com/setup/downloading)  

* 确保主目录下有一个`bin/`目录，在.bash_profile中添加：  
   
   ```
   mkdir ~/bin
   PATH=~/bin:$PAT
   ```
   激活：
   
   ```
   source ~/.bash_profile
   ```
* 下载repo  
	  
	```
	curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
	chmod a+x ~/bin/repo
	```
	
	关于repo的介绍，看[这里](https://source.android.com/setup/developing.html)。
* 初始化repo  
	挂载的磁盘目录一般在/Volumes文件夹下，`cd /Volumes`进去后，找到挂载的磁盘名，cd进入磁盘目录后：
	  
	```
	repo init -u https://android.googlesource.com/platform/manifest
	```
* 开始下载源码
	
	```
	repo sync
	```
	**Note：**这里下载的为AOSP master分支的代码，也就是当前最新的Android版本。如需repo特定分支，请使用：
	  
	```
	repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1
	```
	你可以从这里查看[源代码的各版本分支](https://source.android.com/setup/build-numbers.html#source-code-tags-and-builds)。
	  
	**如需使用本地镜像或者需要验证git，请参阅官方教程**。

#### 7. 编译
编译过程在经历了多个Android版本之后变得更加简单明了。可以参考 [[官方教程]](https://source.android.com/source/initializing.html)

* 设置环境

	```
	source build/envsetup.sh
	```
* cd到源代码目录后：
	
	```
	make -j17
	```
	GNU Make 可以借助 -jN 参数处理并行任务，通常使用的任务数 N 介于编译时所用计算机上硬件线程数的 1-2 倍之间。
	
**Note：**此博文仅编译代码用于查看系统代码。如需要设备调试或者需要修改代码，请参阅[郭霖的博客](https://blog.csdn.net/c10wtiybq1ye3/article/details/78591019)。

#### 8. 在AndroidStudio中查看源码
关于AndroidStuido的下载以及介绍，请移步 [Android Studio官方网站](https://developer.android.com/studio/)。

* 编译完成后，cd到源代码目录，如`/Volumes/android/source`，执行：

	```
	mmm development/tools/idegen/
	```
* idegen完成后，执行：

	```
	sh ./development/tools/idegen/idegen.sh
	```
* 以上两步完成以后，打开AndroidStudio -> File -> Open -> 源代码目录 -> android.ipr，即可在AndroidStuido中查看Android源代码。

## 写在后面
网上其实有很多编译源码的教程，但是大多都是比较老旧的版本，亲测下来最新的master分支已经解决了很多遗留的编译问题。

整个过程并没有很复杂，最难以应对的就是mac昂贵的硬盘空间了，256G的HD结结实实折腾了几次才最终编译完成。

遇到问题多看官方文档才是最正确的姿势。




