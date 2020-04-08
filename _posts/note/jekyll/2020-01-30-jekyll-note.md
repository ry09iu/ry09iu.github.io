---
layout: article
title: 'Jekyll 静态博客搭建踩坑记录'
date: 2020-1-30 15:15
key: macos_20200130_1
category: 
- note 
- jekyll
tags:
- 学习笔记
- Jekyll
---

### 前言
&emsp;&emsp;博客是之前就搭建好的，因为某些原因我渐渐懒得写博文，以至于工作后多年来并没有太多写作上的积累，所以很多东西学了就忘了，有时候会做一些笔记，但因为做的太乱，没有做好总结，时间久了自己写的笔记也不太看得明白了。最近打算重新整理下博客，记录一些东西。

<!-- more -->

&emsp;&emsp;博客之前用的 Hexo 作为博客框架，部署在 Github Pages 和 Coding.net 静态页面，除了 Hexo 之外，还有很多优秀的静态框架，比如 Jekyll，Jekyll 和 Hexo 对于众多使用者们来说自然是褒贬不一，很多使用 Jekyll 的博主都纷纷转向 Hexo，而我，仅仅是因为一个主题，kitian616 的 [jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)，简约的页面设计风格，感人的详细文档，让我爱不释手。于是，我做了一个大胆的决定，打算动手将博客从 Hexo 迁移到 Jekyll。不得不说 Jekyll 的部署环境搭建起来确实挺繁琐，中间踩了不少坑，我打算将踩到的坑一一记录下来，方便以后查阅。所以，这是 Jekyll 踩坑系列的第一篇。

### 部署踩坑
首先，根据 kitian616 的文档，我选择直接 clone 项目来使用，此方法简单高效

`git clone https://github.com/kitian616/jekyll-TeXt-theme.git`

然后执行

`bundle exec jekyll serve`

想要实现本地预览，果然没那么顺利，开始报错了

`bash: bundle: command not found`

小问题，安装个 bundler 就可以啦

`sudo gem install bundler`

接下来继续执行`bundle exec jekyll serve`，报错

```shell
bundler: command not found: jekyll
Install missing gem executables with `bundle install`
```

网上查到原因，因为没有安装 Jekyll 相关组件，执行命令安装

`bundle install`

等待半晌毫无反应，继续查找解决方案，可通过修改 Gem 源来解决，默认源是 https://rubygems.org/，先移除再添加新源

```shell
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/  (这一步报错)
Error fetching http://ruby.taobao.org/:
	server did not return a valid file (http://ruby.taobao.org/specs.4.8.gz)
```

尝试更换新源地址为 https://ruby.taobao.org/

```shell
Error fetching https://ruby.taobao.org/:
	hostname "ruby.taobao.org" does not match the server certificate (https://ruby.taobao.org/specs.4.8.gz)
```

还是不行，找了一圈，看到有人讨论可以使用 https://gems.ruby-china.org/ ，点进去发现该源已经改地址了，新地址为 https://gems.ruby-china.com/，尝试执行

```shell
$ gem sources -a https://gems.ruby-china.com/
https://gems.ruby-china.com/ added to sources
```

看来是成功了，目前来看还算顺利，继续执行`bundle install`，还是卡住，有点无语，上网搜了之后才知道，通过上述方法修改的 Gem 源是全局的，而我在项目里执行`bundle install`，读取的源还是项目根目录 Gemfile 文件里指定的源，也就是默认的 https://rubygems.org/，果断改掉重新执行，这次终于开始安装了。但是没多久后又开始报错了

```shell
...
Installing eventmachine 1.2.7 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

current directory:
/private/var/folders/jt/47rj3mdd1d52gpfmcmy3p0n40000gn/T/bundler20200129-4535-xc9kppeventmachine-1.2.7/gems/eventmachine-1.2.7/ext
/System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/bin/ruby -r ./siteconf20200129-4535-voy65j.rb extconf.rb
mkmf.rb can't find header files for ruby at /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/lib/ruby/include/ruby.h

extconf failed, exit code 1

Gem files will remain installed in
/var/folders/jt/47rj3mdd1d52gpfmcmy3p0n40000gn/T/bundler20200129-4535-xc9kppeventmachine-1.2.7/gems/eventmachine-1.2.7 for inspection.
Results logged to
/var/folders/jt/47rj3mdd1d52gpfmcmy3p0n40000gn/T/bundler20200129-4535-xc9kppeventmachine-1.2.7/extensions/universal-darwin-18/2.3.0/eventmachine-1.2.7/gem_make.out

An error occurred while installing eventmachine (1.2.7), and Bundler cannot continue.
Make sure that `gem install eventmachine -v '1.2.7' --source 'https://gems.ruby-china.com/'` succeeds before bundling.

In Gemfile:
  jekyll-text-theme was resolved to 2.2.5, which depends on
    jekyll-feed was resolved to 0.13.0, which depends on
      jekyll was resolved to 3.8.6, which depends on
        em-websocket was resolved to 0.5.1, which depends on
          eventmachine
```

在网上找到解决方法，执行下面命令，但是报错

```shell
$ open /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg 
The file /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg does not exist.
```

ls 看了下确实没有这个文件，网上有人说需要升级 Ruby，尝试后仍然无效，最后终于找到解决方法

```shell
$ sudo rm -rf /Library/Developer/CommandLineTools
$ xcode-select --install
$ open /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg
```

继续执行`bundle install`，这次不再报错，顺利完成安装。最后重新执行

```shell
$ bundle exec jekyll serve
...
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts
                    done in 2.716 seconds.
 Auto-regeneration: enabled for '/Users/ry09iu/Documents/web-dev/博客/github/ry09iu.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

最后，在浏览器中打开 [http://127.0.0.1:4000](http://127.0.0.1:4000)，终于看到页面了。至此部署博客的踩坑阶段到这里就结束了，虽然中间有点小波折，但还是值得开心一下。

### 维护踩坑

博客部署完成后，当然是折腾配置啦，kitian616 的文档写的很详细，问题不大，基础工作完成后打算提交到 Github，用了那么久的 Git，居然在这踩到一坑，那就是在 git push 时报 403 错误

```shell
remote: Permission to xxxxx.git denied to xxx.
fatal: unable to access 'https://github.com/ xxxxx.git/': > The requested URL returned error: 403
```

解决方案为

```
修改项目中 .git/config 文件中的 url
将 url = https://github.com/git用户名/项目名称 改为
url = https://git用户名@github.com/git用户名/项目名称
```

成功提交后终于可以得到访问改造后的 [http://ry09iu.github.io](http://ry09iu.github.io) 了。

接下来是用 Markdown 写博文了，这里又遇到问题了，那就是修改 .md 文件后刷新页面无法实时更新，必须停止 jekyll 服务，重新启动才能看到修改后的页面。这个问题让我很头疼，因为每次修改 .md 后必须停止服务再启动，重新启动还会遇到 4000 端口已被占用的问题，这就需要手动查询进程 PID 后再使用 kill 命令杀掉，严重降低了工作效率。

网上找了很多资料，基本都是说执行`jekyll serve --watch`来解决，但是我尝试过仍然没有效果，又是尝试各种依赖组件版本，也是无济于事。最后终于找到 stackoverflow 上有人讨论此问题，原因是

```
jekyll serve --force_polling
You don't need --watch, because watching is the default for serve since 2.4.0.
```

果断执行`jekyll serve --force_polling`，终于成功解决问题，Happy，可以开始愉快的写博客了。