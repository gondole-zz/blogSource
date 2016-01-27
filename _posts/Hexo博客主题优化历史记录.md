title: Hexo博客主题优化历史记录
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

## 标签页面

```	java
    hexo new page "tags"
```

## 分类页面

```	java
    hexo new page "categories"
```
<!--more-->

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

## 显示单页面访问量

```	java
	......待编辑
```

# 多说评论
## 在多说注册并且创建站点
## 修改站点配置文件

在站点配置文件中增加duoshuo_shortname字段，duoshuo_shortname就是上一步中你填写的值。

```	java
    duoshuo_shortname: gondole
```

## 去除分类页标签页多说评论

```	java
	title: categories
	date: 2015-09-18 21:46:42
	type: "categories"
	comments: false   #去除多说评论框
```



