---
layout: post
title: "Makefile Sample Code"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}

##Makefile 指定lib, include, compiler等路径

现有libmad交叉编译后安装于：/home/andy/share/libmad_install目录
现在应用程序miniplayer位于：/home/andy/share/mini_player目录

mini_player中的makefile里，怎么指定libmad库和mad.h头文件的路径呢？


指定 头文件用
 -I /home/andy/share/mini_player
指定 库文件： -L 后面是具体的目录。
-L /home/andy/share/libmad_install

makefile如下，先编译完，拷到arm板上运行时提示：
/flac_app: error while loading shared libraries: librt.so.1: cannot open shared object file: No such file or directory
这些lib我已经拷到板上的/lib里了，怎么会找不到
#
# Makefile for the CAMERA Application.
#
#以下是指定编译器路径
CC = /opt/armv6/codesourcery/bin/arm-none-linux-gnueabi-gcc
#以下是指定编译需要的头文件
CFLAGS = -g -Wall -O0 -I/home/andy/share/alsalib/include -I/home/andy/share/libmad_install/include
#以下是源文件
SRCS = main.c miniplayer_decode.c miniplayer_play.c
#以下是指定需要的库文件
LIBS = -L/home/andy/share/libmad_install/lib -lmad  -L/home/andy/share/alsalib/lib -lasound
#以下是指定目标文件 所有当.c文件变成.o文件
OBJS = $(SRCS:.c=.o)
#以下是生成可执行文件
EXECUTABLE = flac_app

#make all 执行生成可执行文件
#1编译器 2编译选项 3输出 4生成的可执行文件 5需要的源文件 6需要当库文件
all:
$(CC) $(CFLAGS) -o $(EXECUTABLE) $(SRCS) $(LIBS)

#make clean 删除所有的.o文件 和生成的可以执行文件
clean:
rm -f $(OBJS) flac_app


makefile中的指定头文件，源文件
可以使用VPATH变量也可以使用vpath后者可以分类指定头文件源文件的搜索路径
记住这样指定的路径仅仅是makefile本身查找头文件源文件的路径

在执行makefile时，还要指定gcc/g++搜索头文件库文件的搜索路径
-L //指定库文件搜索路径  
-ltest//指定使用的动态库/静态库
-I //指定搜索头文件的路径
