# Message Broker Discussion


## Introduction

[INTERMAGNET](http://intermagnet.org/) is considering ways to reduce the
latency of near-real time data delivery from member observatories to critical
users such as space weather forecasters.

[Message Brokers](https://en.wikipedia.org/wiki/Message_broker) provide
low-latency push distribution of messages and appear to fit the use case well.


## Scope

At this time, the discussion is limited to standardization of the message
format and broker topic structure and specifically does not intend to
standardize the protocol and broker implementation to be used.

Both Geomagnetic Informtion Nodes (GINs) and Institutes are encouraged to
develop prototypes based on the outcome of this discussion.


## Message format

A Message is the data being transmitted.  Typically this is handled as opaque,
binary data.  When a platform can only transmit text, tools like Base64
encoding can be used to support binary data with added encoding and decoding
overhead.


### Recommendations:

- include metadata
  - Allow users to handle multiple topics in a single routine
    - observatory
    - channel
    - starttime, sample rate

- support multiple channels and multiple samples per channel
  (within reasonable limits)
  - Mirror data production (vector instrument vs scalar instrument,
    or combined where it makes sense)
  - Balance message overhead with desired data volume
  - Send when data becomes available, but chunk into smaller messages
    where appropriate.

```json
{
    'observatory': 'OBS',
    'instrument': 'INSTRUMENT',
    'type': 'TYPE'

    'starttime': Date,
    'delta': Float,

    'data': {
        'COMPONENT': [
            Float,
            Float,
            ...
        ]
    }
}
```


## Topic format

A Topic is a queue of related messages.  Brokers support multiple Topics and
Producers specify the topic where a message is to be published.  Consumers
subscribe to one or more Topics.  Some Brokers support wildcard subscriptions.

Topics are usually defined hierarchically, making it easier to use wildcards
when subscribing.  Different brokers use different separators to define these
hierarchies, but logical hierarchies can be maintained regardless of the
separator.

### Recommendations:

- intermagnet prefix
    - Allow shared broker environment

- include enough metadata for users to filter
    - `intermagnet | VERSION | OBS | TYPE | INSTRUMENT | INTERVAL`


## Protocols

This list of protocols is provided for information, but the intent is to not
standardize on any protocol at this time.

- [Advanced Message Queueing Protocol (AMQP) v1.0](https://www.amqp.org/)
- [Apache Kafka](https://kafka.apache.org/)
- [Message Queue Telemetry Transport (MQTT)](http://mqtt.org/)
- [Streaming Text Oriented Messaging Protocol (STOMP)](https://stomp.github.io/)
- [Web Application Messaging Protocol (WAMP)](http://wamp-proto.org/)


## Additional Reading

Various links about message brokers for more information.

- http://zeromq.org/whitepapers:amqp-analysis
