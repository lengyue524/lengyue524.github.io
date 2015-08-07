---
layout: post
title: wordpress去掉img外部的p标签
date: 2014-12-10 16:26
author: lengyue524
comments: true
categories: [img, p, wordpress, wordpress]
---
<p>
	当文章中插入图片后，wordpress会自动在img标签外部使用p标签作为父标签。
</p>
<p>
	如果p标签在css中添加下面的属性会导致img向右移动了两个文字单位而超过文章边界。
</p>
<p>
	<br />
</p>
<pre class="prettyprint lang-css">text-indent: 2em;</pre>
我们需要让wordpress去掉这个p标签。在外观 | 编辑中，找到functions.php，添加如下代码
<pre class="prettyprint lang-js">function filter_ptags_on_images($content){
    return preg_replace('<span style="line-height:1.5;">/&lt;p&gt;\\s*?(&lt;a .*?&gt;&lt;img.*?&gt;&lt;\\/a&gt;|&lt;img.*?&gt;)?\\s*&lt;\\/p&gt;/s</span><span style="line-height:1.5;">', '\1\2\3', $content);</span> }
add_filter('the_content', 'filter_ptags_on_images');</pre>
<p>
	<br />
</p>
