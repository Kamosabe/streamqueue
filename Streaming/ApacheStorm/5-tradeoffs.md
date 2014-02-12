## Tradeoffs

1. Because storm nimbus is small enough and has a small working load it can restart fast. In case of a conflict it could be a point of failure.

2. If using a queuing system for example hashing queues in [Redis](http://redis.io/) or [Apache Kafka](http://kafka.apache.org/) as a middle ware to have more flexibility and transparency in a software service platform the queue can be a problem. Also there is a new layer in the process workflow.

3. Because each spout runs in a different thread it is better to use parallelism. The JVM will be used efficient. There is a rule: The more spouts the more memory must be allocated in the JVM.