## Installation

The chapter will describe how to install RabbitMQ on different platforms including Windows and Linux. As RabbitMQ Server is based on Erlang we first have to download, install and configure Erlang for the specific platform.

### Erlang Installation

#### Linux

##### Installing from source package

First you have to install some dependencies Erlang has:

	sudo apt-get install build-essential
	sudo apt-get install libncurses5-dev 
	sudo apt-get install openssl 
	sudo apt-get install libssl-dev
	sudo apt-get install fop 
	sudo apt-get install xsltproc 
	sudo apt-get install unixodbc-dev

After you installed the required dependency packages you can download the latest version of Erlang, unzip and build it:

	wget http://erlang.org/download/otp_src_R15B01.tar.gz
	tar zxvf otp_src_R15B01.tar.gz
	cd otp_src_R15B01
	./configure 
	make 
	sudo make install

##### Ubunutu

On ubuntu platform it is very easy to install Erlang. Just open a terminal, type in the following and press enter: 

	sudo apt-get install erlang


#### Windows

Download Erlang Windows Installer from *Download* section of Erlang's homepage (http://www.erlang.org/download.html) and install it by following the instructions.

Then you should add the installation path of Erlang (up to the bin folder) to the *PATH* enviroment variable. You can do so by navigating to "*Control Panel > System and Security > System Properties > Advanced > Environment Variables > User Variables*" and editing the described entry.

![Erlang Windows environment variable](images/erlang-env-variable.PNG)


### RabbitMQ Server Installation

#### Windows

##### With Installer
Download the RabbitMQ Installer for Window systems from the Download section (http://www.rabbitmq.com/download.html) and install it. It will setup the RabbitMQ server as an service with default configuration.
The server will be started on windows start.

##### Manual
Download RabbitMQ Server .zip-File from Download section of RabbitMQ homepage (http://www.rabbitmq.com/download.html), unzip it and place the content of *rabbitmq_server-XXX* folder in some suitable directory.

To get the server started you can run *rabbitmq-server.bat* in *sbin* directory. To manage the service and start the broker you should run *rabbitmq-service.bat*.

#### Linux (Ubuntu / Debian)
The RabbitMQ server is included in Debian (> Version 6.0) and Ubuntu (> Version 9.04). But as these included versions are often old, it will probably be better to install current version from RabbitMQ homepage.

One way to do this is to go to download section of RabbitMQ homepage (http://www.rabbitmq.com/download.html), download the .deb-file and install it.

Another possibility is to use RabbitMQ APT repository:
	
	deb http://www.rabbitmq.com/debian/ testing main
	wget http://www.rabbitmq.com/rabbitmq.com/rabbitmq-signing-key-public.asc
	sudo apt-key add rabbitmq-signing-key-public.asc
	apt-get update
	sudo apt-get install rabbitmq-server


### Official Clients
The RabbitMQ community has created a large number of clients in different languages such as Java, C, Clojure,  Ruby, Python, PHP, Perl and many more. 

As we are making use of Java Programming language in this book, we will demonstrate where to download and how to use the Java client.

First you have to download the RabbitMQ  Java Client from Download section (http://www.rabbitmq.com/download.html). 

Unzip the archive and put the jar files into your classpath.
If you are using Eclipse you should add the jar-Files into Java Build Path of your project.

![Eclipse Build Path](images/client-project-build-path.PNG)