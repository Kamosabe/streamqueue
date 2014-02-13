## Installation
This section explains the installation of Apache ActiveMQ on a Windows and a Unix operating system.

### Windows
How to install and run the binary distribution on a Windows system is described in the following.

#### Windows Binary Installation
 1. Download the latest release from http://activemq.apache.org/download.html
 2. Extract the ZIP file to a directory of your choice

The following describes the directory structure of the downloaded archive: 

     |___ bin
     |
     |___ conf
     |
     |___ data
     |
     |___ docs
     |
     |___ examples
     |      |___ ampq
     |      |___ mqtt
     |      |___ openwire
     |      |___ stomp
     |
     |___ lib
     |
     |___ webapps
     |
     |___ webapps-demo

 - ***bin*** -- binaries and scripts required to run ActiveMQ
 - ***conf*** -- configuration files required to configure ActiveMQ
 - ***data*** -- directory where the log files and message persistence data is stored
 - ***docs*** -- guides for ActiveMQ
 - ***examples*** -- examples for different APIs, languages, protocols, ...
 - ***lib*** -- jars and libraries needed by ActiveMQ
 - ***webapps*** -- holds the ActiveMQ web console
 - ***webapps-demo*** -- some web-related demos
 
#### Starting ActiveMQ
After successful installation run the ActiveMQ Message Broker. Therefore it is necessary to open the Windows Console and change to the installation directory:

    cd [activemq_install_dir]

The placeholder *[activemq_install_dir]* is the directory in which ActiveMQ was installed (e.g. C:\Program Files\ActiveMQ-5.x). 

Run ActiveMQ by typing in:

    bin\activemq

It is important to know that ActiveMQ creates a working directory relative to the current directory. If you want another place change the directory and launch ActiveMQ from there. 
 
----------

### Unix
This procedure explains how to download and install the binary distribution on a Unix system.

#### Unix Binary Installation
Download the ActiveMQ-GZip file to the Unix machine using either a browser or a tool (e.g. wget, scp, ftp):

    > wget http://activemq.apache.org/path/tofile/apache-activemq-4.1.0-incubator.tar.gz

Extract the files from the GZip file into a directory of your choice. For example:

    > tar zxvf activemq-x.x.x.tar.gz

If the ActiveMQ start-up script is not executable change its permissions. The ActiveMQ script is located in the bin directory. For example:

    > cd [activemq_install_dir]/bin
    
The structure of the downloaded archive is shown above as part of the Windows installation. 

#### Starting ActiveMQ
Use a command shell, change to the installation directory and run ActiveMQ:

    cd [activemq_install_dir]

The placeholder *[activemq_install_dir]* is the directory in which ActiveMQ was installed (e.g. /usr/local/activemq-5.x).

Then type:

    bin/activemq
    
    OR
    
    bin/activemq > /tmp/smlog  2>&1 &;
    Note: /tmp/smlog may be changed to another file name.
    
It is important to know that ActiveMQ creates a working directory relative to the current directory. If you want another place change the directory and launch ActiveMQ from there.