## Description



### Queuing patterns

#### Request - Response
* creating ZMQ context
* creating socket
* comparable to classic client server model and rpc

![Request - Response](/images/reqres.png)


#### Publish - Subscribe
* one way data distribution (one server pushes updates to a set of clients)
* must set subscription (filter) when you use SUB
* The PUB-SUB socket pair is asynchronous -> subscriber may fail first messages cause it takes some time to connect against the publisher (e.g. cause of tcp handshake) 
* A subscriber can connect to more than one publisher, using one connect call each time. Data will then arrive and be interleaved ("fair-queued") so that no single publisher drowns out the others.
* If a publisher has no connected subscribers, then it will simply drop all messages.
* If you're using TCP and a subscriber is slow, messages will queue up on the publisher.

![Publish - Subscribe](/images/publish-subscribe.png)

#### Push / Pull
* parallel processing model
	* ventilator produces tasks that can be done in parallel
	* set of workers that processes tasks
	* sink that collects results back from workers 
* The workers connect upstream to the ventilator, and downstream to the sink.
* We have to synchronize the start of the batch with all workers being up and running.
* The ventilator's PUSH socket distributes tasks to workers evenly (load balancing).
* The sink's PULL socket collects results from workers evenly (fair-queuing).

![Divide and Conquer](/images/divide-conquer.png) 
