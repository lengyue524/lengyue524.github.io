---
layout: post
title: Unity3d:Surface Shader的Properties介绍（二）
date: 2014-12-09 16:43
author: lengyue524
comments: true
categories: [shader, unity]
description: Shader的Properties在Shader的渲染管道中是非常重要的，它是艺术家和用户指定纹理Texture和调整Shader值的手段。
---
<p style="text-indent:2em;">
	Shader的Properties在Shader的渲染管道中是非常重要的，它是艺术家和用户指定纹理Texture和调整Shader值的手段。
</p>
<p style="text-indent:2em;">
	Properties允许在Material的Inspector标签中显示GUI元素，而不用我们使用其他编辑器来修改，这提供给我们一个很直观的修改方法。
</p>


<p style="text-indent:2em;">
	在MonoDevelop中打开上一节的Shader，在第2-4行，我们称这里为Properties块，现在它又一个属性叫做_MainTex。如果你查看使用这个Shader的Material，你会注意到在Inspector标签有一个叫做Texture的GUI元素。这些代码就在为我们创建了这个GUI。
</p>
<p>
	<img src="http://7xky0m.com1.z0.glb.clouddn.com/20141209151046_54477.png" alt="" /> 
</p>
<p style="text-indent:2em;">
	<br />
</p>
<p style="text-indent:2em;">
	<br />
</p>
<hr />
下面我们在BaseDiffuse的Shader中创建Properties学习新的语法来看看它是怎么工作的：
<ol>
	<li>
		<p>
			在Properties块我们删除下面的代码
		</p>
		<p>
			<br />
		</p>
<pre class="prettyprint">_MainTex ("Base (RGB)", 2D) = "white" {}</pre>
		<p>
			<br />
		</p>
	</li>
	<li>
		<p>
			输入下面的代码，保存，然后进入Unity Editor
		</p>
		<p>
			<br />
		</p>
<pre class="prettyprint">_EmissiveColor ("Emissive Color", Color) = (1,1,1,1)</pre>
		<p>
			<br />
		</p>
	</li>
	<li>
		<p>
			当你回到Unity，Shader会被编译，在Material的Inspector标签可以看到一个color选区界面叫Emissive Color替换掉了之前的Texture，我们再添加一个看看会发生什么。
		</p>
		<p>
			<br />
		</p>
<pre class="prettyprint">_AmbientColor ("Ambient Color", Color) = (1,1,1,1)</pre>
		<p>
			<br />
		</p>
	</li>
	<li>
		<p>
			现在我们创建一个其他类型的属性，输入下面的代码
		</p>
		<p>
			<br />
		</p>
<pre class="prettyprint">_MySliderValue ("This is a Slider", Range(0,10)) = 2.5</pre>
		<p>
			<br />
		</p>
	</li>
	<li>
		<p>
			我们创建了新的GUI元素来与我们的Shader直观的交互，我们新建的Slider叫做This is a Slider，如下图所示
		</p>
		<p>
			<img src="http://7xky0m.com1.z0.glb.clouddn.com/20141209150635_97412.png" alt="" /> 
		</p>
		<p>
			<br />
		</p>
	</li>
</ol>
<p>
	<br />
</p>
<p style="text-indent:2em;">
	Properties允许你以直观的方式来调整Shader而不需要编写代码来改变Shader。
</p>
<p style="text-indent:2em;">
	<br />
</p>
<hr />
<p>
	<br />
</p>
<p style="text-indent:2em;">
	每一个Unity Shader都有一个内建结构来寻找它的代码。Properties是Unity预设的功能之一。下面给出这样做的原因：
</p>
<p style="text-indent:2em;">
	Shader程序员创建一个GUI元素直接与Shader代码进行绑定。Properties块中定义的属性可以用来改变Shader的值，颜色和纹理（Texture）。
</p>
<img src="http://7xky0m.com1.z0.glb.clouddn.com/20141209153903_28486.png" /> 
<p style="text-indent:2em;">
	<br />
</p>
<p style="text-indent:2em;">
	我们来看看它在后面到底做了些什么。
</p>
<p style="text-indent:2em;">
	当你添加一个属性时，你需要指定一个Variable Name。Variable Name是Shader代码用来从GUI元素中取值。这省了我们不少时间。
</p>
<p style="text-indent:2em;">
	属性的下一个元素是Inspector GUI Name和属性的类型（Type）。他们是成对出现的。<span>Inspector GUI Name是Material的Inspector中显示的名称，Type是用来控制数据的类型。在Unity Shader中有许多类型可定义。下面的表格介绍了在Shaders中使用的数据类型：</span> 
</p>
<p style="text-indent:2em;">
	<span> 
	<table style="width:100%;" cellpadding="2" cellspacing="0" align="left" border="1" bordercolor="#808080">
		<tbody>
			<tr>
				<td>
					Range (min, max)
				</td>
				<td>
					创建一个Float属性Slider值在min与max之间
				</td>
			</tr>
			<tr>
				<td>
					Color
				</td>
				<td>
					在Inspector标签中创建一个Color拾取器给定默认值=(float,float,float,float)
				</td>
			</tr>
			<tr>
				<td>
					2D
				</td>
				<td>
					创建一个纹理拾取器，允许用户拖拽纹理到Shader
				</td>
			</tr>
			<tr>
				<td>
					Rect
				</td>
				<td>
					创建一个非二次幂纹理(<span style="line-height:1.5;">non-power-of-2 texture&nbsp;</span><span style="line-height:1.5;">)拾取器，功能和2D GUI一样</span> 
				</td>
			</tr>
			<tr>
				<td>
					Cube
				</td>
				<td>
					创建一个立方体纹理(cube map eg:世界立方体纹理)拾取器，<span>允许用户拖立方体拽纹理到Shader</span><span></span><span></span><span></span><span></span> 
				</td>
			</tr>
			<tr>
				<td>
					Float
				</td>
				<td>
					创建一个Float属性但是没有Slider
				</td>
			</tr>
			<tr>
				<td>
					Vector
				</td>
				<td>
					创建一个4个Float属性，允许你创建方向或颜色
				</td>
			</tr>
		</tbody>
	</table>
<br />
<br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	<span><br />
</span> 
</p>
<p style="text-indent:2em;">
	最后，属性都可以设置默认值。在前面的例子中我们为_AmbientColor，类型为Color设置了一个(1,1,1,1)的默认值。这是一个颜色的值可以是RGBA或Float4或r,g,b,a = 1,1,1,1。当它被创建就被设置为白色。
</p>
<p style="text-indent:2em;">
	即使添加了属性在Unity Editor中编辑发现物体并没有变化，这是因为在Shader中没有使用到这些属性，下一节我们将讲解如何在Shader中使用属性
</p>
<p style="text-indent:2em;">
	关于属性的更多信息可以查看官方文档：
</p>
<div class="page">
	<div class="layoutArea">
		<div class="column">
			<p>
				<a href="http://docs.unity3d.com/Documentation/Components/SL-Properties.html" target="_blank">http://docs.unity3d.com/Documentation/Components/SL-Properties.html</a> 
			</p>
		</div>
	</div>
</div>
<p>
	<br />
</p>
