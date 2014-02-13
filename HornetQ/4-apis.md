## APIs
The following chapter describes supported APIs which are important to know. The already known JMS API is not mentioned here again. Its quite common to support it. 

###Core API
The HornetQ server itself does not use JMS. It always deals with core API interactions. For that reason HornetQ provides a simple intuitive Java API that allows the full set of messaging functionality without some of the complexities of JMS. The standard JMS API is only available on the client side but it is also possible to use the core API directly. 

An illustration of this relationship is shown below. User Application 1 is using the JMS API, while User Application 2 is using the core client API directly.

![Figure Core API Architecture](images/hornetq_core_architecture.jpg)

----------

###Embedded Server
Because HornetQ is designed as a set of simple POJOs it is possible to use a HornetQ server directly in Java code and embed it in a Java application.

To instantiate a **core HornetQ server** only, it's necessary to use:

    org.hornetq.core.server.embedded.EmbeddedHornetQ
    
To use the **JMS API** HornetQ provides:

    org.hornetq.jms.server.embedded.EmbeddedJMS

Using the embedded server allows you to replace all XML configuration with plain Java code.

----------

###Connecting to HornetQ with other languages
Next to the usage in a Java environment it is possible to use HornetQ for different development platforms.

####STOMP
HornetQ provides native STOMP support. It is a simple text-orientated protocol and plays an important role in messaging for scripting languages (e.g. Ruby, PHP, Perl) because it is easy to implement a Stomp client in an arbitrary programming language.
 
####REST API
HornetQ provides a REST interface. It depends on the RESTEasy project and can currently only run within a servlet container (e.g. Apache Tomcat).

----------

###Administration and Monitoring
HornetQ has an extensive management API. It allows a user to modify configurations and manage resources. There are three ways to manage HornetQ which are described in the following.

####JMX
The HornetQ management API can be accessed using JMX. JMX (Java Management Extensions) is the most common for monitoring in the Java world. It is enabled by default. You can enable/disable it and configure the used domain with the following XML code in the configuration file:

    <!-- false to disable JMX management for HornetQ -->
    <jmx-management-enabled>true</jmx-management-enabled>
    
    <!-- use a specific JMX domain for HornetQ MBeans -->
    <jmx-domain>my.org.hornetq</jmx-domain>

####Core API
The complete management API is also accessible using the core API. It is only necessary to send regular core message with well-known properties:

 - the name of the managed resource 
 - the name of the management operation
 - the parameters of the management operation

The following example show how to find out the number of messages in the core queue <code>exampleQueue</code>:

    ClientSession session = ...
    ClientRequestor requestor = new ClientRequestor(session, "jms.queue.hornetq.management");
    ClientMessage message = session.createMessage(false);
    ManagementHelper.putAttribute(message, "core.queue.exampleQueue", "messageCount");
    session.start();
    ClientMessage reply = requestor.request(m);
    int count = (Integer) ManagementHelper.getResult(reply);

####JMS API
The management API is also available for standard JMS connection. It is nearly the same as the Core API works. The JMS 1.1 API does not define a management api but the standard mechanisms already allows such features. 

Instead of using the <code>ClientRequestor</code> of the core API it's necessary to use a <code>QueueRequestor</code> with the special management queue:

    Queue managementQueue = HornetQJMSClient.createQueue("hornetq.management");

####Management notifications
It is also possible to receive notifications when things happen. It is called management notifications. All three APIs described above can be configured to use this feature:

 - JMX notifications 
 - Core messages 
 - JMS messages