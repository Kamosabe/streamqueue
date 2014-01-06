## Installation

In the next chapters the installation process of Apache S4 will be explained. In the installation process a Linux operating system with the Ubuntu distribution is beeing used. Before S4 can be installed a project automation tool gradle ([MUSC13]) must be installed. Also check if Apache Zookeeper is available.

### Gradle

Download and extract gradle archive from the official web site:
```
wget http://services.gradle.org/distributions/gradle-1.6-bin.zip 
unzip gradle-1.6-bin.zip
``` 

Set the Gradle environment variable and install gradle:
``` 
export GRADLE_HOME=/foo/bar/gradle-1.6 
export PATH=$PATH:$GRADLE_HOME/bin 
gradle
``` 

### Apache Zookeeper

Zookeeper can be installed with aptitude. The configuration files are stored with default settings in the filesystem in the path /etc/zookeeper. There are no modification in terms of optimizations.
To check if zookeeper is up and running a zookeeper client should get a connection:
```
./zkCli.sh -server 127.0.0.1:2181 
```

### S4

Download and extract S4 archive:
```
wget http://www.apache.org/dist/incubator/s4/s4-0.6.0-incubating/apache-s4-0.6.0-incubating-src.zip
unzip apache-s4-0.6.0-incubating-src.zip
```

Set the S4 environment variable:
```
export S4_HOME=/foo/bar/apache-s4-0.6.0-incubating-src
export PATH=$PATH:$S4_HOME
```

Change directory to S4_Home and install S4
```
gradle install
gradle s4-tools::installApp
```

If there are troubles in the installation procedure with 
```
gradle tasks
```
it is possible to get all possible project building tasks. Make sure the path of execution is correct.

[MUSC13]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#MUSC13  "Gradle in Action"