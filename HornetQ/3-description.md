## Description

###General
HornetQ is the successor of JBoss Messaging 1.x. The original development name was JBoss Messaging 2. Because of the huge differences on code base between HornetQ and JBoss Messaging 1.x the decision for a new name was made. 

The big advantage of HornetQ is that it is designed for large messages (several GB) and queues (more than one TB). Of course it could be easily integrated in JBoss-Components but it is also flexible usable with general java components. 


----------


###Features
HornetQ has a lot of features. The most common functions are described in the following.

####JMS
HornetQ provides a fully JMS 1.1 (Java Message Service) compliant API. The JMS standard provides many benefits and guarantees for message-oriented communication.

####Core API
HornetQ has its own client side API which can be used instead of JMS. The HornetQ core API provides all the power of JMS without some of its complexity and more features. 

####Integration
HornetQ is designed as a set of Plain old Java Objects (POJOs) without any third party dependencies. It only requires standard JDK classes. Therefore its very easy to run it in JBoss Microcontainer, Spring or embedded in any third party product. 

####Performance
HornetQ provides message persistence using its own built-in, high performance journal. It uses automatically Linux Asynchronous IO (AIO) or as fall back Java NIO to run on any Java platform.

HornetQ can send and receive multi-gigabyte messages even the server is running with only 50MB of RAM. The developers tested messages up to 8GB.