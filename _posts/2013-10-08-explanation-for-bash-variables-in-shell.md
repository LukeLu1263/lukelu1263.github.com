---
layout: post
title: "Explanation for 0, ,  bash,  and %2 variables in shell"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}


##What's the variable?

$$ 

Shell's PID（ProcessID） 

$! 

Shell最后运行的后台Process的PID 

$? 

最后运行的命令的结束代码（返回值） 

$- 

使用Set命令设定的Flag一览 

$* 

所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 

$@ 

所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 

$# 

添加到Shell的参数个数 

$0 

Shell本身的文件名 

$1～$n 

添加到Shell的各参数值。$1是第1参数、$2是第2参数…。 

我们先写一个简单的脚本，执行以后再解释各个变量的意义 

## For example
	touch variable 

	vi variable 

####The content is： 

	#!/bin/sh
	echo "number:$#" 
	echo "scname:$0" 
	echo "first :$1" 
	echo "second:$2" 
	echo "argume:$@" 

save and quit 

####grant the power

	chmod +x variable 

####Run the script

	./variable aa bb 

####Output:

number:2 

scname:./variable 

first: aa 

second:bb 

argume:aa bb 

####We can get something from the output:

$# 是传给脚本的参数个数 

$0 是脚本本身的名字 

$1是传递给该shell脚本的第一个参数 

$2是传递给该shell脚本的第二个参数 

$@ 是传给脚本的所有参数的列表
