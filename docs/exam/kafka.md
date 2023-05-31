# Kafka

## 问题

- Kafka 是什么？主要应用场景有哪些？
- 和其他消息队列相比,Kafka 的优势在哪里？
- 队列模型了解吗？Kafka 的消息模型知道吗？
- 什么是 Producer、Consumer、Broker、Topic、Partition？
- Kafka 的多副本机制了解吗？带来了什么好处？
- Zookeeper 在 Kafka 中的作用知道吗？
- Kafka 如何保证消息的消费顺序？
- Kafka 如何保证消息不丢失
- Kafka 如何保证消息不重复消费

## 答案


### Kafka 是什么？主要应用场景有哪些？

!!! tip "Kafka 是什么？主要应用场景有哪些？"
    
    分布式流式处理平台。可以发布订阅消息流，可把消息持久化到磁盘。
    
    场景：
    
    - 消息队列
    - 日志收集


### 和其他消息队列相比,Kafka 的优势在哪里？

!!! tip "和其他消息队列相比,Kafka 的优势在哪里？"

    1. 性能更好
    2. 生态也好，据我所知，大数据和流计算应用也很多
    

### 队列模型了解吗？Kafka 的消息模型知道吗？

!!! tip "队列模型了解吗？Kafka 的消息模型知道吗？"

    早期的是一个队列被一个消费者使用，未消费的会一直等着被消费或者超时，但是多个消费者不好解决。
    
    kafka发布订阅消息模型：使用主题，类似于广播的形式。生产者生效消息到主题里，通过主题传递给订阅者或者说消费者
    
    
    
    
### 什么是 Producer、Consumer、Broker、Topic、Partition？

!!! tip "什么是 Producer、Consumer、Broker、Topic、Partition？"
    
    - Producer:生产者
    - Consumer:消费者
    - Broker:代理，相当于一个kafka实例
    - Topic:订阅的主题
    - Partition:分区，topic的一部分，可以理解为消息队列里的队列，一个topic可以有多个分区。


### Kafka 的多副本机制了解吗？带来了什么好处？

!!! tip "Kafka 的多副本机制了解吗？带来了什么好处？"

    这个多副本有一个leader，生产者和消费者只与这个leader交互，其它的叫做follower，只有leader故障了，才会从follower里选举一个作为leader。
    
    好处就是更安全，容灾能力变强。


### Zookeeper 在 Kafka 中的作用知道吗？

!!! tip "Zookeeper 在 Kafka 中的作用知道吗？"

    1. broker注册，topic注册到broker的对应关系也保存在zookeeper
    2. 负载均衡

### Kafka 如何保证消息的消费顺序？

!!! tip "Kafka 如何保证消息的消费顺序？"

    - 1 个 Topic 只对应一个 Partition。
    - （推荐）发送消息的时候指定 key/Partition。


### Kafka 如何保证消息不丢失

!!! tip "Kafka 如何保证消息不丢失"

    生产者
    1. kafkaTemplate.send有个返回future，增加一个回调
    2. kafkaTemplate.send().get()变成同步（不推荐）
    3. Producer retries重试次数配置一个合理的值
    
    消费者
    关闭自动提交，改成手动提交


###  Kafka 如何保证消息不重复消费

!!! tip " Kafka 如何保证消息不重复消费"

    
    kafka 出现消息重复消费的原因：
    
    服务端侧已经消费的数据没有成功提交 offset（根本原因）
    
    1. 消费端服务做幂等
    2. 关闭自动提交，手动提交。然后消费者收到消息，立刻提交。然后通过定时任务，校验数据兜底

