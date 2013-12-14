## Installation

The following chapter explains the installation of Apache ActiveMQ on a Windows and a Unix operating system.

* **Demo???**
* **Dependencies???**

----------

## Windows
In the following it is explained how to install and run the binary distribution on a Windows system.

### Windows Binary Installation

 1. Download the latest release from http://activemq.apache.org/download.html
 2. Extract the ZIP file to a directory of your choice

### Starting ActiveMQ
After the successful installation we have to run the ActiveMQ Message Broker. Therefore it is necessary to open the Windows Console and to change to the installation directory:

    cd [activemq_install_dir]

In that case [activemq_install_dir] is the directory in which ActiveMQ was installed (e.g. C:\Program Files\ActiveMQ-4.x). 

Then you can run ActiveMQ by typing:

    bin\activemq

It is important to know that ActiveMQ creates a working directory relative to the current directory. If you want another place it is necessary to change the directory and launch ActiveMQ from there. 
 
----------

## Unix
This procedure explains how to download and install the binary distribution on a Unix system.

### Unix Binary installation
Download the activemq gzip file to the Unix machine, using either a browser or a tool, i.e., wget, scp, ftp, etc. for example:

    > wget http://activemq.apache.org/path/tofile/apache-activemq-4.1.0-incubator.tar.gz

Extract the files from the gzip file into a directory of your choice. For example:

    > tar zxvf activemq-x.x.x.tar.gz

If the ActiveMQ start-up script is not executable, change its permissions. The ActiveMQ script is located in the bin directory. For example:

    > cd [activemq_install_dir]/bin

### Starting ActiveMQ
From a command shell, change to the installation directory and run ActiveMQ:

    cd [activemq_install_dir]

where activemq_install_dir is the directory in which ActiveMQ was installed, e.g., /usr/local/activemq-4.x. 
Then type:

    bin/activemq
    
    OR
    
    bin/activemq > /tmp/smlog  2>&1 &;
    Note: /tmp/smlog may be changed to another file name.
    
It is important to know that ActiveMQ creates a working directory relative to the current directory. If you want another place it is necessary to change the directory and launch ActiveMQ from there.