title: Hexo博客的多设备同步
date: 2016-01-22 14:32:35
tags: hexo
categories: hexo
---
![](http://7xqdqt.com1.z0.glb.clouddn.com/2016%2F01%2F27%2F24hrPQn.jpg)

在公司搭建了两天hexo博客，主题各种配置，发了几篇文章，回到家里想继续搞时发现搞不了...， GitHub上面存放的是hexo根据md生成的网页文件，于是搜索了一下怎么样在多设备上操作hexo博客，在公司弄完回家接着弄啊得。今天看到一种方法，试验了下，大概走通了，记录一下。

博客的源文件主要在source文件夹里，所以需要把这个文件夹同步到云端，这里选择使用GitHub仓库来备份source文件夹。

<!--more-->

# 备份 hexo/source 

在你的Github里新建一个仓库，eg：hexoSource，创建完毕后在电脑上切换到你的hexo的source目录，执行如下命令进行上传：

```	java
    git init	#初始化同步目录
	git remote add origin git@github.com:username/hexoSource.git
	git add .
	git commit -m "init hexo source "
	git pull origin master		#同步远程仓库文件
	git push -u origin master	#push到远程仓库
```

这样本地的source文件夹就备份到github上了，另外hexo下的_config.yml和theme下的主题（可能自定义配置的比较多）可以备份到网盘上面，在另外一台电脑上初始化好hexo之后可以直接覆盖。

# 恢复 hexo/source

在另外一台电脑上，首选按照搭建hexo博客的正常流程来进行各种配置：

1. 安装 Node.js
2. 安装 Git
3. 安装 Hexo

最后执行完hexo初始化命令后，本地hexo文件夹下会有正常的source文件，此时切换到该目录，进行github上source备份文件的关联恢复，clone远程source仓库文件到本地

```	java
    git clone username@host:/path/to/repository
```

clone完之后备份的远程source文件就到本地了，并且和git做了关联，接着就可以正常编辑博客文件了以及正常部署，记得下班时同步本地source文件到github上喔：

```	java
    git pull origin master
```
