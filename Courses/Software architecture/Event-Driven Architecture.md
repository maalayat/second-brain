## Introduction

1. Monolithic
2. DIP
	- Dependency Inversion Principle compliant
	- Open/Closed Principle compliant
3. Hexagonal Architecture
4. CQRS

## Issues

- Eventual consistency
- Projections handlers
- Duplicated events
- Unsorted events
- Testing [Microservice testing](https://martinfowler.com/articles/microservice-testing/)

[Reactive Manifesto](https://www.reactivemanifesto.org/)

## Defining the structure of our domain events

### Json api

[Specifications](https://jsonapi.org/)

### Example

```
{
  "data": {
    "id": "event id", // UUID
    "type": "domain_event_name", // solmedia.video.1.event.video.published
    "occurred_on": "date event has occurred on", // with formant yyyy-mm-dd UTC?
    "attributes": { // our domain data
      "id": "aggregate id",
      "some_parameter": "some value"
    },
    "meta": {
      "some_key": "some value",
      "host": "machine hostname",
      "legacy": "true"
    }
  }
}
```

### Proposal

#### Publisher
- 1 exchange
- Exchange topic

#### Consumer
- 1 queue

### AsyncAPI Topic definition

```
organization.notification.1.event.client.sms.send.pending
```

- Organization `organization`
- Service/Team/Department `notification`
- Message version `1`
- Message type (command or an event) `event`
- Resources and sub-resources `client`
- Event/command name `sms.send`
- Status (queued, succeeded, faile, done) `pending`

https://github.com/fmvilas/topic-definition



## Introduction to messaging queues

Publisher, Exchange, Queue, and Consumer. Exchange types, and you will learn how to define your own queue architecture.

### Message Broker

![[message_broker.png]]

#### Exchange Types

- Fanout: Broadcast
- Direct: binding key. Send an event if binding_key == routing_key
- Topic: similar to direct with wildcars into binding key

#### Protocols

- AMQP: Advanced Message Queuing Protocol (ActiveMQ or RabbitMQ)
- MQTT: Message Queuing Telemetry Transport
- STOMP: Simple (or Streaming) Text Oriented Message Protocol

#### Simulator

https://tryrabbitmq.com/

#### Docker
- https://hub.docker.com/_/rabbitmq/

## Error handling when consuming events
When consuming from a queue, the events may arrive out of order, and they may also be duplicated. We will learn what strategies we can follow to manage these situations without them being a problem.

### Unordered Events

![[unordered_events.png]]

#### Solution 1: Re-queue the event by incrementing counter

![[re-queue_event.png]]

- Maximum number of retries to avoid infinite loops **in the consumer**
- Queue in **dead letter** the events that exceed the maximum number of retries.
- **The application** is responsible for this logic

#### Solution 2: Do not send ACK to the queue

![[Pasted image 20230111135052.png]]

- Maximum number of retries to avoid infinite loops in **the infrastructure**
- Queue in **dead letter** the events that exceed the maximum number of retries.
- **The infrastructure** is responsible for this logic

### Duplicate events

#### Let it crash
- Retries and dead letter

#### Log of processed events
- Save ids events

#### Idempotency

### Code
```
async onMessage(message: ConsumeMessage) {  
  const content = message.content.toString();  
  const domainEvent = this.deserializer.deserialize(content);  
  
  try {  
    await this.subscriber.on(domainEvent);  
  } catch (error) {  
    await this.handleError(message);  
  } finally {  
    this.connection.ack(message);  
  }  
}

private async handleError(message: ConsumeMessage) {  
  if (this.hasBeenRedeliveredTooMuch(message)) {  
    await this.deadLetter(message);  
  } else {  
    await this.retry(message);  
  }  
}  
  
private async retry(message: ConsumeMessage) {  
  await this.connection.retry(message, this.queueName, this.exchange);  
}  
  
private async deadLetter(message: ConsumeMessage) {  
  await this.connection.deadLetter(message, this.queueName, this.exchange);  
}  
  
private hasBeenRedeliveredTooMuch(message: ConsumeMessage) {  
  if (this.hasBeenRedelivered(message)) {  
    const count = parseInt(message.properties.headers['redelivery_count']);  
    return count >= this.maxRetries;  
  }  
  return false;  
}  
  
private hasBeenRedelivered(message: ConsumeMessage) {  
  return message.properties.headers['redelivery_count'] !== undefined;  
}
```