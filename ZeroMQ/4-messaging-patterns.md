## Messaging patterns

Messaging patterns are hard-coded rules, defining how ZeroMQ is routing and queuing messages between the applications. Message patterns are implemented by pairs of sockets and their types.
We will cover the three build-in core ZeroMQ pattern in the following sections.


### Request - Reply
The request-reply pattern connects a set of clients to a set of services, where a client sends a message to one or more servers and receive a reply for each message sent. The replies to the request have to be strictly in order.
The *HelloWorld* application in chapter 1 one is a typical example for the request-response pattern.


![Request - Response](images/zmq-req-rep-2.png)

#### Request
A client uses a socket of type `ZMQ_REP` for sending messages to and receiving replies from a server. If the socket is connected to more than one server the messages are sent with the round-robin routing strategy. Sockets of type `ZMQ_REP` does not throw any messages away. If there are no services / server to send the message, all send operations are blocked until a service comes available.  

	ZMQ.Context ctx = ZMQ.context(1);
	ZMQ.Socket request = ctx.socket(ZMQ.REQ);
	request.connect("tcp://localhost:5555");


#### Reply
A server uses a socket of type `ZMQ_REP` to receive messages from and send replies to clients. If the connection between a client and the server is lost, the replied message is thrown away. In case there is more than one client connected the incoming routing strategy is fair-queue. The round-robin scheduling is the simplest way of implementing the fair-queue strategy.

	ZMQ.Context ctx = ZMQ.context(1);
	ZMQ.Socket respond = context.socket(ZMQ.REP);
	respond.bind("tcp://*:5555");


### Publish - Subscribe
The publish-subscribe pattern a one-to-many model where a publisher sends a message to a set of connected subscribers. This is a one-way data distribution pattern. It is similar to TV broadcasting, where only the viewers who turn on the specific channel are receiving the related information. That means the publisher do not care if there is any subscriber and only connected subscriber are receiving messages, whereas the others will miss them.

![Publish - Subscribe](images/zmq-pub-sub-2.png)


#### Publisher
A socket of type `ZMQ_SUB` is used by a publisher to distribute data. A message will be delivered from the publisher to all its connected subscriber (fan-out). If an subscriber is in some kind of exceptional state and cannot receive a message in this moment the message will be dropped. If there is not a single subscriber for the publisher the messages will be simply dropped.

	ZMQ.Context context = ZMQ.context(1);
	ZMQ.Socket publisher = context.socket(ZMQ.PUB);
	publisher.bind("tcp://*:5556");


#### Subscriber
A socket of type `ZMQ_SUB` is used by a subscriber to subscribe to a publisher. A subscriber can conncet to more than one publisher, using one connect call each time. Data will then arrive and be interleaved ("fair-queued") so that no single publisher drowns out the others. One important thing to mention is, that the subscriber has to set a filter while subscribing to a publisher otherwise he will not receive any message. It is possible to set multiple filters.


	ZMQ.Context context = ZMQ.context(1);
	ZMQ.Socket subscriber = context.socket(ZMQ.SUB);
	subscriber.connect("tcp://localhost:5556");
	String filter = "test";
	subscriber.subscribe(filter.getBytes());

### Pipeline
The pipeline pattern is used for distributing data to nodes arranged in a pipeline. Parallel processing of data can be done using this pattern. 

![Pipeline](images/zmq-pipeline-2.png) 

Let's think of a scenario where a node has to process a huge task which can be divided into smaller pieces. The smaller tasks can then be pushed to fast worker nodes, who process these tasks in parallel. At the end every worker then pushes it's result to a collector node.

In this scenario the workers connect upstream to the Ventilator (Producer) Node and downstream to the sink (Collector Node). The tasks will be distributed evenly by the ventilators PUSH socket to the workers (load balancing). The sink's PULL socket collects the results from workers evenly (fair-queuing). 

Here is full cod example of the pipeline pattern to demonstrate the usage of `PUSH`and `PULL`sockets and to warp up everything we learned so far:

TaskVentilator.java:

	import java.util.Random;
	import org.zeromq.ZMQ;
	
	public class TaskVentilator 
	{
	    public static void main (String[] args) throws Exception 
		{
	        ZMQ.Context context = ZMQ.context(1);
	
	        //  Socket to send messages to workers
	        ZMQ.Socket workerSocket = context.socket(ZMQ.PUSH);
	        workerSocket.bind("tcp://*:5557");
	
	        //  Initialize random number generator
	        Random r = new Random(System.currentTimeMillis());
	
	        //  Send 100 tasks
	        for (int i = 0; i < 100; i++) 
			{
	            int workload = r.nextInt(100) + 1;
	            String string = String.format("%d", workload);
	            workerSocket.send(string, 0);
	        }
			
			//  Give 0MQ time to deliver
	        Thread.sleep(1000);              
	
	        sink.close();
	        sender.close();
	        context.term();
	    }
	}


TaskWorker.java:

	import org.zeromq.ZMQ;

	public class TaskWorker 
	{
	    public static void main (String[] args) throws Exception 	
		{
	        ZMQ.Context context = ZMQ.context(1);
	
	        //  Socket to receive messages from ventilator
	        ZMQ.Socket ventilatorSocket = context.socket(ZMQ.PULL);
	        ventilatorSocket.connect("tcp://localhost:5557");
	
	        //  Socket to send messages to sink
	        ZMQ.Socket sinkSocket = context.socket(ZMQ.PUSH);
	        sinkSocket.connect("tcp://localhost:5558");
	
	        while (Thread.currentThread ().isInterrupted () == false) 
			{
	            String string = new String(ventilatorSocket.recv(0)).trim();
	            
				long msec = Long.parseLong(string);
	
	            //  Do the 'work'
	            Thread.sleep(msec);
	
	            //  Send results to sink
	            sinkSocket.send("result".getBytes(), 0);
	        }
	        ventilatorSocket.close();
	        sinkSocket.close();
	        context.term();
	    }
	}

TaskSink.java:

	import org.zeromq.ZMQ;
	
	public class TaskSink 
	{
	    public static void main (String[] args) throws Exception 
		{
	        ZMQ.Context context = ZMQ.context(1);

	        ZMQ.Socket workerSocket = context.socket(ZMQ.PULL);
	        workerSocket.bind("tcp://*:5558");
	
	        //  Process 100 results from workers
	        for (int i = 0; i < 100; i++) 
			{
				String result = new String(workerSocket.recv(0)).trim();

				// doing something 'useful' with the result
				System.out.println("result of worker " + i + ": " + result);
	        }
	        receiver.close();
	        context.term();
	    }
	}