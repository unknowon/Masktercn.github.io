---
layout: post
title: Blog创建小记
date: 2019-04-10 20:43:23.000000000 +09:00
category: Blog Operations
---
首先放出我的Blog：[Maskter](https://blog.maskter.cn)
<hr/>

##创建步骤
* ==**申请**[Github Pages](https://pages.github.com)==

	网站首页就有创建教程，按部就班就好。
	
	完成以后会得到一个username.github.io的resp
	
	并且能够访问到hello world
	
	可以继续按照这个教程导入模版等。
	
	此时，blog已经有了雏形。
	
	如果不满足于此，那就继续。
	
	(这里先把这个resp下载下来或者clone下来，后面会用到)
	
* ==**安装配置jekyll**==

	我的blog是使用jekyll来写的，所以需要在自己电脑上配置jekyll。
	
	电脑是mac，以下是mac的配置方法。
	
	执行`sudo gem install jekyll`
	
	就能安装jekyll，但是在安装的时候踩了一个坑
	
	```
	ERROR: While executing gem ... (Gem::FilePermissionError)
	You don't have write permissions for the /usr/bin directory.
	```
	问了谷哥之后找到方法
	
	执行`sudo gem install -n /usr/local/bin jekyll`
	
	成功安装
	
	接下来进入到之前clone的resp的路径
	
	`cd /ideaProjects/githubBlog/Masktercn.github.io`
	
	执行`bundle install`
	
	如果出现提示
	
	```
	-bash: bundle: command not found
	```
	
	那就说明需要安装bundle，根据之前踩的坑
	
	执行`sudo gem install -n /usr/local/bin bundle`
	
	安装成功以后执行`bundle install`
	
	出现一堆输出，并且没看见错误就说明成功了。
	
	接下来 执行`bundle exec jekyll serve`
	
	把blog在本地跑起来
	
	出现
	
	```
	Configuration file: /Users/lulu/IdeaProjects/githubBlog/Masktercn.github.io/_config.yml
            Source: /Users/lulu/IdeaProjects/githubBlog/Masktercn.github.io
       Destination: /Users/lulu/IdeaProjects/githubBlog/Masktercn.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.593 seconds.
 Auto-regeneration: enabled for '/Users/lulu/IdeaProjects/githubBlog/Masktercn.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
	```
	就成功了，浏览器访问`http://127.0.0.1:4000/`就可以看到效果了
	
	修改过什么东西以后执行
	
	```
	git add --all
	git commit -m "commit information"
	git push -u origin master
	```
	提交到github
	
	搞定。
	
* ==**绑定自己的域名**==

	在根目录下新建文件`CNAME`
	
	在里面写上你的域名，比如我的`blog.maskter.cn`(注意，不要带协议 比如http)
	
	保存以后在你买域名的地方解析一下
	
	![blogCreateImg1](/assets/images/blogCreateImg1.png)
	
	解析完成以后就搞定啦
	
	写的文章放在`_posts`目录下，markdown编辑我用的是macdown。
