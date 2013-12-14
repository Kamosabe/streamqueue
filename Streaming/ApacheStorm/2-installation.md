## Installation

The following installation explains how to build and run a storm cluster on a single machine. At the end of this chapter there will be shown a demo of the wordcount topology. The installation will be done on a Linux operating system especially the Linux distribution Ubuntu is beeing used.

For building a storm cluster there are depenendencies to be solved. The next chapters are describing the building process.

### Apache Zookeeper

Zookeeper can be installed with aptitude. The configuration files are stored with default settings in the filesystem in the path /etc/zookeeper. There are no modification in terms of optimizations.
To check if zookeeper is up and running a zookeeper client should get a connection:
```
./zkCli.sh -server 127.0.0.1:2181 
```

### ZeroMQ

ZeroMQ is a message queuing library and is used for message passing. Download and build zeromq. Because storm uses java bindings to zeromq jzmq must be build.

Download, extract and build zeromq:
```
wget download.zeromq.org/zeromq-3.2.4.tar.gz
tar xvfz zeromq-3.2.4.tar.gz
./configure
make 
make install
ldconfig
```

Download, extract and build binding jzmq:
```
wget github.com/zeromq/jzmq/archive/master.zip
./autogen.sh
./configure
make
```

### Storm

Download and extract storm:
```
wget https://www.dropbox.com/s/fl4kr7w0oc8ihdw/storm-0.8.2.zip
unzip storm-0.8.2.zip
```

If storm is extracted to the filesystem path /opt/storm the configuration file /opt/storm/conf/storm.yaml must be updated with following entry:
```
storm.local.dir: "/opt/storm"
```

### Running cluster

Before storm can be started zookeeper must be running.
Storm needs to start nimbus, supervisor and the ui.
First nimbus should be started:
```
/opt/storm/bin/storm nimbus
``` 

After nimbus has been started the supervisor should be started:
```
/opt/storm/bin/storm supervisor
``` 

At the the end the ui should be started:
```
/opt/storm/bin/storm ui
``` 

Every command should be executed in a separate shell. So that storm is up and running on system start, startup scripts needs to be created. Normally there are skeleton files in /etc.

### Running wordcount topology

To run the topology the storm-starter package will be used. Also to start maven must be available on the system.

Clone with git storm-starter into /opt/storm-starter:
```
git clone https://github.com/nathanmarz/storm-starter.git
```

Run the topology in the shell (/opt/storm-starter):
```
mvn -f m2-pom.xml compile exec:java -Dexec.classpathScope=compile -Dexec.mainClass=storm.starter.WordCountTopology
```

Result output:
```
...
11245 [Thread-72] INFO  backtype.storm.daemon.executor  - Processing received message source: split:6, stream: default, id: {}, ["an"]
11245 [Thread-72] INFO  backtype.storm.daemon.task  - Emitting: count default [an, 44]
11245 [Thread-72] INFO  backtype.storm.daemon.executor  - Processing received message source: split:6, stream: default, id: {}, ["day"]
11245 [Thread-72] INFO  backtype.storm.daemon.task  - Emitting: count default [day, 44]
11245 [Thread-72] INFO  backtype.storm.daemon.executor  - Processing received message source: split:6, stream: default, id: {}, ["the"]
11245 [Thread-72] INFO  backtype.storm.daemon.task  - Emitting: count default [the, 198]
11245 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.supervisor  - Shutting down b2e95bd5-af88-4ac3-9933-91fbdc62373d:144f804a-5610-4a7c-9f07-2eb202a56324
11245 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.process-simulator  - Killing process 6da59a4c-7a7e-4485-8636-ea1633ea9efa
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.worker  - Shutting down worker word-count-1-1384126173 b2e95bd5-af88-4ac3-9933-91fbdc62373d 4
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.worker  - Shutting down receive thread
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.messaging.loader  - Shutting down receiving-thread: [word-count-1-1384126173, 4]
11246 [Thread-95] INFO  backtype.storm.messaging.loader  - Receiving-thread:[word-count-1-1384126173, 4] received shutdown notice
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.messaging.loader  - Waiting for receiving-thread:[word-count-1-1384126173, 4] to die
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.messaging.loader  - Shutdown receiving-thread: [word-count-1-1384126173, 4]
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.worker  - Shut down receive thread
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.worker  - Terminating zmq context
11246 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.worker  - Shutting down executors
11247 [storm.starter.WordCountTopology.main()] INFO  backtype.storm.daemon.executor  - Shutting down executor count:[2 2]
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 44.692s
[INFO] Finished at: Mon Nov 11 00:29:46 CET 2013
[INFO] Final Memory: 33M/82M
[INFO] ------------------------------------------------------------------------
```
