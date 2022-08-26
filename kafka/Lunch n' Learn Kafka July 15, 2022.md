We have clusters
clusters filled with brokers
Each of those brokers will have topics on it

Topics are logical collection where we send payloads to.
Each topics contain several partitions, in theory can have one partition, but not very useful.
We have about 128 partitions and go up to MaxInt. Each partition is assigned to a broker and they are the leader, kind of source of truth partition.

Each broker has partitions on it, and they are replicas.
Producer writes data onto our partition.
Then consumers will connect to our partitions from partifular brokers. They will consume from the leader and never the replicas (not 100% confident in that)

Topics and Partitions
Topics are there for humans. When you get down to the nitty gritty, in most cases all they care about are partitions.
Under the hood we are subscribed to partitions under topics and we are subscribing to multiple partitions on a specific topic even though we don't know that's happening.

Producers:
Must specify a topic
can specify a partition
Can specify a key
Messages will be committed to a single partition in the destination topic
We don't specify a partition or a key at Redox

Producer - Configurations
acks - which specifies when a payload is considered "committed"
-1 - is what Redox sets which means we publish data to the Leader and it gets published to all replicas - but "all" doesn't necessarily meant to all. All can be set to a number. In Redox, that is set to 2.

Compression Algorithm

Random Notes

Consumers
Kafka consumers can consume from replicas if they are rack aware, at Redox they are not. Our consumers consume from partition leaders.

In Kafka, replication factor includes leader + repicas in the count. In the elastic search world, replica factor just counts the replicas, not the leaders.

Consumer Groups
In order to consume data from topics, they need to be part of a consumer group. Make sure there are not multiple consumers working on the same work. Number of partitions one group is subscribed to is proportional to scaling.

Most of our consumer groups have one topic.

It's a pain in the butt to increase the # of partitions. It is impossible to remove a partition.

Partitions are the FIFO unit, but we don't use any of the FIFO unit with Kafka. As soon as you change your partition count, but that hash # modifies on the partition count, and that could change the FIFO in terms of where that data comes. But we don't use the FIFO here at Redox which makes it easier.

Data does not get rebalanced when a new partition is added.

Consumer Groups - Fetch Configurations

The consumer sends a fetch request to the broker with minBytes, maxWaitTimeinMs, maxBytes.

Kafka will wait for minBytes amount of data to build up before responding to a fetch

Kafka will wait maxWaitTimeInMs to reach minBytes before responding anyway.

Kafka will stop waiting for data once it reaches maxBytes when responding to a fetch.

Most of these aren't set and the defaults are set.
There is no way to control the quantity of payloads returned per fetch.
If there is a payload larger than maxBytes, kafka will return it anyway.
Having too large of a maxBytes configuration can result in very unhappy consumers when catching up on topic depth.

Consumer Groups - Offset Manaement
Each message is assigned an offset which is unique and ordered within its single partition
Each time a consumer fetches data it is served payloads based on its current offset for that partition in the consumer group.
Offsets can be tracked outside of kafka
Once a payload has been processed the consumer "commits" the offsets.
- This makes it so during the next fetch kafka gives it new payloads.
- Kafkajs will save this offsets in a buffer then flush them to kafka periodically
	- You can also force kafkajs to commit any offsets on demand.

**Consumer Groups - Important Timeouts**
heartbeatInterval - The minimum amount of time between heartbeats
	- kafkajs will throw away heartbeats any more frequent than this value
	- should be no more than 1/3 of sessionTimeout
sessionTimeout - It wants to hear from each consumer at least one time every sessionTimeout. This needs to be set longer than your longest processing step. Otherwise if it doesn't hear from the consumer, it gets evicted from the consumer group.
rebalanceTimeout - The maximum time that the coordinator will wait for each mamber to rejoin when rebalancing the group. This must be longer than sessionTimeout so there is time for evicted consumers to get back in to the next generation of the consumer group.

The shorter the timeout values are, the better.

Consumer Groups - Rebalancing
 Very difficult - write more notes later

Data Retention
If you are setting a retention time for the broker, you can use ms, seconds, minutes, etc.
if you are setting it for a topic, you can only set it in ms
Redox uses time based retention.

This topic matters more for shorter retention periods.
