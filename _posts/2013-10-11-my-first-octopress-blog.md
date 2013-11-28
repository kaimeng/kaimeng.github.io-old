---
layout: post
title: "使用 Github 搭建博客"
description: "使用 Octopress 基于 Github 搭建个人博客系统"
tags: [技术, Jekyll, Octopress]
comments: true
share: true
modified: 2013-11-28
---

最近接触人工智能的开源项目 conceptNet 开始学习使用 github，然后发现竟然可以基于 github 搭建自己的博客， 因为一直在想有自己的独立博客，在网上搜了一些资料之后，果断开始折腾。

开始接触时用的是基于 Jekyll 的 Bootstrap，但原始的主题很丑，搞了半天换主题都没有成功。折腾中又发现了 octopress，装了试用一下，感觉比 Bootstrap 要简单些，而且原生的主题就很很不错了，换到 octopress 又开始折腾。

下面开始折腾。

# 准备工作

既然是基于gibhub,那么你得有github的账号，去github网站申请一个 github 的账号。
使用github搭建博客其实是基于 GitHub Pages 服务，详细参考[这里](https://help.github.com/categories/20/articles)。
新建一个名为  *username.github.io* 的仓库，这个库就是你以后博客系统的存放的位置。

1. 下载安装[Git](http://code.google.com/p/msysgit/downloads/list)
2. 下载安装[Ruby](https://www.ruby-lang.org/zh_cn/downloads/)
3. 配置Git，可以参考 [BeiYuu](http://beiyuu.com/) 的文章，[使用Github Pages建独立博客](http://beiyuu.com/github-pages/)

# 安装octopress
具体安装过程可以参考[官方教程](http://octopress.org/docs/setup/)，英文看着头疼的，可以参考[唐巧](http://blog.devtang.com)的文章，[象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)。这边写点我安装时出的问题。

打开 git bash,
克隆 octopress 到本地

	$ git clone git://github.com/imathis/octopress.git octopress
	$ cd octopress/

安装依赖

	$ gem install bundler
	$ bundle install

再安装 Octopress

	$ rake install

我在安装 rake install 时 出现如下的问题：

	$ rake install
	rake aborted!
	You have already activated rake 0.9.6, but your Gemfile requires rake 0.9.2.2. Using bundle exec may solve this.
	e:/360cloud/octopress/Rakefile:2:in `<top (required)>'
	(See full trace by running task with --trace)

解决方法：

	修改文件 octopress\Gemfile，其中的
	gem 'rake', '~> 0.9'
	修改为
	gem 'rake', '~> 0.9.6'
	重新安装
	$ rake install

设置 github_pages

	$ rake setup_github_pages

输入你的 github 建的 repository 地址,可以看到这个说明设置成功

	## Now you can deploy to https://github.com/username/username.github.io with `rakedeploy` ##

此时发现 octopress 目录变成 source 分支，其中多出 一个 _deploy 目录

	$ cd _deploy/

发现 octopress/_deploy (master)，_deploy 是master 分支，这生成的是最后部署的目录。

回到 octopress

	$ cd ..

# 测试与上传

测试一下看看效果

	$ rake preview

在浏览器中输入 `http://127.0.0.1:4000`  如能看到博客界面，恭喜你，已经成功部署了 Octopress 系统，按 Ctrl+C 退出。
	
接下来要发布到上面我们创建的 github 库中。

生成静态页面

	$ rake generate

发布到 master 分支

	$ rake deploy

发布到 source 分支

	$ git add .
	$ git commit -m "your message"
	$ git push origin source

第一次上传估计要过10分钟左右才能生效，打开 username.github.io，就可以看到上面显示的界面了。

# 写博客

	
现在我们来写一篇博客：

	$ rake new_post["my first octopress blog"]

进入 octopress\source\_posts, 发现新建了一个 markdown 文件，打开就可以开始写博客了。markdown 是一个轻量级的标记语言，非常适合用来写文章，具体语法见[这里](http://wowubuntu.com/markdown)。
windows 下建议使用 [MrakdownPad](http://markdownpad.com/)，非常好用的编辑器，可以实时显示效果。

编辑完成之后，就可以按照上面的步骤上传，记得别忘了 source 分支啊。 :-)

# 中文的支持

修改 Git\etc\profile 文件，在最后加上

	export LC_ALL=en_US.UTF-8
	export LANG=en_US.UTF-8

修改 Ruby200-x64\lib\ruby\gems\2.0.0\gems\jekyll-0.12.0\lib\jekyll\convertible.rb 文件

	将 
    self.content = File.read(File.join(base, name)) 
    修改为
    self.content = File.read(File.join(base, name), :encoding => "utf-8")

如果是 jekyll 1.3.0, 修改 ...\jekyll-1.3.0\lib\jekyll\convertible.rb 文件

    将
	self.content = File.read_with_options(File.join(base, name), merged_file_read_opts(opts))
	修改为
	self.content = File.read_with_options(File.join(base, name), :encoding => "utf-8")
	
     
修改 Ruby200-x64\lib\ruby\gems\2.0.0\gems\jekyll-0.12.0\lib\jekyll\tags\include.rb 文件

	将
    source = File.read(@file)
    修改为
    source = File.read(@file, :encoding => "utf-8")

同样如果是 jekyll 1.3.0 如上类似修改。

# 主题
[这里](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)有很多第三方主题，挑自己喜欢的，按照各个主题的说明进行安装。

# 在新电脑上写博客
首先安装Git和Ruby,并根据前面的介绍进行配置。
将github上你博客项目的 source 分支clone到本地

	$ git clone -b source https://github.com/username/username.github.io octopress
clone master分支到 _deploy
	
	$ cd octopress
    $ git clone https://github.com/username/username.github.io _deploy 
安装配置
	
	$ gem install bundler
	$ bundle install
	$ rake setup_github_pages
这样就可以在这台电脑上进行写作了。

如果在不止一台电脑上进行写作，开始之前须进行本地的更新，
	
更新 source 分支

	$ cd octopress
	$ git pull origin source
更新 master 分支

	$ cd ./_deploy
	$ git pull origin master  # update the local master branch

# 设置评论
octopress 自带的是 Disqus 的评论系统，但国内的速度有点慢，并且也不支持国内的社交账号登录，这里使用国内的[多说](http://duoshuo.com/)。可以参考 Haveee 的[为 Octopress 添加多说评论系统](http://havee.me/internet/2013-02/add-duoshuo-commemt-system-into-octopress.html)

在多说上注册账号，点击 *我要安装*，进行相应的设置，记录你的多说域名 `duoshuo_name`。
在 `_config.yml` 文件中加入如下配置：

	# Duoshuo comments
	duoshuo_name: yourname
	duoshuo_show_comment_count: true

在 `source/_layouts/post.html` 文件中 disqus 的代码下方添加如下代码

{% highlight html %}
{% raw %}

{% if site.duoshuo_name and page.comments == true %}
  <section id="comment">
    <h1>发表评论</h1>
	{% include post/duoshuo.html %}
  </section>
{% endif %}
{% endraw %}
{% endhighlight %}

在 `source\_includes\post\` 文件夹中添加 `duoshuo.html` 文件，文件代码

{% highlight html %}
{% raw %}

{% if site.duoshuo_name %}
<!-- Duoshuo Comment BEGIN -->
  <div class="ds-thread"></div>
  <script type="text/javascript">
  var duoshuoQuery = {short_name:"{{ site.duoshuo_name }}"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = 'http://static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		|| document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- Duoshuo Comment END -->
{% endif %}
{% endraw %}
{% endhighlight %}

# 其它设置
因国内不能登录 twitter, 注释掉 `_config.yml` 中有关 twitter 中的内容，可以加快加载速度，同样注释掉 `source\_includes\custom\head.html` 中的 Google 字体。

## 另
后来发现使用原生的 Jekyll 有很多非常漂亮的[网站](https://github.com/mojombo/jekyll/wiki/sites)，而且并不比 Octopress 复杂，反而觉得要简单一些，便参考 [HPSTR](http://mademistakes.com/articles/hpstr-jekyll-theme) 主题做了现在的页面。

