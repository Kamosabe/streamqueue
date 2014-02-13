##Example
This chapter shows a simple "Hello World" example for HornetQ. It uses the **message queue pattern**. It is the same used for ActiveMQ.

At first producer and consumer have to create a connection to the ActiveMQ server and create a queue. Over the queue producer and consumer can communicate with each other. 

####HelloWorldExample

    import java.util.Properties;
    import javax.jms.Connection;
    import javax.jms.ConnectionFactory;
    import javax.jms.MessageConsumer;
    import javax.jms.MessageProducer;
    import javax.jms.Queue;
    import javax.jms.Session;
    import javax.jms.TextMessage;
    import javax.naming.InitialContext;
    
    public class QueueExample {

        public static void main(String[] args) {
            Properties props = new Properties();
        	props.put("java.naming.factory.initial", "org.jnp.interfaces.NamingContextFactory");
        	props.put("java.naming.provider.url", "localhost");
        	props.put("java.naming.factory.url.pkgs", "org.jboss.naming:org.jnp.interfaces");
        	
            // Step 1. Create an initial context to perform the JNDI lookup.
            InitialContext initialContext = new InitialContext(props);
            
            // Step 2. Perfom a lookup on the queue
            Queue queue = (Queue) initialContext.lookup("/queue/exampleQueue");
            
            // Step 3. Perform a lookup on the Connection Factory
            ConnectionFactory cf = (ConnectionFactory) initialContext.lookup("/ConnectionFactory");
            
            // Step 4.Create a JMS Connection
            connection = cf.createConnection();
            
            // Step 5. Create a JMS Session
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            
            // Step 6. Create a JMS Message Producer
            MessageProducer producer = session.createProducer(queue);
            
            // Step 7. Create a Text Message
            TextMessage message = session.createTextMessage("This is a text message");
            
            System.out.println("Sent message: " + message.getText());
            
            // Step 8. Send the Message
            producer.send(message);
            
            // Step 9. Create a JMS Message Consumer
            MessageConsumer messageConsumer = session.createConsumer(queue);
            
            // Step 10. Start the Connection
            connection.start();
            
            // Step 11. Receive the message
            TextMessage messageReceived = (TextMessage) messageConsumer.receive(5000);
            
            System.out.println("Received message: " + messageReceived.getText());
        }
    }