### Overview

The first component of Kafka is the "Kafka server" or "Kafka broker". And this is the first server that users first interact with to accept connections. Network applications typically all go with this pairing of having a server and listening in on a port (the default port for Kafka: 9092).

Producer -> Kafka broker -> Consumer

### Two abstractions:

*First abstraction: producers and consumers*
Producers produce content, it publishes content to the broker
Consumer consumes content from the broker.

*Second abstraction: connections *
The Producer is connected to the Kafka broker via a TCP connection. Raw TCP connection so it's bi-directional so broker can send information to the producer and the producer can send information to the broker.

The consumer then establishes this TCP connection, there is a protocol (not sure what this protocol is) - may be custom binary TCP. So once you establish these TCP connections, here's the concept.

### Topics
Topics are basically these logical partitions where you write contents to. So  it is logical partioning of data.

When the producer writes, it has to specify which topic to write to. So if I want to write message A to topic A, example "hello". Consumer goes, hey, I want to consume Topic B.

The broker will send the messages to the consumer.

#### Users Topic
My Kafka broker has Users Topic

Example.
Producer publishes "John" to users topic.
Producer establishes a connection right now then we send a request and that request says hey broker, publish "John" the string "John" to the Users topic.

Just take that string and then appends it to the topic.

"appending" in Kafka is very important.

Can always APPEND, can never DELETE/REMOVE stuff.

Producer wants to publish something else. Publish "Ed" to Users topic.

Appends word Ed to the Users Topic. Each message is referred to by essentially the topic and the position. The position is very fast access. (0 is John, 1 is Ed with indexing).

Sequential.

Consumer now decides it wants to consume the Users topic. Consumer will read from the beginning, of index 0, then index 1. Consumer is polling from the topic (not a pushing model.). Consumer asks for more.

The broker pushes the information to the consumer.

Topics grow large.
### What happens when a database grows too large?

We shard them.

Example - consumers from number 1- 100,000 use this table and this database. And the customers from number 100001 to 200,000 use another table and this database.

Based on the index/customer you are looking for, you will know which database to search for that customer.

We want to distribute the data because queries get slower and slower if the data is large. 

So now we want to shrink it up.

#### Partitions
We're going to partition the data. So let's we create two partitions.

Partition 1 will have users first names A-M, Partition 2 from N-Z. These can grow independently.

But now the Producer and Consumers need to understand what partitions are. They need to know what topics to read and write from, they also need to know what partitions to write and read from.

Example:

So now Producer wants to publish "Nader". They have to write to Users topic on Partition 2. When it writes the new data, it has a new position.

For example, position 0.

Consumer wants to consume from the User topic on Partition 2, Position 0. 

Consumers only work with positions and topics. It's not a relational database, we use it for fast writing and distributed of events. These are the benefits.

### Queue vs Pub Sub
Queue: Message published once, consumed once.
* message gets written to the queue, consumer pops it from the queue, no one else can access that again.
- everything happens only once, you don't want someone else executing the same task twice.
- queues are great for just executing tasks once.

Pub Subs: Message published once, consumed many times.
* I want a message published once, but I want this to be consumed many times.
* I want to just broadcast this message. 
Example:
YouTube, publish a video, and you want it to be consumed by multiple services - could be the compress service, another one take the raw video make it through multiple resolutions, and encoding it different formats, copyright service to check if copyright id, etc.

**Kafka came in and asked --- how can we do both?**

They designed with this in mind. The answer was: **Consumer Group**

### Consumer Group

* Invented to do parallel processing on partitions. 
* Consumer can read from partitions, but consumer group removes its awareness of partitions.
* Can consume parallel data from multiple partitions.
* Each partition can only have one consumer
* Consumer can consume as many partitions
* Consumer group verifies the rules above.
* Each consumer is its own individual processes, which mean you can consume those partitions in parallel.
* The system of those consumers consuming those partitions in parallel they act like a queue.

Example:
1. You have a Consumer Group called Group 1. 
2. You create a new consumer, Consumer 1, and you join the consumer group. 
3. If you are the only consumer in this Group, you are responsible for all the partitions in that Users topic. Any time you start consuming, you will get a message from each partition it is responsible for.
4. You create another consumer, Consumer 2 
5. Consumer 2 sees that consumer 1 is responsible for example, 2 partitions, so you take on one of the partitions and now Consumer 1 is responsible for partition 1, and Consumer 2 is now responsible for partition 2. 
6. If there are only 2 partitions, a new consumer cannot join the group since there is no work, there are no other partitions.


Example:
Consumer 1, Partition 1.
It reads position 0 moving to position N.

Consumer 2, Partition 2.
It reads position 0 moving to position M.

The moment you read the message from the position, it's like reading it from the queue in Partition 1.

Consumer 2 will never be able to read off of Partition 1. So we can read a queue from Partition 2.

* To act like a queue, put all your consumers in one group
* Each consumer will be responsible for one partition, and it will not be seen by any other consumers in that group.
* Message when read it will be gone/committed as read. Still in the system however.
* To act like a queue, put all your consumers in one group
* To act like a pub/sub system where the messages are broadcasted to every consumer, put each consumer in a *unique* group.
* Partition can be consumed by consumers in multiple Consumer Group. (Consumer 1 from Consumer Group 1, Consumer 1 from Consumer Group 2, Consumer 1 from Consumer Group 3, etc)
* As a result you get parallel processing.

