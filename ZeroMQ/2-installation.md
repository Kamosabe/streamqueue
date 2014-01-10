## Installation
(http://zeromq.org/area:download)

### Linux
Make sure you have libtool, autoconf, automake, uuid-dev installed, otherwise you have may have to install them by:

    sudo apt-get install libtool
	sudo apt-get install autoconf
	sudo apt-get install automake
	sudo apt-get install uuid-dev

Also make sure you have C++ Compiler installed, otherwise install it:

	sudo apt-get install build-essential

Then go to Download section on ZeroMQ homepage (http://zeromq.org/area:download) and download the latest stable release.

Afterwards you have to unpack the tarball, create the make file and then build and install the program:

	tar -xvzf zeromq...tar.gz
	./configure
	make
	sudo make install

If you do not have java installed already, you can install it by:

	sudo apt-get install default-jre
	sudo apt-get install default-jdk


### Windows
Download the latest stable relase of ZeroMQ (http://zeromq.org/distro:microsoft-windows) and install it somewhere on your local drive (e.g. "c:/")

...


### Java Binding

#### Windows
JeroMQ (Pure Java ZeroMQ):

* JAR Files: http://repo.typesafe.com/typesafe/repo/org/jeromq/jeromq/0.3.0-SNAPSHOT/
* Benchmark: http://nguyentantrieu.info/blog/benchmark-pubsub-jeromq-java/


#### Linux
Clone JZMQ from Github:
	
	git clone https://github.com/zeromq/jzmq.git

Go to into folder:

	cd jzmq

Run the "autogen" script:
	
	./autogen.sh

Build and install it:

	./configure
	make
	sudo make install  

