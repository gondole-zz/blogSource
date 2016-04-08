title: Hexo搭建GitHub博客
date: 2016-01-21 16:29:12
tags: hexo
categories: hexo
---
![](http://7xqdqt.com1.z0.glb.clouddn.com/2016%2F03%2F24%2Fhexo.png)

之前在csdn有一个博客，不过也没有写起来，只是当作一个在线笔记本来使用，记录收藏一些开发相关的知识。大概一年前看到一个网站，感觉网站样式很清爽简洁，看到下方有hexo强力驱动时才发现hexo这个玩意，但是放下没去了解，这两天心血来潮决定好好整整，简单按照网上的教程一步一步搭建了这个githubPage，这里也大概记录一下步骤吧，毕竟脑袋不如烂笔头好使......

<!--more-->

# 环境准备

- Node 安装: 在Node.js官网下载相应平台的最新版本，一路安装即可。

- Git 安装: 可以选用msysgit，谷歌搜索至git官网，下载安装，设置环境变量以便全局使用, GIT_HOME, %GIT_HOME%\bin。

- Sublime:支持MarkDown语法的强大编辑器，支持很多编程语言，支持语法高亮等很多特性。

# GitHub

- 首先需要一个帐号
- 建立一个与你用户名响应的仓库，仓库名为 your_user_name.github.com
- 添加SSH公钥到Account settings -> SSH Keys -> Add SSH Key

如何添加SSH Key ?

首先设置用户名和密码：  

	git config --global user.email "你的github帐号"
	git config --global user.name "你的github用户名"  

生成密钥：  

	ssh-keygen -t rsa -C "github用户名"  

默认会在c盘.ssh文件夹下生成两个文件 id_rsa和id_rsa.pub，打开id_rsa.pub，拷贝其中的内容添加到Add SSH Key。

最后验证：

	ssh -T git@github.com

验证时出现了一个问题：连接超时，22端口被禁用，于是更换ssh_config文件内容为：

	Host github.com  
	Hostname ssh.github.com  
	Port 443


# Hexo安装

此时Node和Git都已经安装完毕，ssh也已经关联上，执行如下命令安装hexo：

	npm install -g hexo

## 初始化

可以新建一个文件夹 eg：hexo，在此文件夹下执行命令：

	hexo init

将hexo初始化到该目录，到此时，安装工作执行完毕。

## Hexo使用

## 生成静态页面

cd到init初始化目录，执行如下命令，生成静态页面到hexo\public目录。
	 
	hexo generate

或则缩写
	
	hexo g

## 写文章

	hexo new "postName" #新建文章

此时会在source下_posts文件夹下生成postName.md文件，打开即可进行编辑。

## 部署

	hexo deploy

或则缩写

	hexo d

有时会提示警告：ERROR Deployer not found: git
执行如下命令就可以安装deploy
   
	npm install hexo-deployer-git --save

此时打开username.github.com 即可预览你的站点啦。


# 主题安装

	git clone https://github.com/iissnan/hexo-theme-next.git themes/next

主题会克隆到themes下，打开hexo下的_config.yml，修改主题为next即可。
	
	theme: next

更新主题
	
	cd themes/next
	git pull


# 分页插件

在blog目录下的_config.yml站点配置文件末尾添加以下内容以设置分页参数

```	java
	# Plugins
	index_generator:
	  per_page: 10 ##首页默认10篇文章标题，如果值为0不分页
	
	archive_generator:
	  per_page: 10 ##归档页面默认10篇文章标题，如果值为0不分页
	  yearly: true ##生成年视图
	  monthly: true ##生成月视图
	
	tag_generator:
	  per_page: 10 ##标签页面默认10篇文章，如果值为0不分页
	
	category_generator: 
	  per_page: 10 ##分类页面默认10篇文章，如果值为0不分页
```


# 添加RSS

添加feed插件，一定记得加--save

	npm install hexo-generator-feed --save

插件安装完成后 在站点配置文件_config.yml 里添加  

	plugins:  
		- hexo-generator-feed

在主题配置文件里添加  

	rss: /atom.xml

然后在执行 hexo g时就可以自动生成atom.xml文件了。


# 添加sitemap

添加sitemap插件

	npm install hexo-generator-sitemap

开启sitemap功能：编辑hexo/_config.yml，添加如下代码：

	plugins:
		- hexo-generator-sitemap
		
访问username/sitemap.xml即可看到站点地图。不过，sitemap的初衷是给搜索引擎看的，为了提高搜索引擎对自己站点的收录效果，我们最好手动到google和百度等搜索引擎提交sitemap.xml。


# gulp插件压缩

部署之前压缩生成的html文件，可以提高网页的加载速度。

## 安装gulp相关插件

	npm install gulp -g  
	npm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean --save

## 执行 gulp 任务

安装完 gulp 插件后，需要在 package.json 同级目录下，新建 gulpfile.js。  

文件保存后，当我们执行 hexo g && gulp 时，gulp 会根据 gulpfile.js 中的配置，对 public 目录中的静态资源文件进行压缩。压缩完成后执行 hexo d 部署到 github / vps 中即可。