---
layout: post
title: Django_Python3.5+安装mysql依赖
categories: [Django]
tags: [Django,Python3]
comments: true
description: Django+Python3.5+使用mysql比较复杂，记录一下。
---
由于Python2.7的mysql模块与3.5+版本不兼容，所以3.5+版本需要安装对应3.5版本的mysql模块。

本教程假设你已安装Django与Python3.5+版本，理论上更高版本也适用,Python-dev库可能需要对应的
修改，比如使用Python3.6 下面命令中就要改为python3.6-dev。

安装python3.5-dev

{% highlight shell %}
sudo apt-get install python3.5-dev
{% endhighlight %}


安装mysql客户端模块

mysqlclient支持mysql4.1-5.5版本支持python2.7，3.3-3.5版本

{% highlight shell %}
sudo pip3 install mysqlclient
{% endhighlight %}
