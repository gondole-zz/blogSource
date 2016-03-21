title: Hexo博客Next主题优化
date: 2016-01-22 17:16:30
tags: hexo
categories: hexo
---

目前使用NexT.Pisces主题，优化过程比较零碎，也比较多，在此整理一下。

# 菜单设置

NexT主题菜单设置，用于设置博客上方导航栏，在主题配置文件中修改。

```	java
menu:  
  home: /                       #主页  
  categories: /categories		#分类页（需手动创建）  
  #about: /about				#关于页面（需手动创建）  
  archives: /archives			#归档页  
  tags: /tags					#标签页（需手动创建）  
  #commonweal: /404.html        #公益 404 （需手动创建）
```
<!--more-->

## 标签页面

```	java
    hexo new page "tags"
```

## 分类页面

```	java
    hexo new page "categories"
```

# 分页插件 （针对站点）

在blog目录下的_config.yml配置文件末尾添加以下内容以设置分页参数

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

# 添加博客访问量统计

站点访问计数我使用的是不蒜子
使用非常方便，只需一行脚本+一行标签

## 显示站点总访问量

我们使用的是hexo，所以要找到网站的布局文件，不同的主题的布局文件可能不一样，下面教程是针对NexT主题做出的修改。

找到站点的themes/next/layout/_partials目录下的footer.swig文件
将以下脚本和标签插入到文件中

```	java
	<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
	
	本站总访问量 <span id="busuanzi_value_site_pv"></span> &nbsp&nbsp&nbsp
	您是第<span id="busuanzi_value_site_uv"></span>个来到的小伙伴
```

插入到这里

```	java
	<div class="powered-by">
		...
	  </a>
	</div>
	
	# 插入到这里
	
	{% block footer %}{% endblock %}
```

## 显示文章阅读次数

因为不蒜子只能统计文章详情页面的阅读次数，在主页面是没法显示的，所以后来换成了leanCloud来进行阅读次数的显示。
1. 注册leanCloud，获取到appId和appKey 写入到主题配置文件里。  
2. 因为Next主题已经集成了相关代码，所以直接部署就生效了。


# 多说评论

## 多说创建个人站点

在多说注册并且创建站点并且可以进行相关设置比如默认头像以及自定义CSS评论框样式

## 修改站点配置文件

在站点配置文件中增加duoshuo_shortname字段，duoshuo_shortname就是上一步中你填写的值。

	duoshuo_shortname: your_duoshuo_name

## 去除标签页评论

	title: categories
	date: 2015-09-18 21:46:42
	type: "categories"
	comments: false   #去除多说评论框

## 把多说评论依赖的embed.js放置底部

Yahoo性能中心总结的高性能网站设计的规则提及，把Javascript脚本尽量放到页面底部加载，这里不多说。
wordpress多说插件提供了在网页底部插入多说核心脚本embed.js这选项供用户选择，比较人性化。其他博客程序的话可以把embed.js放置到主题的footer底部加载。这里以hexo静态博客程序，NexT.Mist主题举个栗子，这里需要修改的文件是duoshuo.swig，路径是your-hexo-site\themes\next\layout\_scripts目录下，将下面一段代码

	(document.getElementsByTagName('head')[0]

修改成下面的代码

	(document.getElementById('footer')

# 添加RSS

添加feed插件，一定记得加--save

	npm install hexo-generator-feed --save

插件安装完成后 在站点配置文件_config.yml 里添加  

	plugins:  
	- hexo-generator-feed

在主题配置文件里添加  

	rss: /atom.xml

然后在执行 hexo g时就可以自动生成atom.xml文件了。

# 添加 Swiftype 搜索

按照next的github上的教程配置了半天没生效...， 后来网上搜索到了新版本Swiftype Search的配置方法，注册 Swiftype 后 在安装配置时，复制你的code  

	_st('install','xxxxxxxxxxxxx','2.0.0');

其中 **xxxxxxxxxxxxx** 为你的swiftypeKey，然后在站点配置文件_config.yml中添加：

	swiftype_key: xxxxxxxxxxxxx

另外需要注意的是，在进入搜索框 **(search field)** 一项时，主意将搜索框的ID改成NexT主题搜索框的ID **#st-search-input**，TYPE修改为 **elementID**，最后进入(**activate**)这一项，点击右下角的**ACTIVATE SWIFTYPE**按钮即可完成swiftype的所有配置了。

# gulp插件压缩

### 安装gulp相关插件

	npm install gulp -g  
	npm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean --save

### 执行 gulp 任务

安装完 gulp 插件后，需要在 package.json 同级目录下，新建 gulpfile.js。  

文件保存后，当我们执行 hexo g && gulp 时，gulp 会根据 gulpfile.js 中的配置，对 public 目录中的静态资源文件进行压缩。压缩完成后执行 hexo d 部署到 github / vps 中即可。


    