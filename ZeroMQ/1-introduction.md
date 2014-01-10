# ZeroMQ


## introduction
* not a traditional message queue
* looks like an embeddable networking library but acts like a concurrency framework
* gives sockets that carry atomic messages across various transports like in-process, inter-process, TCP, multicast.
* Zero stands for: zero broker, zero latency, zero administration, zero cost, zero waste: culture of minimalism.
* "how to connect any code to any code, anywhere"
* ZMQ applications always start by creating a *context*, and then using that for creating sockets.
* messages are just strings, bytes
* Traditional network programming is built on the general assumption that one socket talks to one connection, one peer. 
* ØMQ sockets are doorways to fast little background communications engines that manage a whole set of connections automagically. 
* Whether using blocking send or receive or poll, all you can talk to is the socket, not the connections it manages

### advantages
* It delivers blobs of data (messages) to nodes, quickly and efficiently. 
* nodes can be mapped to threads, processes, or nodes. 
* it gives any application a single socket API to work with, no matter what the actual transport
is (e.g., in-process, inter-process, TCP, or multicast). 
* it automatically reconnects to peers as they come and go. 
* it queues messages at both sender and receiver, as needed. 
* it manages these queues carefully to ensure processes don’t run out of memory, over‐flowing to disk when appropriate.
* It handles socket errors. 
* It does all I/O in background threads. 
* It uses lock-free techniques for talking between nodes, so there are never locks, waits, semaphores, or deadlocks.

### disadvantages