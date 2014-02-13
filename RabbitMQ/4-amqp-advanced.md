## AMQP Advanced



### Virtual Hosts
* mini-RabbitMQ server with own
	* queues
	* exchanges
	* bindings
	* permissions
* safely use one RabbitMQ server for multiple applications
* minimum one vhost per server (default)
* rabbitmqctl add_vhost [name]
* rabbitmqctl delete_vhost [name] 

### Persistence
* normally queues and exchanges and therefore also messages will not survive reboot of RabbitMQ server
* to make a message survive a crash of the AMQP broker you have to follow some steps:
	* flag message as persistent by setting delivery mode to 2 (persistent)  
	* message must be published to an exchange that*s durable and
	* message arrive in queue that is durable
* RabbitMQ will achieve persistence by writing message to disk
* price: performance