## APIs
The following chapter describes supported APIs which are important to know. The already known JMS API is not mentioned here again. Its quite common to support it. 

###Embedded Broker
In Chapter 2 the installation of ActiveMQ is focused on the standalone middleware component, but ActiveMQ supports communication with embedded brokers too. Being a Java application itself, ActiveMQ could be naturally integrated in other Java applications. 

To use ActiveMQ in native Java you have to use:

    org.apache.activemq.broker.BrokerService

The following simple code example shows a simple configuration of the broker:

    BrokerService broker = new BrokerService();
	broker.addConnector("tcp://localhost:61616");
	broker.start();
	
The broker supports the complete configuration in pure Java. On default it is set up in a XML configuration file. The code example above would be described by the following XML:

    <broker xmlns="http://activemq.org/config/1.0" brokerName="localhost" dataDirectory="${activemq.base}/data">
        <transportConnectors>
            <transportConnector name="openwire" uri="tcp://localhost:61616" />
        </transportConnectors>
    </broker>

Using the embedded broker you don't need any XML configuration and could fully initialize the broker with using just plain Java code. 


----------


###Connecting to ActiveMQ with other languages
Next to the usage in a Java environment it is possible to use ActiveMQ as a general messaging solution for a variety of development platforms. 

####STOMP
ActiveMQ supports Stomp (Streaming Text Orientated Messaging Protocol) as communication protocol. It is a simple text-orientated protocol and plays an important role in messaging for scripting languages (e.g. Ruby, PHP, Perl) because it is easy to implement a Stomp client in an arbitrary programming language.

####.Net Message Service API (NMS)
.Net Message Service API is a subproject of ActiveMQ and provides a standard C# interface to messaging systems. The idea is to create a unified messaging API for C# similar to  JMS in Java.

####C++ Messaging Service API (CMS)
C++ Messing Service API is also an ActiveMQ subproject. The idea behind is the same as the NMS API. It represents a standard C++ interface for communcating with messaging systems. 

####REST API
ActiveMQ also supports messaging via REST. It comes with an embedded Web server. The server is used to provide all necessary Web infrastructure for the ActiveMQ broker and is part of the broker. 
The API is implemented by:

    org.apache.activemq.web.MessageServlet


----------


###Administration and Monitoring
In addition to messaging client applications it's often necessary to monitor the broker. For this purpose ActiveMQ provides some possibilities to manage and monitor the broker.

####JMX
JMX (Java Management Extensions) is the most common for monitoring in the Java world. For that reason ActiveMQ supports JMX as well. To enable JMX support in ActiveMQ it's still necessary to expand the XML configuration with the following:

    <managementContext>
        <managementContext connectorPort="2011" jmxDomainName="my-broker"/>
    </managementContext>

####Advisory Messages
Advisory Messages are a mechanism to provide monitoring by using the JMS API. It can be used to notify messaging clients about important broker events. Advisory messages are send to topics to publish events to all subscribed clients. The used prefix is:

    ActiveMQ.Advisory

