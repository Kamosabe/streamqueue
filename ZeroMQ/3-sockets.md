## Sockets

In chapter 1 we were talking about sockets, messages, types of transports, messaging patterns and routing schemes. 

In this chapter we will discuss sockets and messages in more detail to get an better idea of what ZeroMQ really is and what it can do for us.

BSD Sockets are the de facto standard API for performing network programming. They present some kind of synchronous interface to either connection-oriented reliable byte streams (using TCP or SCTP) and connection-less unreliable datagrams (over UDP). 

Besides reliability stream sockets via TCP also ensures sending data sequenced and unduplicated.

Conventional Sockets allow only strict one-to-one and in some cases one-to-many or many-to-one relationships between endpoints, whereas ZMQ sockets allowing many-to-many relationships. Therefore it is possible that a ZMQ socket may be connected to multiple endpoints, sending messages to them and simultaneously accepting incoming connections from multiple different endpoints.

But there are more important differences between conventional sockets and ZMQ sockets. 

ZMQ sockets are asynchronous. Compared to the BSD sockets they are not blocking. The application can hand the message over to ZMQ and process further instead of waiting the message to be send via the socket. ZeroMQ will send the message in background. 

Furthermore it does not matter if there is a destination available right now or not. If not, ZMQ can enqueue the message and send it later when the receiver gets reconnected.


### Socket API

As we saw in the *HelloWorld* example of chapter 1 we need a context to create a socket: `context.socket(3)`. The numeric parameter here is the type of socket. Socket Types will be described in chapter 4 "Messaging Patterns".

#### Socket.bind()

With the `socket.bind(arg)` function it is possible to create an endpoint for accepting connections. The `arg` Parameter is a String consisting of two parts: *transport* and *address* separated by "://".
The *transport* part specifies the underlying transport protocol to use and the *address* part then depends on the transport protocol selected.

The following transports are defined:

- *inproc* - inter-thread communication transport
- *ipc* - inter-process communication transport
- *tcp* - unicast transport using TCP
- *pgm, epgm* - reliable multicast transport using PGM


Example:

	socket.bind("tcp://*:5555");
	socket.bind("ipc:///tmp/data");

#### Socket.connect()

Through `socket.connect(arg)` function we can connect the socket to a specific endpoint, defined by the arg Parameter. As for the `Socket.send()` function the `arg` Parameter is a String consisting of the two parts: *transport* and *address*.


Example:

	socket.connect("tcp://localhost:5555");
	socket.connect("ipc:///tmp/data");


#### Socket.send()

Calling the `socket.send(message, flags)` function instruct ZMQ to enqueue the message to later send it to the specified endpoint. Setting the `ZMQ_SENDMORE` flag indicates that the message is composed of 1 or more message parts. The number of message parts is unlimited and every each part is a independent Socket.send(). The last function call has to be send without the flag to tell ZMQ that this will be the last part of a multiparted message. 


Example:

	// sending a multipart message
	socket.send(messagePart1, ZMQ_SNDMORE);
	socket.send(messagePart2, ZMQ_SNDMORE);
	socket.send(messagePart3, 0);


#### Socket.recv()

`Socket.recv(flag)` shall receive a (multipart) message from the socket and return it as a array of bytes. If there are no messages available on the specified socket the `recv()` function will block. 

Example:

	 byte[] reply = socket.recv(0);


### Messages

As we were talking about messages all the time it would make sense to define what exactly is an message in the context of ZeroMQ. 

![Request - Response](images/ZMQ-string.PNG)


Conventional sockets are transferring data as streams of bytes or discrete datagrams, but ZeroMQ transfers messages. Messages are length-specified binary data without trailing 0 (not C style). They can be multiparted and must fit in memory. ZeroMQ guarantees to deliver all parts of an message or none of them. As ZeroMQ does not know anything about the data (except its size) the formatting is completely up to the developer of the application.

Multipart messages can be send ...

