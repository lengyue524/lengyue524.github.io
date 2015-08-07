---
layout: post
title: Unity3d:创建一个简单的Surface Shader（一）
date: 2014-12-09 11:36
categories: [Unity3d,Shader]
tags: [Unity3d,Shader]
comments: true
---
3D图形中的Shader总是那么神秘的东西。初学Unity3d的朋友看到游戏中各种光影效果，却又不知道怎样才能做出那样的光影效果，我们以一个简单的Surface Shader创建来进入Shader的世界。

在Unity editor中Project标签下,右键点击Assets文件夹选择`Create | Folder`。

右键创建的文件夹选择Rename或点击F2重命名文件夹为Shaders。

同样的方法创建文件夹，命名为Materials。

右键Shaders文件夹选择`Create | Shader`，右键Materials文件夹选择`Create | Material`。

重命名上面创建的两个文件为BasicDiffuse。

双击Shader文件夹中的BasicDiffuse文件使用MonoDeveloper打开，你就能看到Shader代码。

现在我们为我们的Shader指定一个自定义文件夹。

代码的第一行是Shader的自定义描述

通过这个描述，Unity可以使它在Material中的Shader下拉列表中看到，我们在这里将其修改为`Shader "Imakiba/BasicDiffuse"`。当然你可以在人任何时候命名为你想要的名字。保存文件，Unity会自动加载修改过的Shader。修改后你的Shader文件应该是下面的样子：

{% highlight python %}
Shader "Imakiba/BasicDiffuse" {
    Properties {
        _MainTex ("Base (RGB)", 2D) = "white" {}
    }
    SubShader {
        Tags { "RenderType"="Opaque" }
        LOD 200

        CGPROGRAM
        #pragma surface surf Lambert

        sampler2D _MainTex;

        struct Input {
            float2 uv_MainTex;
        };

        void surf (Input IN, inout SurfaceOutput o) {
            half4 c = tex2D (_MainTex, IN.uv_MainTex);
            o.Albedo = c.rgb;
            o.Alpha = c.a;
        }
        ENDCG
        } 
    FallBack "Diffuse"
}
{% endhighlight %}

回到Unity Editor 选择我们在第4步创建的Material在Inspector标签栏中Shader下拉列表中选择`Imakiba | BasicDiffuse`（如果你修改了第7步中的命名，请按你命名的名称来选择）。在此你为Material指定了一个Shader，然后你可以把这个Material指定给一个Object。

到这里我们就创建了我们的第一个自己的Shader，但是按照这里的步骤将Shader指定到Object发现我们的Object并没有发生什么变化。这是因为Shader是Unity的基本属性，我们并没有修改里面的代码。我们看到下面还有好多行的代码没有解释呢，下一节我将介绍下下面的代码。