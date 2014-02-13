##Example
This chapter shows a simple "Hello World" example for ActiveMQ. It uses the **message queue pattern**. It means point-to-point messaging. In this type of messaging a producer send a message to a queue. This message is persisted to provide a guarantee of delivery and some time later the messaging system delivers the message to a consumer. The consumer can process the message.

At first producer and consumer have to create a connection to the ActiveMQ server and create a queue. Over the queue producer and consumer can communicate with each other. 

####Producer

    import javax.jms.Connection;
    import javax.jms.DeliveryMode;
    import javax.jms.Destination;
    import javax.jms.JMSException;
    import javax.jms.MessageProducer;
    import javax.jms.Session;
    import javax.jms.TextMessage;
    import org.apache.activemq.ActiveMQConnectionFactory;
    
    public class HelloWorldProducer {

    	private final static String QUEUE_NAME = "hello";
    
    	public static void main(String[] args) throws JMSException {
    		// Create a Connection and Session to ActiveMQ server
    		ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory("vm://localhost");
    		Connection connection = connectionFactory.createConnection();
    		connection.start();
    		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    
    		// Create the destination (Queue)
    		Destination destination = session.createQueue(QUEUE_NAME);
    
    		// Create a MessageProducer from the Session to the Queue
    		MessageProducer producer = session.createProducer(destination);
    		producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
    
    		// Create and send a messages
    		String text = "Hello world!";
    		TextMessage message = session.createTextMessage(text);
    
    		// Tell the producer to send the message
    		producer.send(message);
    		System.out.println("Sent message: " + text);
    
    		// Clean up
    		session.close();
    		connection.close();
    	}
    }

####Consumer

    import javax.jms.Connection;
    import javax.jms.Destination;
    import javax.jms.JMSException;
    import javax.jms.Message;
    import javax.jms.MessageConsumer;
    import javax.jms.Session;
    import javax.jms.TextMessage;
    import org.apache.activemq.ActiveMQConnectionFactory;
    
    public class HelloWorldConsumer {
    
    	private final static String QUEUE_NAME = "helloWorld";
    
    	public static void main(String[] args) throws JMSException {
    		// Create a Connection and Session to ActiveMQ server
    		ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory("vm://localhost");
    		Connection connection = connectionFactory.createConnection();
    		connection.start();
    		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    
    		// Create the destination (Queue)
    		Destination destination = session.createQueue(QUEUE_NAME);
    
    		// Create a MessageConsumer from the Session to the Queue
    		MessageConsumer consumer = session.createConsumer(destination);
    
    		// Wait for a message
    		Message message = consumer.receive(1000);
    
    		if (message instanceof TextMessage) {
    			TextMessage textMessage = (TextMessage) message;
    			String text = textMessage.getText();
    			System.out.println("Received: " + text);
    		} else {
    			System.out.println("Received: " + message);
    		}
    
    		consumer.close();
    		session.close();
    		connection.close();
    	}
    }