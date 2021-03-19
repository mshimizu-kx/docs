---
title: Using Kafka with kdb+ – Interfaces
description: How to connect a kdb+ server process to the Apache Kafka distributed streaming platform
keywords: apache, api, consumer, fusion, interface, kafka, learning, library, machine, producer, q
---
# ![Apache Kafka](../img/kafka.png) Using Kafka with kdb+




:fontawesome-brands-github: [KxSystems/kafka](https://github.com/KxSystems/kafka)


`kafkakdb` is a thin wrapper for kdb+ around the `librdkafka` C API (available on [MacOS/Linux](https://github.com/edenhill/librdkafka) or [Windows](https://www.nuget.org/packages/librdkafka.redist/1.0.0)) for [Apache Kafka](https://kafka.apache.org/).

Follow the [installation instructions](https://github.com/KxSystems/kafka#building-and-installation) for set-up.

To run examples on this page you will need a Kafka broker available. It is easy to [set up a local instance for testing](https://github.com/KxSystems/kafka#setting-up-test-kafka-instance).


## API

The library follows the `librdkafka` API closely where possible.
As per its [introduction](https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md):

- Base container `rd_kafka_t` is a client created by `.kafka.newProducer` and `.kafka.newConsumer`. These provide global configuration and shared states.
- One or more topics `rd_kafka_topic_t`, which are either producers or consumers are created by the function `.kafka.newTopic`.

Both clients and topics accept an optional configuration dictionary.
`.kafka.newProducer`, `.kafka.newConsumer` and `.kafka.newTopic` return an int which acts as a Client or Topic ID (index into an internal array). Client IDs are used to create topics and Topic IDs are used to publish or subscribe to data on that topic. They can also be used to query metadata – state of subscription, pending queues, etc.


### Minimal producer example

```q
\l kafka.q
// specify kafka brokers to connect to and statistics settings.
kfk_cfg:`metadata.broker.list`statistics.interval.ms!`localhost:9092`10000
// create producer with the config above with timeout of 5 seconds.
producer:.kafka.newProducer[kfk_cfg; 5000i]
// setup producer topic "test"
test_topic:.kafka.newTopic[producer;`test;()!()]
// publish current time with a key "time"
.kafka.publish[test_topic; .kafka.PARTITION_UA; "hello from producer";"messagekey1"];
```

:fontawesome-regular-hand-point-right: 
[:fontawesome-brands-github: KxSystems/kafka/examples/test_producer.q](https://github.com/KxSystems/kafka/blob/master/examples/test_producer.q)


### Minimal consumer example

```q
\l kafka.q
// create consumer process within group 0 with a timeout of 5 seconds.
consumer:.kafka.newConsumer[`metadata.broker.list`group.id!`localhost:9092`0; 5000i];
data:();
// setup meaningful consumer callback(do nothing by default)
consume_callback:{[consumer;msg]
    msg[`data]:"c"$msg[`data];
    msg[`rcvtime]:.z.p;
    data,::enlist msg;
    .kafka.commitOffsetsToTopicPartition[consumer; msg `topic; enlist[msg `partition]!enlist msg[`offset]; 1b]
    }
// subscribe to the "test" topic with default partitioning
.kafka.subscribe[consumer;`test];
.kafka.registerConsumeTopicCallback[consumer; `test`; consume_callback consumer]
```

:fontawesome-regular-hand-point-right: 
[:fontawesome-brands-github: KxSystems/kafka/examples/test_consumer.q](https://github.com/KxSystems/kafka/blob/master/examples/test_consumer.q) for a slightly more elaborate version 

## Configuration

The library supports and uses all configuration options exposed by `librdkafka`, except callback functions, which are identical to Kafka options by design of `librdkafka`. 

:fontawesome-regular-hand-point-right: 
[:fontawesome-brands-github: edenhill/librdkafka/CONFIGURATION.md](https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md) for a list of options

## Performance and tuning

:fontawesome-regular-hand-point-right: 
[:fontawesome-brands-github: edenhill/librdkafka/wiki/How-to-decrease-message-latency](https://github.com/edenhill/librdkafka/wiki/How-to-decrease-message-latency)

There are numerous configuration options and it is best to find settings that suit your needs and setup. See [Configuration](#configuration) above. 
