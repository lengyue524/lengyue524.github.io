---
layout: post
title: SwipeRefreshLayout的使用
categories: [Android,Java]
tags: [Android,java,layout]
comment: true
---
最近官方推出了自己的下拉刷新布局**[SwipeRefreshLayout](https://developer.android.com/reference/android/support/v4/widget/SwipeRefreshLayout.html)**，下面介绍下此布局的使用方法。

布局中只能包含一个child，并且child必须是可滚动的，比如ScrollView或ListView。

{% highlight yaml %}
//布局文件中找到SwipeRefreshLayout初始化
SwipeRefreshLayout mSrl = (SwipeRefreshLayout) rootView.findViewById(R.id.swiperefresh);
//设置刷新布局的颜色序列，刷新时会按照颜色顺序变化
mSrl.setColorScheme(android.R.color.holo_blue_bright,
android.R.color.holo_green_light,
android.R.color.holo_blue_dark,
android.R.color.holo_orange_light);
//设置刷新事件监听
mSrl.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {

    @Override
    public void onRefresh() {
        //刷新列表
        LoadAllTaskList();
    }
}
{% endhighlight %}

效果如图所示：

![SwipeRefreshLayout](http://1.bp.blogspot.com/-IVuCm0r7DpQ/VCjxBBobsFI/AAAAAAAAAxQ/f7WjSE5mCNA/s1600/hbPullToRefresh.png)