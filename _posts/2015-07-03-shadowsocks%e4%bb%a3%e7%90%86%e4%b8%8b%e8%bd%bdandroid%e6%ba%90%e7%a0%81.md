---
layout: post
title: shadowsocks代理下载repo,清华镜像下载Android源码
date: 2015-07-03 18:04
author: lengyue524
comments: true
categories: [android]
---
<h4>由于GFW的封锁，下载android源码很麻烦，这里记录下翻墙下载源码的方法。</h4>

<p>我这里使用的代理工具是shadowsocks，在ubuntu的安装方法我就不介绍了。 我这里安装的是shadowsocks-qt5，客户端可以连接服务端即可，不用配置自动代理或全局代理。 注意，我设置的本地客户端地址为默认127.0.0.0端口为1080。</p>

<hr />

<h4>下载repo</h4>

<p>因为repo支持代理下载，我们只需要在官方的命令上加上代理就可以正常下载了。</p>

<p><code>curl --socks5 127.0.0.1:1080 https://storage.googleapis.com/git-repo-downloads/repo &gt; ~/bin/repo</code></p>

<hr />

<h4>下载源码</h4>

<p>清华镜像地址：<code>git://aosp.tuna.tsinghua.edu.cn/android/</code></p>

<p>repo其实就是python的脚本文件，我们需要修改文件第5行，替换为</p>

<p><code>REPO_URL = 'git://aosp.tuna.tsinghua.edu.cn/android/git-repo'</code></p>

<p>然后执行</p>

<p><code>repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest</code></p>

<p>完毕后执行<code>repo sync</code>即可</p>

<p>其他操作，比如下载特定版本号，只需要将 https://android.googlesource.com/ 全部使用 git://aosp.tuna.tsinghua.edu.cn/android/ 代替即可。</p>
