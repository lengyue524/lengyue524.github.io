---
layout: post
title: Android中的Garbage Collection
categories: [Android Tips]
tags: [Android,java]
comments: true
description: John McCarthy早在1959年lisp语言中就发明了Garbage Collection。
---
John McCarthy早在1959年lisp语言中就发明了Garbage Collection。早期的gc遵循两个原则：
1. 找到不再使用的对象
2. 回收它

但是我们如何从内存中找到这些对象呢，这是一个很复杂的问题。但是经过50多年的改革创新，Android中的Garbage Collection要比John McCarthy的原始版本更加高效而且非侵入式。

为了让gc事件更加高效，Android运行时将内存依据数据类型分割为不同的区块，让系统能够更好的组织分配内存。系统会更加对象的特性将其放到最合适的区块中。

![][image-1]

在不同的Android运行时下，gc的表现也是不同的。

在Dalvik虚拟机中，gc会暂停当前运行的代码，直到gc完成才会继续运行。如果gc的执行时间过长或频繁的gc就会导致APP运行不畅。

![][image-2]

而在ART虚拟机中，gc事件和和当前运行的代码是异步运行的，只在gc时间完成时会让当前运行的代码短暂的暂停。这提升了APP的运行效率，但是我们仍然要注意内存管理来避免内存抖动和OOM。

![][image-3]


[image-1]:	http://7xky0m.com1.z0.glb.clouddn.com/3769277C-2AB0-4A5C-8356-4EAB8981DF9A.jpeg
[image-2]:	http://7xky0m.com1.z0.glb.clouddn.com/89F53CD7-4383-4E05-B649-1A5A03319D2C.png
[image-3]:	http://7xky0m.com1.z0.glb.clouddn.com/BF9E0CA8-72D8-4464-9810-9B32116D7F30.png