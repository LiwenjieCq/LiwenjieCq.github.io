---
layout: post
title: "Jekyll+GitHub Pages搭建博客"
date: 2017-03-25
categories: jekyll
tags: jekyll
---

* content
{:toc}

学得了一些编程知识之后就着手搭建一个自己博客，但不知道从哪里入手，无意中发现jekyll+github搭建博客很方便使用，于是就找了一个[教程](http://www.jianshu.com/p/8f843034c7ec)开始搭建，教程写的很详细。
就把我搭建的过程记录一下，也是我的第一篇博客，留个纪念。




   [GitHub](https://github.com)是一个代码托管的网站，上面有许多开源的优秀项目  
   [Jekyll](jekyllrb.com)是一个简单的，为博客设计的静态网站生成器
## 注册github账号
注册账号
![login](/my_picture/2017-02-19-create-my-bolg-jekyll/Login.png)

![picture](/my_picture/2017-02-19-create-my-bolg-jekyll/Step2.png)  
建立仓库
![picture](/my_picture/2017-02-19-create-my-bolg-jekyll/NewRepo.png)  

![picture](/my_picture/2017-02-19-create-my-bolg-jekyll/CreateRepo.png)
- 我的用户名是LiwenjieCq,那么名字取为[LiwenjieCq.git.io](https://github.com/LiwenjieCq)(**十分重要**)  
- GitHub认为一个账号对应一个用户或一个组织，GitHub会分配给这个用户一个域名:username.github.io,访问这个域名时GitHub就会去解析username.github.io的master分支  
- 如果不想把网页放在usernaem.github.io项目下管理，比如我想建一个名叫blog的项目去管理我的博客，
就需要在blog下建一个名叫gh-pages的分支，因为GitHub规定在除username.github.io以外的其他项目时只有在gh-pages分支，
才会生成网页文件，此时的域名就会变成:username.github.io/blog
到此就成功了可以建一个index.heml，push到对应分支看看效果
![picture](/my_picture/2017-02-19-create-my-bolg-jekyll/Settings.png)  

## 安装jekyll  
- 在windows下安装jekyll[安装教程](http://blog.csdn.net/rainloving/article/details/45745491)
- 安装好后可以看看[阮一峰的教程](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)  
- jekyll装好就可以使用本地调试了，如果没模板可用，可以去这里看看[jekyll模板](http://jekyllthemes.org/) 
- jekyll下有几个重要的文件夹(由于不清楚这些目录结构，改模板的时候也用了不少时间)



	| ------------- |-------------:|
	| _config.yml | 网站的主要配置文件，保存配置数据。很多配置选项 |
	| _drafts | 草稿文件 | 
	| _includes      | 被模板包含的界面，在所有布局都可以用到的界面，如用标签 {-% include tag.html %-}(符号"-"避免md的标记冲突)来把文件_includes/tag.html包含起来 |
	| _posts     | 存放博客文章(命名格式YEAR-MONTH-DAY-title.md)      |
	| _layouts      | 存放文章的外部模板，网页的各种布局 |
	| index     | 网站的主页面      |
	| _site | 放置的是jekyll转换后的网页文件 |
	| CNAME | 用于记录你的自定义域名A。访问域名A会通过DNS 解析到github的服务器IP，不过你的http头上记录的域名还是A，github的服务器会根据该域名找到对应的仓库。      |
	



## 设置评论功能
- 评论用的插件是[友言](http://www.uyan.cc/),z在官网注册一个账号把对应的js代码拷贝到相应模板下即可  
- **本地测试会出错，必须在线上测试才有效果**



## 安装github客户端
- 最后在介绍一个工具[github客户端](https://desktop.github.com/)(需要翻墙)操作起来十分方便，安装好后有一个Git shell，由于第一次接触git在上面可以学习到许多命令  
- 编辑文章用的是[MarkdownPad2](http://markdownpad.com/)写文章十分高效，也很容易上手.*[参考](http://www.jianshu.com/p/q81RER)*
- 这里有个坑就是用Cmd Markdown编辑器写的文章jekyll不能解析，找了半天

**最后我所理解的搭建过程大概就是这样，也遇到过许多的问题，问题的答案网上多数都能找得到**  






