---
layout: post
title: "How to port sox on Android"
description: ""
category: 
tags: [sox]
---
{% include JB/setup %}


###将sox移植到android平台上需要注意的几点：

1.	不要在iOS、windows平台下进行./configure操作， 可能是由于sox需要的系统库与linxu有些不同。

	如果非要在iOS、windows平台下进行./configure操作，就需要带参数 host=arm-eabi 

	即 ./configure host=arm-eabi 

2.	根据你是否有一些头文件来修改soxconfig.h

3.	为你需要编译的库编写android.mk文件.

4.	The project on [my bitbucket](https://bitbucket.org/LukeLu1263/sox-android-lib)

##Reference

[http://habrahabr.ru/post/119693/](http://habrahabr.ru/post/119693/) (俄文)

[https://github.com/Kyborg2011/SoxPlayer](https://github.com/Kyborg2011/SoxPlayer) (android)
