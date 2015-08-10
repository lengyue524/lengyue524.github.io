---
layout: post
title: Unity3d:Surface Shader中使用Properties（三）
date: 2014-12-10 15:09
author: lengyue524
comments: true
categories: [shader, Unity3d]
---
在Surface Shader中我们需要将Properties与Shader代码进行绑定，这样在Material的Inspector标签中修改属性值才会对Shader产生影响。

下面的步骤告诉我们在Surfaces Shader中如何使用Properties：



<ol>
    <li><span style="line-height: 1.5;"><span style="line-height: 1.5;">打开之前创建的Shader文件，删除下面的代码</span></span>
<pre class="brush:cpp">sampler2D _MainTex;
half4 c = tex2D (_MainTex, IN.uv_MainTex);</pre>
</li>
    <li><span style="line-height: 1.5;">在</span><span style="line-height: 1.5;"><span style="line-height: 1.5;">CGPROGRAM行下面添加下面的代码：</span></span>
<pre class="prettyprint">float4 _EmissiveColor;
float4 _AmbientColor;
float _MySliderValue;</pre>
</li>
    <li><span style="line-height: 1.5;"><span style="line-height: 1.5;">上面两步完成后我们就能在Shader中使用我们的属性了。我们将_EmissiveColor和_AmbientColor相加然后做_MySlideValue次幂运算，然后将值赋值给o。我们在surf函数中加入如下代码：</span></span>
<pre class="prettyprint lang-js">void surf (Input IN, inout SurfaceOutput o)
{
    float4 c =  pow(_EmissiveColor + _AmbientColor,  _MySliderValue);
    o.Albedo = c.rgb;
    o.Alpha = c.a;
}</pre>
</li>
    <li><span style="line-height: 1.5;"><span style="line-height: 1.5;">最后我们的代码应该如下所示。保存代码后，Unity会自动编译。确保你的代码正确，如果你已经将Materail指定到Object上，你会发现改变Inspector标签中的属性值，Object的颜色会发生变化。是不是很有意思！</span></span>
<pre class="prettyprint">Shader "Imakiba/BasicDiffuse" {
    Properties {
        _EmissiveColor ("Emissive Color", Color) = (1,1,1,1)
        _AmbientColor ("Ambient Color", Color) = (1,1,1,1)
        _MySliderValue ("This is a Slider", Range(1,10)) = 2.5
    }
    SubShader {
        Tags { "RenderType"="Opaque" }
        LOD 200
     
        CGPROGRAM
        #pragma surface surf Lambert

        float4 _EmissiveColor;
        float4 _AmbientColor;
        float _MySliderValue;
        struct Input {
            float2 uv_MainTex;
        };

        void surf (Input IN, inout SurfaceOutput o) {
            float4 c = pow(_EmissiveColor+_AmbientColor,_MySliderValue);
            o.Albedo = c.rgb;
            o.Alpha = c.a;
        }
        ENDCG
    } 
    FallBack "Diffuse"
}
</pre>
<img title="" src="http://7xky0m.com1.z0.glb.clouddn.com/20141210150104_36948.png" alt="" width="900" height="370" align="" /></li>
</ol>
上面的代码中，我们使用到了pow(x,y)这个方法，这是一个内建函数运算的结果为x<sup>y</sup>。

想知道pow()方法的更多信息，可查看Cg文档，里面提供了各种资源来学习Shader还有API来学习Cg Shading 语言。

<a href="http://http.developer.nvidia.com/CgTutorial/cg_tutorial_appendix_e.html" target="_blank">http://http.developer.nvidia.com/CgTutorial/cg\_tutorial\_appendix\_e.html</a>

<sup>
</sup>