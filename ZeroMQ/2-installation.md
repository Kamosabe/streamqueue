## Installation
(http://zeromq.org/area:download)

### Linux

* Make sure libtool, autoconf, automake, uuid-dev is installed
	* otherwise: sudo apt-get install 'package-name'
* make sure c++ compiler is installed
	* otherwise: sudo apt-get install build-essential
* Unpack the tar.gz file
	* tar -xvzf zeromq...tar.gz
* Configure, Make, Make install (http://tldp.org/LDP/LG/current/smith.html)
	* ./configure
	* make
	* sudo make install
* install java:
	* sudo apt-get install default-jre
	* sudo apt-get install default-jdk
*      


### Java Binding

#### Windows
JeroMQ (Pure Java ZeroMQ):

* JAR Files: http://repo.typesafe.com/typesafe/repo/org/jeromq/jeromq/0.3.0-SNAPSHOT/
* Benchmark: http://nguyentantrieu.info/blog/benchmark-pubsub-jeromq-java/


#### Linux
* Clone JZMQ from Github:
	* git clone https://github.com/zeromq/jzmq.git
* Build
	* cd jzmq
	* ./autogen.sh
	* ./configure
	* make
	* sudo make install  