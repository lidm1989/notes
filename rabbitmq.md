# **Rabbitmq的安装与使用**

# Exchanges and Exchange Types

问题：交换机是如何根据key来分配消息到队列？

## Direct Exchange

 A direct exchange delivers messages to queues based on the message routing key. A direct exchange is ideal for the unicast routing of messages (although they can be used for multicast routing as well). Here is how it works:

* A queue binds to the exchange with a routing key K
* When a new message with routing key R arrives at the direct exchange, the exchange routes it to the queue if K = R

Direct exchanges are often used to distribute tasks between multiple workers (instances of the same application) in a round robin manner. When doing so, it is important to understand that, in AMQP 0-9-1, messages are load balanced between consumers and not between queues.

A direct exchange can be represented graphically as follows: exchange delivering messages to queues based on routing key 

## Fanout Exchange

 A fanout exchange routes messages to all of the queues that are bound to it and the routing key is ignored. If N queues are bound to a fanout exchange, when a new message is published to that exchange a copy of the message is delivered to all N queues. Fanout exchanges are ideal for the broadcast routing of messages.

Because a fanout exchange delivers a copy of a message to every queue bound to it, its use cases are quite similar:

* Massively multi-player online (MMO) games can use it for leaderboard updates or other global events
* Sport news sites can use fanout exchanges for distributing score updates to mobile clients in near real-time
* Distributed systems can broadcast various state and configuration updates
* Group chats can distribute messages between participants using a fanout exchange (although AMQP does not have a built-in concept of presence, so XMPP may be a better choice)

## Topic Exchange

 Topic exchanges route messages to one or many queues based on matching between a message routing key and the pattern that was used to bind a queue to an exchange. The topic exchange type is often used to implement various publish/subscribe pattern variations. Topic exchanges are commonly used for the multicast routing of messages.

Topic exchanges have a very broad set of use cases. Whenever a problem involves multiple consumers/applications that selectively choose which type of messages they want to receive, the use of topic exchanges should be considered.

Example uses:

* Distributing data relevant to specific geographic location, for example, points of sale
* Background task processing done by multiple workers, each capable of handling specific set of tasks
* Stocks price updates (and updates on other kinds of financial data)
* News updates that involve categorization or tagging (for example, only for a particular sport or team)
* Orchestration of services of different kinds in the cloud
* Distributed architecture/OS-specific software builds or packaging where each builder can handle only one

## Headers Exchange

 A headers exchange is designed for routing on multiple attributes that are more easily expressed as message headers than a routing key. Headers exchanges ignore the routing key attribute. Instead, the attributes used for routing are taken from the headers attribute. A message is considered matching if the value of the header equals the value specified upon binding.

It is possible to bind a queue to a headers exchange using more than one header for matching. In this case, the broker needs one more piece of information from the application developer, namely, should it consider messages with any of the headers matching, or all of them? This is what the "x-match" binding argument is for. When the "x-match" argument is set to "any", just one matching header value is sufficient. Alternatively, setting "x-match" to "all" mandates that all the values must match.

Headers exchanges can be looked upon as "direct exchanges on steroids". Because they route based on header values, they can be used as direct exchanges where the routing key does not have to be a string; it could be an integer or a hash (dictionary) for example. 

# Queues

问题：队列怎样将消息发送给消费者？

# rabbitmq-plugins
    rabbitmq-plugins list

# rabbitmq-managent
    rabbitmq-plugins enable rabbitmq_managenment
    
# rabbitmq_tracing
    rabbitmq-plugins enable rabbitmq_tracing

# 参考
[1][Redis官网](http://redis.io/)

[2][redis-py官网](http://redis-py.readthedocs.io/en/latest/index.html)

