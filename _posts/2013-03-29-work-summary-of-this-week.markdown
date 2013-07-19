---
layout: post
title: "Work Summary Of This Week"
description: "关于iOS本周学习的内容"
category: Summary 
tags: [Weekly Summarize]
---


#本周主要学习了以下几个知识点:
##[Blogging Like A Hacker](http://pages.github.com)
整个博客就是使用terminal写的，然后通过push到github的pages上，经过github服务器端编译生成该博客。

##[SVPullToRefresh](https://github.com/samvermette/SVPullToRefresh)
此第三方库比EGOPullToRefresh更灵活，因为EGO需要继承于UITableView，而SVPullToRefresh使用block语句灵活的处理。
他的两个特点是：1.下拉刷新  2.自动加载tableView

##[Regular Expression](https://github.com/wezm/RegexKitLite)
RegexKitLite可以支持ICU标准的正则表达式，只要写出对应的正则表达式，就可以匹配出符合该表达式的语句，使用方便简单。

##[RTLabel](git://github.com/honcheng/RTLabel.git)
RTLabel是一个Rich Text的第三方库，可以使用他设置字体类型、字体颜色以及设置字体链接等。

##[Block语句](https://developer.apple.com/library/ios/#featuredarticles/Short_Practical_Guide_Blocks/index.html#//apple_ref/doc/uid/TP40009758)
总的来说，block类似于C函数指针，但却比指针语法更灵活，正式因为他可以封装程序指令所带来的便捷。

##Migrating to Delegate pattern from NotificationCenter
将获取数据的方式更改为delegate方式而不是使用notificationcenter，使得代码更简洁。

##其他
另外的工作都是一些细节性的缝缝补补，知识补充，还需要梳理整体知识结构。
