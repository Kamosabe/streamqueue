## Installation
 - http://zeromq.org/area:download
 - http://stackoverflow.com/questions/12901871/install-compile-zeromq-zmq-0mq-on-ubuntu-12-04-32bit
 - http://maddigitiser.wordpress.com/2013/05/02/installing-zeromq-on-ubuntu-13-04/

This chapter will describe how to install and get ZMQ running on Linux and Window systems. 

### Linux
Make sure libtool, autoconf, automake, uuid-dev are installed, otherwise you may have to install them by:

    sudo apt-get install libtool
	sudo apt-get install autoconf
	sudo apt-get install automake
	sudo apt-get install uuid-dev

Also make sure you have C++ Compiler installed, otherwise install it:

	sudo apt-get install build-essential

Then go to Download section on ZeroMQ homepage (http://zeromq.org/area:download) and download the latest stable release. Alternatively you can directly download it into your current path via:

	wget http://download.zeromq.org/zeromq-4.0.3.tar.gz

Afterwards you have to unpack the tarball.

	tar -xvzf zeromq-4.0.3.tar.gz

Then create the make file, build and install the program:

	cd zeromq-4.0.3
	./configure
	make
	sudo make install
	sudo ldconfig


As all of the examples in this book are making use of Java Programming Language it should be installed on your system. If you do not have java installed, you can install it by:

	sudo apt-get install default-jre
	sudo apt-get install default-jdk


For further information about Java Programming language binding for ZeroMQ see chapter 4 "APIs".


### Windows
 - http://zeromq.org/area:download
 - http://zeromq.org/docs:windows-installations

There are two ways to install ZeroMQ on an Windows system.

#### Installer

Go to "Installers for Microdsoft Windows" page (http://zeromq.org/distro:microsoft-windows), download the latest stable release for your system (x64 or x86) and install it into a directory of your choice.

#### build with Microsoft Visual C++ 2008

First you need Microsoft Visual C++ 2008 or newer installed. 

Then go to download section of ZeroMQ homepage(http://zeromq.org/area:download) and download Windows sources of the latest stable release.

Unpack the .zip source archive and open the solution `buildes\msvc\msvc.sln` in Visual C++ and build it. The ZeroMQ libraries will be in the `lib` subdirectory.