---
layout: post
title: Android的动画框架Property Animation
date: 2014-12-16 11:32
author: lengyue524
comments: true
categories: [android, Java, property Animation]
description: Android的View Animation使用的是tween动画与frame动画
---
<p style="text-indent:2em;">
    Android的View Animation使用的是tween动画与frame动画，关于View Animation</span>的使用可参考：
</p>
<p style="text-indent:2em;">
    <a href="http://www.ibm.com/developerworks/cn/opensource/os-cn-android-anmt1/index.html?ca=dat-" target="_blank">http://www.ibm.com/developerworks/cn/opensource/os-cn-android-anmt1/index.html?ca=dat-</a> 
</p>
<p style="text-indent:2em;">
    Property Animation的官方英文链接可参考：
</p>
<p style="text-indent:2em;">
    <a href="http://developer.android.com/guide/topics/graphics/prop-animation.html" target="_blank">http://developer.android.com/guide/topics/graphics/prop-animation.html</a> 
</p>


<p style="text-indent:2em;">
    <br />
</p>
<hr />
<h3>
    Property Animation与View Animation 的区别
</h3>
<p>
    <br />
</p>
<p style="text-indent:2em;">
    使用View Animation的开发者都知道，执行动画的类必须是View的子类，否则你必须自己写代码来实现。并且只提供了View对象的属性来执行动画，比如缩放，旋转，但是背景色却无法使用。
</p>
<p style="text-indent:2em;">
    另一个缺陷是View Animation系统只是修改了View被画的位置，而不是View本身。比如你让一个button在屏幕上移动。<span>button</span>正确的被绘制在屏幕上，但是<span>button</span>点击的地方并没有根据<span>button被绘制的地方而改变，你需要实现自己的逻辑来处理。</span> 
</p>
<p style="text-indent:2em;">
    <span>在Property Animation系统中，这些限制被完全移除了。你可以动画任何对象的任何属性，并且对象自身被确实修改了。<span>Property Animation系统提供了更好的方式来执行动画，在更复杂的情况，你定义了一个东来来改变你的属性，比如颜色，位置，尺寸并且能够定义动画的一部分，比如插值和多个动画的同步。</span></span> 
</p>
<p style="text-indent:2em;">
    然而View Animation系统执行的更快，并且写的代码更少。如果View Animation可以实现你的需求，你不需要使用Property Animation来代替它。根据不同的使用场景来使用这两个动画系统。
</p>
<p style="text-indent:2em;">
    Property Animation顾名思义为属性动画，它通过修改对象的布局属性来产生动画效果。但是也可以通过定义的动画让属性按照动画的定义来改变（这个改变用户可能在屏幕上看不到任何动画效果）。
</p>
<p style="text-indent:2em;">
    Property Animation有如下特性：
</p>
<p style="text-indent:2em;">
    <br />
</p>
<ul>
    <li>
        <span style="line-height:1.5;">持续时间：动画的持续时间。</span> 
    </li>
    <li>
        <span style="line-height:1.5;">时间插值：通过方法设定动画在当前时间的属性值。</span> 
    </li>
    <li>
        <span style="line-height:1.5;">重复次数与行为：设定动画结束后是否重复与动画的重复次数，并且可以反转动画。</span> 
    </li>
    <li>
        <span style="line-height:1.5;">动画组：可以将一组动画按照顺序放到组中按顺序执行，并可以设置动画间的延迟时间。</span> 
    </li>
    <li>
        <span style="line-height:1.5;">帧刷新延迟：你可以设定多久刷新动画帧。默认值是10ms刷新一次，但是这个时间是依据系统是否繁忙与系统执行定时器的速度。</span> 
    </li>
</ul>
<p>
    <br />
</p>
<p style="text-indent:2em;">
    <!--more-->
</p>
<hr />
<h3>
    它是如何工作的
</h3>
<p>
    <img src="http://developer.android.com/images/animation/valueanimator.png" alt="" /> 
</p>
<p>
    上图是官方给出的动画计算用到的重要组件，与计算方式。
</p>
<p>
    ValueAnimator对象追中动画的时间，比如动画执行了多久和当前的属性值。
</p>
<p>
    ValueAnimator包含TimeInterpolator和TypeEvaluator，前者用来定义动画的差值，后者定义属性在动画中如何计算。
</p>
<p>
    要创建一个动画，手续创建一个ValueAnimator对象给在动画中的属性一个开始值和结束值。当执行start()方法，动画开始执行。在整个动画过程中，ValueAnimator计算一个过渡分量0-1，依据动画的持续时间和已执行的动画时间。0表示动画执行了0%1表示动画执行完。
</p>
<p>
    当ValueAnimator计算完一次过渡分量，它会调用设定的TimeInterpolator来计算插值分量，插值分量映射过渡分量到一个新的分量来进行插补运算。
</p>
<p>
    当插值分量计算完，ValueAnimator调用适当的TypeEvaluator来依据分量因子计算动画的属性值。
</p>
<p>
    在API Demos的<span style="line-height:1.5;">com.example.android.apis.animation包中提供了使用property animation的实例。</span> 
</p>
<p>
    <br />
</p>