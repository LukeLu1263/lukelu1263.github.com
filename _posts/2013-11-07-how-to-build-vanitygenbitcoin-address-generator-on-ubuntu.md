---
layout: post
title: "How to build vanitygen(bitcoin address generator) on ubuntu?"
description: "Building bitcoin vanity address generator step by step."
category: 
tags: [bitcoin]
---
{% include JB/setup %}

Install Ubuntu 12.04
-----------------------------------------
You can edit the file after installation.

*/etc/lightdm/lightdm.conf*

	[SeatDefaults]
	autologin-guest=false
	autologin-user=<your username>
	autologin-user-timeout=0
	autologin-session=lightdm-autologin
	greeter-session=lightdm-gtk-greeter
	user-session=xubuntu



Install AMD Driver on Xubuntu
-----------------------------------------

> Caution: Choose your driver wisely, or you’ll lose 10% ~ 20% hash power!

###For AMD Raedon 7xxx Series

* If any of your cards on board is AMD Raedon 7xxx Series cards, you’ll need AMD APP SDK 2.6+, which is included in Catalyst 11.12+ and beyond (I recommend Catalyst 12.3 - which include SDK 2.6).

> Reason: 7xxx Series use some kind of new architecture called GCN, which can only be recognized with SDK 2.6+.

Now follow the steps (**in commandline**):

*(Don’t use the GUI (Settings -> Additional drivers) to install the post-release driver. It will fail and I don’t know why.)*

	sudo apt-get install fglrx-updates fglrx-amdcccle-updates fglrx-updates-dev

Done!

###For AMD Raedon 6xxx, 5xxx Series

* If you don’t have any AMD Raedon 7xxx Series cards (like 6xxx, 5xxx), you should use SDK 2.5, which is included in Catalyst 11.7 ~ 11.11. (Recommend Catalyst 11.11) 
>Reason: Non-7xxx Series with SDK 2.6 will lose about 10% performance. 

* **Note that**: For AMD Raedon 5xxx Series, **Vanitygen** only work well on SDK 2.7 and below. SDK 2.8 and later is bad!

Now follow the steps.

* Install the prerequisite packages

	sudo apt-get install build-essential cdbs dh-make dkms execstack dh-modaliases fakeroot libqtgui4

*If you are using the x86_64 architecture (64 bit):*

	sudo apt-get install ia32-libs-multiarch:i386 lib32gcc1 libc6-i386
	sudo apt-get install ia32-libs

Compile and install the driver:

	wget http://www2.ati.com/drivers/linux/ati-driver-installer-11-11-x86.x86_64.run
	sudo sh ./ati-driver-installer-11-11-x86.x86_64.run --buildpkg Ubuntu/precise
	sudo dpkg -i fglrx*.deb

Done!

Config AMD Driver
-----------------------------------------

Check if all your cards can be detected and then write configuration to **/etc/X11/xorg.conf**.

	aticonfig --lsa
	sudo aticonfig --adapter=all --initial

If everything went fine, reboot the computer by “**sudo reboot**”.

After that, check if everything works:

	sudo aticonfig --adapter=all --odgt

Now enjoy a headless Xubuntu!

######You can also reference this [relative guide](https://docs.google.com/document/d/1Gw7YPYgMgNNU42skibULbJJUx_suP_CpjSEdSi8_z9U/edit# "AMD-GPU-Driver").


Build Vanitygen from source code
---------------------------------------
Refer to INSTALL in vanitygen root directory.

You should install OpenSSL, PCRE and AMD APP SDK(see above).

	sudo apt-get install libpcre++-dev 

(On OSX, instead you should linking /opt/local/include/pcre.h into /usr/include/)

After you make , you will done this all. But Maybe you will false for some reason about AMD include files and lib files.

If you get these error, modify the Makefile to be:
	
	LIBS=-lpcre -lcrypto -lm -lpthread -I/opt/AMDAPP/lib/x86_64/lib  ## I added lib path
	CFLAGS=-ggdb -O3 -Wall -I/opt/AMDAPP/include 					 ## I added include path
	OBJS=vanitygen.o oclvanitygen.o oclvanityminer.o oclengine.o keyconv.o pattern.o util.o
	PROGS=vanitygen keyconv oclvanitygen oclvanityminer

	PLATFORM=$(shell uname -s)
	ifeq ($(PLATFORM),Darwin)
	OPENCL_LIBS=-framework OpenCL
	else
	OPENCL_LIBS=-lOpenCL
	endif

	most: vanitygen keyconv

	all: $(PROGS)

	vanitygen: vanitygen.o pattern.o util.o
	        $(CC) $^ -o $@ $(CFLAGS) $(LIBS)
	
	oclvanitygen: oclvanitygen.o oclengine.o pattern.o util.o
	        $(CC) $^ -o $@ $(CFLAGS) $(LIBS) $(OPENCL_LIBS)
	
	oclvanityminer: oclvanityminer.o oclengine.o pattern.o util.o
	        $(CC) $^ -o $@ $(CFLAGS) $(LIBS) $(OPENCL_LIBS) -lcurl
	
	keyconv: keyconv.o util.o
	        $(CC) $^ -o $@ $(CFLAGS) $(LIBS)
	
	clean:
	        rm -f $(OBJS) $(PROGS) $(TESTS)
	        
	        
Enjoy that!
