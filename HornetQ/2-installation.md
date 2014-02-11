## Installation

This section explains the installation of JBoss HornetQ on a Windows operating system.

> HornetQ only runs on Java 6 or later.

----------

## Stand-alone HornetQ Server
How to install and run the stand-alone HornetQ server is explained in the following.

### Installation
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
After successful installation run the HornetQ Server. Therefore it is necessary to open the Windows Console and change to the installation directory:

    cd [hornetq_install_dir]

The placeholder [hornetq_install_dir] is the directory in which HornetQ was installed (e.g. C:\Program Files\HornetQ-2.x). 

Then type:

    bin\run.bat
