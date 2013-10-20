---
layout: post
title: "How to port sox on iOS"
description: ""
category: 
tags: [sox]
---
{% include JB/setup %}

##where is the compiler we need?
**Note:**	LLVM-GCC is not included in Xcode 5.

So, use the newer clang compiler instead like this:

**For CC**: xcrun -find -sdk iphoneos clang

**For CXX**: xcrun -find -sdk iphoneos clang++

**Note:** Not all llvm-gcc arguments that where used exist in clang. 

## Build Script
This [Build script]() for xcode 5 and [another script]() for xcode 4.

##Reference

[How to build libcurl(or any other project that uses GNU Autotools) for iOS](http://seiryu.home.comcast.net/~seiryu/libcurl-ios.html)

[Building sigc++ for iOS](http://ares.lids.mit.edu/redmine/projects/forest-game/wiki/Building_sigc++_for_iOS)

[Sox on sourceforge](http://sox.sourceforge.net/libsox.html)

[Sox on ios](https://github.com/shieldlock/SoX-iPhone-Lib)

[iOS equalizer with libsox doing effects](http://uberblo.gs/2011/04/iosiphoneos-equalizer-with-libsox-doing-effects)

[iOS equalizer with libsox making it a framework](http://uberblo.gs/2011/04/iosiphoneos-equalizer-with-libsox-making-it-a-framework)

[Solution for LLVM-GCC is not included in xcode 5](https://github.com/kivy/kivy-ios/issues/63)
