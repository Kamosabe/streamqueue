# RabbitMQ

## Introduction

RabbitMQ is a open source message broker written in Erlang. 

It lets application share data via a common protocol (AMQP) or simply queue jobs for processing by distributed workers. It lets you decouple your applications and easily add more components (like workers). 

RabbitMQ is, except Qpid, the only message broker, which is based on the Advanced Message Queuing Protocol. 

### Advanced Message Queuing Protocol

The Advanced Message Queuing Protocol (AMQP) is a networking protocol that enables applications (clients) to connect to and communicate with messaging brokers. 

Messaging brokers receive messages from publishers / producer and route them, based on specific rules, to consumers. 

We can think of an messaging broker as a delivery service, focused on application to application messaging. 

A message broker acts like a router between applications. It decouples producer and consumer in such a way that both of them do not even have to know each other. 

Unlike TCP for example there is no exchange of (IP) addresses or routing information. 

### Messages
Messages published to a AQMP message broker consists, besides a handful of flags, of two parts: A payload (the data you want to send) and a label.
 
The payload is not interpreted nor changed by the broker and the format is up to the producer of the message. It is common to use serialization formats like JSON to serialize structured data in order to publish it as the message payload.
 
The label describes the payload and also may get interpreted by the broker to determine who should get a copy of the message.

The consumer in turn will then only receive the payload. No label. It is like receiving a letter wrapped in a blank envelope. 

### Hello World example
To gain an fast overview how a Java program, using RabbitMQ, may look like, we will walk through a "Hello World" example. 


#### Producer

	import java.io.IOException;
	
	import com.rabbitmq.client.Channel;
	import com.rabbitmq.client.Connection;
	import com.rabbitmq.client.ConnectionFactory;
	
	public class HelloWorldProducer
	{
	    private final static String QUEUE_NAME = "hello";
	
	    public static void main(String[] argv) throws IOException
	    {
			// create a connection and a channel to RabbitMQ server
			ConnectionFactory factory = new ConnectionFactory();
			factory.setHost("localhost");
			Connection connection = factory.newConnection();
			Channel channel = connection.createChannel();
		
			// declare a queue with name "hello"
			// this can to be done by producer or consumer
			channel.queueDeclare(QUEUE_NAME, false, false, false, null);
		
			// send the message to the RabbitMQ server
			String message = "Hello World!";
			channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
			System.out.println(" [x] Sent '" + message + "'");
		
			// close connection
			channel.close();
			connection.close();
	    }
	}


#### Consumer

	import com.rabbitmq.client.Channel;
	import com.rabbitmq.client.Connection;
	import com.rabbitmq.client.ConnectionFactory;
	import com.rabbitmq.client.QueueingConsumer;
	
	public class HelloWorldConsumer
	{
	    private final static String QUEUE_NAME = "hello";
	
	    public static void main(String[] argv) throws java.io.IOException, java.lang.InterruptedException
	    {
			// create a connection and a channel to RabbitMQ server
			ConnectionFactory factory = new ConnectionFactory();
			factory.setHost("localhost");
			Connection connection = factory.newConnection();
			Channel channel = connection.createChannel();
			
			// declare a queue with name "hello" 
			// this can to be done by producer or consumer
			channel.queueDeclare(QUEUE_NAME, false, false, false, null);
		
			// create a consumer and subscribe to queue
			QueueingConsumer consumer = new QueueingConsumer(channel);
			channel.basicConsume(QUEUE_NAME, true, consumer);
		
			System.out.println(" [*] Waiting for messages. To exit press CTRL+C");
		
			while (true)
			{
				// receive next message from queue
			    QueueingConsumer.Delivery delivery = consumer.nextDelivery();
			    String message = new String(delivery.getBody());
			    System.out.println(" [x] Received '" + message + "'");
			}
	    }
	}

Producer and Consumer first have to create a connection and channel to RabbitMQ Server. Through this connection and channel it is then possible for the clients to declare queues on the server and to send messages to and receive message from this queues.