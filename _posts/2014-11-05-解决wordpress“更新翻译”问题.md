---
layout: post
title: 解决wordpress“更新翻译”问题
date: 2014-11-05 14:28
author: lengyue524
comments: true
categories: [php, wordpress]
---
<p>
    打开php.ini文件
</p>
<pre class="brush:bash;">vi /usr/local/php/etc/php.ini</pre>
<p>
    按/键输入<span style="color:#373737;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:15px;line-height:24px;">scandir搜索此函数名，</span><span><span style="font-size:15px;line-height:24px;">或找到</span></span>disable_functions这一行。
</p>
<p>
    <span><span style="font-size:15px;line-height:24px;">删除</span></span><span style="color:#373737;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:15px;line-height:24px;">scandir函数。保存关闭。</span> 
</p>
<p>
    <span><span style="font-size:15px;line-height:24px;">重新读取php.ini文件</span></span> 
</p>
<pre class="brush:bash;">service php-fpm reload</pre>
<p>
    &nbsp;
</p>