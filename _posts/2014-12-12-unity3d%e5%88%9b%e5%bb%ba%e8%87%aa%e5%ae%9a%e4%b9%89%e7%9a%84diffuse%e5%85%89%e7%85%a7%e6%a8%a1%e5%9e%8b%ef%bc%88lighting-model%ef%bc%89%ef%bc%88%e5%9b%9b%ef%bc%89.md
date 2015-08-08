---
layout: post
title: Unity3d:创建自定义的Diffuse光照模型（lighting model）（四）
date: 2014-12-12 16:21
author: lengyue524
comments: true
categories: [shader,unity]
---
<p>
	前面创建的Shader都是使用的Unity内建的光照函数，虽然能够满足大多数项目的需要，但是还是会遇到内建的光照模型无法满足需要的效果的时候。这时候就需要自定义光照模型的了。使用自定义的光照模型，我们可以控制边界光，Cubemap的光照，甚至Shader对外界环境的变化。
</p>


<p>
	这一节我们使用之前创建的光照模型，创建一点不同的效果。
</p>
<ol>
	<li>
		<span style="line-height:1.5;">修改</span><span style="line-height:1.5;">#pragma语句
<pre class="prettyprint">#pragma surface surf BasicDiffuse</pre>
</span> 
	</li>
	<li>
		<span style="line-height:1.5;">subshader中添加下面的语句
<pre class="prettyprint">inline float4 LightingBasicDiffuse(SurfaceOutput s,fixed3 lightDir,fixed4 atten){
	float difLight = max(0,dot(s.Normal,lightDir));
	float4 col;
	col.rgb = s.Albedo * _LightColor0.rgb * (difLight * atten * 2);
	col.a = s.Alpha;
	return col;
}</pre>
</span> 
	</li>
	<li>
		<span style="line-height:1.5;">保存Shader,回到Unity Editor，你会发现并没有什么变化。我们在这里做的只是去掉了与Unity内建光照的关联，创建了我们自己的光照模型。<br />
</span> 
	</li>
</ol>
<hr />
我们来看看它是怎么工作的：

<ul>
	<li>
		<span style="line-height:1.5;"></span><span style="line-height:1.5;">#pragma surface告诉我们Shader使用的哪个光照模型。在我们创建的时候，Unity给我们默认指定为Lambert光照模型。我们现在告诉Shader去寻找我们自定义的BaseDiffuse光照模型。</span> 
	</li>
	<li>
		<span style="line-height:1.5;">创建我们自定义的光照模型只需要声明一个新的光照模型方法，你只需要修改方法名就可以对应相应的光照模型.例如:LightingName是由Lighting&lt;your name&gt;变换过来的。你可以使用下面3个光照模型函数：</span> 
	</li>
	<ul>
		<li>
			<span style="line-height:1.5;"></span><span style="line-height:1.5;"></span><span style="line-height:1.5;">half4 LightingName (SurfaceOutput s, half3 lightDir, half atten){}
			<p>
				这个方法用来正向渲染而不需要取景方向
			</p>
</span> 
		</li>
		<li>
			<span style="line-height:1.5;">half4 LightingName (SurfaceOutput s, half3 lightDir, half3 viewDir, half atten){}
			<p>
				这个方法用来正向渲染，使用取景方向
			</p>
</span> 
		</li>
		<li>
			<span style="line-height:1.5;"><span style="line-height:1.5;">half4 LightingName_PrePass (SurfaceOutput s, half4 light){}</span><br />
这个方法用来延迟渲染</span> 
		</li>
	</ul>
	<li>
		dot是Cg语言的内建方法，你可以用它来比较两个向量的方向。返回值为-1到1。值为-1表示向量平行，方向相反；1表示向量平行，方向相同；0表示向量互相垂直。
		<p>
			dot返回的是两个向量夹角的计量值,值越大表面接受到的入射光越强。更多信息可参考：<a href="http://http.developer.nvidia.com/CgTutorial/cg_tutorial_chapter05.html" target="_blank">http://http.developer.nvidia.com/CgTutorial/cg_tutorial_chapter05.html</a>
		</p>
	</li>
	<li>
		要完成光的扩散计算我们要乘以Unity提供的数据和SurfaceOutput结构的数据。我们要s.Albedo(这个只有surf方法提供)乘以_LightColor0.rgb(Unity内部变量)然后乘以(difLight * attend),然后返回col值。
	</li>
</ul>