## Distributed System
We make a distributed system by taking a Kafka broker, and copy it, make it a leader/follower system. Where we have one leader where it takes all the requests, and one follower where it reads from the leader, or a master follower kind of configuration.

Example:
1. Copy Kafka broker on port 9092 and make another Kafka broker on port 9093.
2. Copy the Users Topic to new broker.
3. Copy the Partition 1 to new broker.
4. Copy the Partition 2 to new broker.

Question:
How do we know which broker is the leader, which one is the follower?

In Kafka, we don't want a leader/follower/broker at on the final level. I want the follower/leader on the *partition* level.

Example:
I want Partition 1 from Kafka broker 1 to be the leader, and the Partition 2 from Kafka broker 1 to be the follower
So I will have Partition 1 from Kafka broker 2 to be the follower, and the Partition 2 from Kafka broker 2 to be the leader.
Kafka broker 2 Partition 2 leader
Kafka broker 1 Partition 1 leader

Where will this information stored?....

### Zookeeper
(Controversial) A lot of people do not like this technology. Zookeeper herds the cat, and says "YOU are the leader", "FOLLOW the leader."

Example:
How do we produce in this configuration where we have Zookeeper?

When you connect for the bidirectional connect - before you submit, you need to ask who is the partition leader of the thing I want to write to. Say the Producer wants to write "Zain" to topic "Users" on Partition 2. But we need to know which one is the leader. I have no idea who is the leader.

(Author is not sure about if the producer is aware of who is the partition leader, because you have to write to the leader, not the follower, so is the producer aware)

Is it - any broker that I'm connected to, please write this message to Partition 2, and the brokers to determine which one is the leader to take that message to write to the leader

or

do the producers know, query the brokers for it to tell us which broker/partition has the leader.

Zookeeper gossip between the other to figure out who the gossip is the leader. And then the Broker on 9093 says it will be me.

Then it writes the message to the partition, and the follower copies it.

**What about consuming?**

I want to consume topic "Users" Partition 1. So the consumer can just read. You can either read it from the leader or a follower, and it depends on the Zookeeper.

## Code Example
- Spin up zookeeper
- Spin up kafka cluster (single)
- Create Topic node js (kafkajs)
- Create Producer node js (kafkajs)
- Create Consumer node js (kafkajs)

1. Spin up Zookeeper (and expose the port 2181 to the same port on my machine)
`docker run --name zookeeper -p 2181:2181 zookeeper`
2. Spin up Kafka (and expose 9092) with Zookeeper and expose the Kafka broker what your address is. Kafka supports SSL and plain text protocol. We need KAFKA similar terminology. text is HTTP, the other is HTTPS. Kafka default/Zookeeper starts up three instances. It will assume you have three instances. Then you have to force how many instances you want bThTy the KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=num_of_instances
`docker run --name kafka -p 9092:9092 -e KAFKA_ZOOKEEPER_CONNECT=ip_address_of_machine:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEST://ip_address_of_machine:9092 -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 confluentinc/cp-kafka`
3. Initialize node js workspace: `npm init -y`
3. Install kafkajs:  `npm install kafkajs`

# Kafka Pros
- Append only Commit log
	- heart of Kafka
	- always know where the end; appends at the end is quick
	- no manipulation of data
	- write once, read many
- Performance
	- You're done once you have written
	- Seeking with a position is extremely fast.
	- Read fast, write fast. It is designed for events
- Distributed
	- With zookeeper, the concept of partitions with the append commit logs, you can have one leader partition and the rest of the partitions will follow that.
	- Because of the design with it being sharded, can easily distribute these around, and easily scale as a result
- Long polling
	- Kafka does not use a push model. Limitations of a push model is that the consume cannot consume as fast as producers most of the time. (Think youtube videos)
	- Let's do a polling model where the consumer polls for a message. "Hey is there a message?" and "Don't respond if there isn't any" - wait for X amount of time, and by the way if at least there are X # of messages of X amount of bytes or data available to me ("consumer"), push it to me ("consumer"). There are no empty missed requests - only respond when there is something available for me("consumer")
	- Consumer starts doing the long polling behind the scenes. 
- Event-driven architecture, Pub sub and Queue
	- It's a queue because you can put all your consumers in one group 
	- each consumer group will only receive one message by one consumer, and will not be received by any other consumer
	- Can be pub sub - hey I uploaded a video, and lots of other services can consume the same message. Can be sent to multiple consumers by use of consumer group.
	- Event-driven (micro-services with Kafka). Kafka uses naturally with micro services. Instead of talking to one another, it is more like an event driven. Like "hey, a video has just been uploaded." "a video has just been copyrighted" "hey a video has just been encoded", "a video is ready to be edited". One can store these events, and then one can just listen to these event changes as a consumer. When something happens -> do this as a result instead of having to deal with that logic yourself.
	- Scaling
	- Parallel Processing
		- consumers like there is no tomorrow because all the consumers will hit those partitions in parallel.
		- multiple partitions can read them in parallel
		- due to sharding of the data/partitions, this allows them to read in parallel
		- as a producer, you need to know which partition to write to.

# Kafka Cons
- Zookeeper
	- Kafka may remove zookeeper as a dependency
	- Engineers complain about this - on scale, this behaves strange on scale
	- If it's down, many things can have issues
- Producer explicit partition can lead to problems (they have to have the knowledge of knowing where to write to) which complicates the client
	- Vitesse abstracts the sharding from the client
- Complex to install, configure and manage (took the author a whole day to figure this out)