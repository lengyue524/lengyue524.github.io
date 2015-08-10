---
layout: post
title: Unity3d:让它更亮一点，Half Lambert光照模型（五）
date: 2014-12-30 11:46
author: lengyue524
comments: true
categories: [shader, Unity3d]
---
<p>Half Lambert通过光照值的计算来让背光部分更加明亮一些。</p>

<p>这个技术最初是在《半条命》游戏中使用的，它可以防止低光部分过暗而看起来一片黑的情况。</p>

<p>要使用Half Lambert我们只需改变一小部分代码:</p>

<pre><code>inline float4 LightingHalfLambert(SurfaceOutput s,fixed3 lightDir,fixed atten){
    float difLight = max(0,dot(s.Normal,lightDir));
    float halfLambert = difLight*0.5+0.5;
    float4 col;
    col.rgb = s.Albedo * _LightColor0.rgb * (halfLambert * atten * 2);
    col.a = s.Alpha;
    return col;
}
</code></pre>

<p>下面是漫反射和Half Lambert的视觉区别，可以看到Lambert暗部明显黑，并且颜色没有渐变，显的很平。Half Lambert暗步比较明亮，并且渐变明显。</p>

<p><img src="http://7xky0m.com1.z0.glb.clouddn.com/3D9D1156-5153-484F-866A-DBF18D1331EB-300x166.jpg" alt="Lambert和Half Lambert的视觉区别" /></p>
