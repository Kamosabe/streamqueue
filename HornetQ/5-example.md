##Example
This chapter shows a simple "Hello World" example for HornetQ. It uses the **message queue pattern**. It is the same as used for ActiveMQ.

At first producer and consumer have to create a connection to the HornetQ server.  The command <code>session.createQueue("ExpiryQueue")</code> does not really create a queue it only does a lookup. The queue itself has to be configured in the server's configuration file. The queue <code>ExpiryQueue</code> is configured on default. 

Over the queue producer and consumer can communicate with each other. 

####HelloWorldExample

    import java.util.Properties;
    import javax.jms.Connection;
    import javax.jms.ConnectionFactory;
    import javax.jms.JMSException;
    import javax.jms.MessageConsumer;
    import javax.jms.MessageProducer;
    import javax.jms.Queue;
    import javax.jms.Session;
    import javax.jms.TextMessage;
    import javax.naming.Context;
    import javax.naming.InitialContext;
    import javax.naming.NamingException;
    
    public class QueueExample {
    
    	public static void main(final String[] args) throws JMSException, NamingException {
    		// Step 1. Create an initial context to perform the JNDI lookup.
    		Properties props = new Properties();
    		props.put(Context.PROVIDER_URL, "jnp://localhost:1099");
    		props.put(Context.INITIAL_CONTEXT_FACTORY, "org.jnp.interfaces.NamingContextFactory");
    		props.put(Context.URL_PKG_PREFIXES,	"org.jboss.naming:org.jnp.interfaces");
    		Context ctx = new InitialContext(props);
    
    		// Step 2. Lookup the connection factory
    		ConnectionFactory cf = (ConnectionFactory) ctx.lookup("/ConnectionFactory");
    
    		// Step 3. Create a JMS Connection
    		Connection connection = cf.createConnection();
    
    		// Create a JMS Session
    		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    
    		// Create a JMS queue
    		Queue queue = session.createQueue("ExpiryQueue");
    
    		// Create a JMS Message Producer
    		MessageProducer producer = session.createProducer(queue);
    
    		// Create a Text Message
    		TextMessage message = session.createTextMessage("Hello World");
    
    		// Send the Message
    		producer.send(message);
    		System.out.println("Sent message: " + message.getText());
    
    		// Create a JMS Message Consumer
    		MessageConsumer messageConsumer = session.createConsumer(queue);
    
    		// Start the Connection
    		connection.start();
    
    		// Receive the message
    		TextMessage messageReceived = (TextMessage) messageConsumer.receive(5000);
    		System.out.println("Received message: " + messageReceived.getText());
    	}
    }