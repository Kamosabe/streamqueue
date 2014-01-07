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

Change directory to S4_Home and install S4:
```
gradle install
gradle s4-tools::installApp
```

If there are troubles in the installation procedure with 
```
gradle tasks
```
it is possible to get all possible project building tasks. Make sure the path of execution is correct.

### Running cluster

Start a Zookeeper instance:
```
./s4 zkServer -clean
```

Result:
```
s4@s4:/opt/s4$ ./s4 zkServer -clean
02:56:56.169 [main] INFO  org.apache.s4.tools.ZKServer - Starting zookeeper server on port [2181]
02:56:56.172 [main] INFO  org.apache.s4.tools.ZKServer - cleaning existing data in [/tmp/tmp/zookeeper/data] and [/tmp/tmp/zookeeper/log]
02:56:56.466 [main] INFO  org.apache.s4.tools.ZKServer - Zookeeper server started on port [2181]
```


Define new cluster with name=cluster1 and 2 nodes:
```
./s4 newCluster -c=cluster1 -nbTasks=2 -flp=12000
```

Result:
```
s4@s4:/opt/s4$ ./s4 newCluster -c=cluster1 -nbTasks=2 -flp=12000
03:02:44.304 [main] INFO  org.apache.s4.tools.DefineCluster - preparing new cluster [cluster1] with [2] node(s)
03:02:44.587 [main] INFO  org.apache.s4.tools.DefineCluster - New cluster configuration uploaded into zookeeper
```

Start nodes in 2 shells:
```
./s4 node -c=cluster1
```

Create and deploy new application in cluster1:
```
./s4 s4r -a=hello.HelloApp -b=`pwd`/build.gradle myApp
./s4 deploy -s4r=`pwd`/build/libs/myApp.s4r -c=cluster1 -appName=myApp
```

Result first and second shell:
```
03:06:28.213 [main] INFO  o.a.s.comm.topology.AssignmentFromZK - New session:91027761808408578; state is : SyncConnected
03:06:28.327 [main] INFO  o.a.s.comm.topology.AssignmentFromZK - Successfully acquired task:Task-0 by s4

03:07:24.320 [main] INFO  o.a.s.comm.topology.AssignmentFromZK - New session:91027761808408579; state is : SyncConnected
03:07:24.420 [main] INFO  o.a.s.comm.topology.AssignmentFromZK - Successfully acquired task:Task-1 by s4
```


Provide 2nd cluster, deploy, start and provide some data to external stream (in the same directory of myApp):
```
./s4 newCluster -c=cluster2 -nbTasks=1 -flp=13000
./s4 deploy -appClass=hello.HelloInputAdapter -p=s4.adapter.output.stream=names -c=cluster2 -appName=adapter
./s4 adapter -c=cluster2
echo "Bob" | nc localhost 15000
```
Result of platform loader:
```
03:46:56.417 [S4 platform loader] INFO  org.apache.s4.core.S4Bootstrap - Loading application [myApp] from file [/tmp/tmp8539142349529098921s4r]
03:46:56.418 [S4 platform loader] WARN  org.apache.s4.core.DefaultCoreModule - s4.tmp.dir not specified, using temporary directory [/tmp/1388976416418-0] for unpacking S4R. You may want to specify a parent non-temporary directory.
03:46:56.423 [S4 platform loader] INFO  o.a.s4.base.util.S4RLoaderFactory - Unzipping S4R archive in [/tmp/1388976416418-0/tmp8539142349529098921s4r-1388976416423]
03:46:56.746 [S4 platform loader] INFO  org.apache.s4.core.S4Bootstrap - App class name is: hello.HelloApp
03:46:57.116 [S4 platform loader] INFO  o.a.s4.comm.topology.ClusterFromZK - Changing cluster topology to { nbNodes=2,name=cluster1,mode=unicast,type=,nodes=[{partition=0,port=12000,machineName=s4,taskId=Task-0}, {partition=1,port=12001,machineName=s4,taskId=Task-1}]} from null
03:46:57.371 [S4 platform loader] INFO  o.a.s4.comm.topology.ClusterFromZK - Adding topology change listener:org.apache.s4.comm.tcp.TCPEmitter@703b16bb
03:46:57.618 [S4 platform loader] INFO  o.a.s.c.s.ThrottlingSenderExecutorServiceFactory - Creating a throttling executor with a pool size of 1 and maxrate of 200000 events / s
03:46:57.674 [S4 platform loader] INFO  org.apache.s4.core.util.S4Metrics - Metrics reporting not configured
03:46:57.680 [S4 platform loader] INFO  o.a.s4.comm.topology.ClusterFromZK - Adding topology change listener:org.apache.s4.core.SenderImpl@366a782d
03:46:57.883 [S4 platform loader] INFO  o.a.s4.comm.topology.ClustersFromZK - New session:91027944759558147
03:46:57.890 [S4 platform loader] INFO  o.a.s4.comm.topology.ClustersFromZK - New session:91027944759558147
03:46:57.928 [S4 platform loader] INFO  o.a.s4.comm.topology.ClusterFromZK - Changing cluster topology to { nbNodes=2,name=cluster1,mode=unicast,type=,nodes=[{partition=0,port=12000,machineName=s4,taskId=Task-0}, {partition=1,port=12001,machineName=s4,taskId=Task-1}]} from null
03:46:57.931 [S4 platform loader] INFO  org.apache.s4.core.S4Bootstrap - Loaded application from file /tmp/tmp8539142349529098921s4r
03:46:58.044 [S4 platform loader] DEBUG o.a.s4.comm.topology.ClustersFromZK - Adding input stream [names] in cluster [cluster1]
03:46:58.080 [S4 platform loader] INFO  o.a.s4.comm.topology.ClustersFromZK - Detected new stream [names]
03:46:58.135 [S4 platform loader] INFO  org.apache.s4.core.App - Init prototype [hello.HelloPE].
```
Result of a node:
```
Hello Bob!
```

Status
```
s4@s4:/tmp/myApp$ ./s4 status
calling referenced s4 script : /opt/apache-s4-0.6.0-incubating-src/s4

App Status
----------------------------------------------------------------------------------------------------------------------------------
        Name              Cluster                                                  URI
----------------------------------------------------------------------------------------------------------------------------------
      adapter             cluster2      null
       myApp              cluster1      file:/tmp/myApp/build/libs/myApp.s4r
----------------------------------------------------------------------------------------------------------------------------------



Cluster Status
----------------------------------------------------------------------------------------------------------------------------------
                                                                                    Active nodes
        Name                App           Tasks   --------------------------------------------------------------------------------
                                                   Number    Task id                         Host                         Port
----------------------------------------------------------------------------------------------------------------------------------
      cluster2            adapter          1         1        Task-0                          s4                          13000
      cluster1             myApp           2         2        Task-0                          s4                          12000
                                                              Task-1                          s4                          12001
----------------------------------------------------------------------------------------------------------------------------------



Stream Status
----------------------------------------------------------------------------------------------------------------------------------
        Name                               Producers                                              Consumers
----------------------------------------------------------------------------------------------------------------------------------
       names                          cluster2(adapter)                                       cluster1(myApp)
----------------------------------------------------------------------------------------------------------------------------------

```




[MUSC13]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#MUSC13  "Gradle in Action"