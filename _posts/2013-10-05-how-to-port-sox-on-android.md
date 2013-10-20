---
layout: post
title: "How to port sox on Android"
description: ""
category: 
tags: [sox]
---
{% include JB/setup %}


## Porting sox on android you need to notice the following:

1.  **DO NOT** execute configure script on OSX or windows. This will cause different file or configuration corresponding to their system.

	If you want to do that, please take the parameters for the script like this:

	./configure host=arm-eabi 

	(--help for some help. Enable or disable some libraries)

2.	According to whether you have some header or not, modify soxconfig.h.

3.  write **android.mk** for the libraries you want to use.

4.  The lpc10、libgsm and wavpack are prerequisite.

5.	The project is on [my bitbucket](https://bitbucket.org/LukeLu1263/sox-android-lib)

##Reference

[http://habrahabr.ru/post/119693/](http://habrahabr.ru/post/119693/) (俄文)

[https://github.com/Kyborg2011/SoxPlayer](https://github.com/Kyborg2011/SoxPlayer) (android)
