## Messaging patterns

Messaging patterns are hard-coded rules, defining how ZeroMQ is routing and queuing messages between the applications. Message patterns are implemented by pairs of sockets and their types.
We will cover the three build-in core ZeroMQ pattern in the following sections.


### Request - Reply
The request-reply pattern connects a set of clients to a set of services, where a client sends a message to one or more servers and receive a reply for each message sent. The replies to the request have to be strictly in order.
The *HelloWorld* application in chapter 1 one is a typical example for the request-response pattern.

![Request - Response](images/reqres.png)

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

![Publish - Subscribe](images/publish-subscribe.png)



* must set subscription (filter) when you use SUB 
* A subscriber can connect to more than one publisher, using one connect call each time. Data will then arrive and be interleaved ("fair-queued") so that no single publisher drowns out the others.
* If a publisher has no connected subscribers, then it will simply drop all messages.
* If using TCP and a subscriber is slow, messages will queue up on the publisher.

### Pipeline
* parallel processing model
	* ventilator produces tasks that can be done in parallel
	* set of workers that processes tasks
	* sink that collects results back from workers 
* The workers connect upstream to the ventilator, and downstream to the sink.
* We have to synchronize the start of the batch with all workers being up and running.
* The ventilator's PUSH socket distributes tasks to workers evenly (load balancing).
* The sink's PULL socket collects results from workers evenly (fair-queuing).

![Divide and Conquer](images/divide-conquer.png) 





### Exlusive Pair

* 1:1 relation
* should only be used to connect via IPC


### Intermediaries and Proxies

#### Dynamic Discovery problem
ZMQ does not come with an message broker. So there is no single server in between, which routes and handles all these messsages. Normally this is a good thing cause all this transport is done in a decentralized way.

But there may be situations in real world, which make it necessary to have some *static* component in the middle of the application. For example when there are a lot of publisher and subscriber and we even want to add more dynamically, it gets harder and harder to remember all subscriber endpoints (protocol, port, maybe IP) to which we have to connect to.

![Discovery problem](images/discovery-problem.png)


That's why it is possible to add some kind of proxy (similar to a HTTP proxy) in a ZMQ application which acts as a static component to connect to and simply routes the messages from one endpoint to another one.

![Proxy](images/proxy.png) 

#### Shared Queue ... REQ / RES



#### ZMQ builtin proxy function