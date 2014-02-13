## Installation

This section explains the installation of HornetQ on a Windows and Unix operating system.

> HornetQ only runs on Java 7 or later.

----------

## Stand-alone HornetQ Server
How to install and run the stand-alone HornetQ server is explained in the following.

### Installation
Steps of installation:

 1. Download the latest release from http://www.jboss.org/hornetq/downloads.html
 2. Extract the ZIP file to a directory of your choice

The following describes the directory structure of the downloaded archive: 

     |___ bin
     |
     |___ config
     |      |___ jboss-as-4
     |      |___ jboss-as-5
     |      |___ stand-alone
     |
     |___ docs
     |      |___ api
     |      |___ quickstart-guide
     |      |___ user-manual
     |
     |___ examples
     |      |___ core
     |      |___ javaee
     |      |___ jms
     |
     |___ lib
     |
     |___ licenses
     |
     |___ schemas

 - *bin* -- binaries and scripts required to run HornetQ
 - *config* -- configuration files required to configure HornetQ containing default configurations
 - *docs* -- guides and javadocs for HornetQ
 - *examples* -- JMS and Java EE example
 - *lib* -- jars and libraries required to run HornetQ
 - *licenses* -- licenses for HornetQ
 - *schemas* -- XML Schemas used to validate HornetQ configuration files


### Starting HornetQ
After successful installation run the HornetQ Server. Therefore it is necessary to open up a shell or command prompt and navigate to the installation directory:

    cd [hornetq_install_dir]

The placeholder [hornetq_install_dir] is the directory in which HornetQ was installed (e.g. C:\Program Files\HornetQ-2.x). 

Then type on Unix:

    bin$ ./run.sh
    
or on Windows:

    bin\run.bat
    
HornetQ is starting now with the following output:
    
    15:05:54,108 INFO  @main [HornetQBootstrapServer] Starting HornetQ server
    ...
    15:06:02,566 INFO  @main [HornetQServerImpl] HornetQ Server version 
    2.0.0.CR3 (yellowjacket, 111) started