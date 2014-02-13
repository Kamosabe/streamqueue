## AMQP Advanced

### Persistence

Per default queues, exchanges and therefore also messages will not survive a reboot or a crash of RabbitMQ server.

To make a message survive a crash of the AMQP broker we have to follow the following steps:

- The message has to be flagged as *persistent* by setting its delivery mode to 2.
- message must be published to an exchange that is *durable*
- message has to arrive in a queue that is *durable*

We can declare an durable exchange by setting the `durable` parameter of `exchangeDeclare()` method to `true`:
	
	String exchangeName = "nameOfExchange";
	String exchangeType = "topic";
	boolean durable = true;
	channel.exchangeDeclare(exchangeName, exchangeType, durable); 

We can declare an durable queue by setting the `durable` parameter of `queueDeclare()`method to `true`:

	String queueName = "nameOfQueue";
	boolean durable = true;
	channel.queueDeclare(queuName, durable, false, false, null); 

One could ask, why not always make queues and exchange durable and messages persistent to be save if something goes wrong.

But there is a drawback: Performance. RabbitMQ will achieve persistence by writing message to disk and this can be an time consuming task.

### Virtual Hosts
To make it possible for a single broker to host multiple isolated environments (own queues, exchanges, bindings, permissions) the AMQP protocol offers us the concept of virtual hosts (vhosts).

Actually while starting a RabbitMQ server, one virtual host is created right away per default.

To add or remove vhosts from the server we simple can use the command line tool `rabbitmqctl` from the RabbitMQ installation.

Creating vhost "newvhost":

![AMQP connection](images/create-vhost.PNG)


Deleting vhost "newvhost":

![AMQP connection](images/delete-vhost.PNG)

AMQP clients specify what vhost they want to use while creating a connection.

	ConnectionFactory factory = new ConnectionFactory();
	factory.setVirtualHost(virtualHost);
	Connection conn = factory.newConnection();
