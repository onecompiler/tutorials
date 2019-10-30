Distributed queue management(DQM) is a point-to-point messaging mechanism. IBM MQ provides decoupling of provider and consumer information using pub-sub mechanism. Publish-Subscribe mechanism doesn't need information about the sender/receiver parties to each other.

A publisher publishes a message along with the topic and then queue manager sends the message to all the subscribers who subscribe for the particular topic.

The sender and receiver applications no need to know about each other.

## Topic
Topic is a text string which can be a meaningful title of the data that is getting published.
Subscribers subscribe to the topics published by the publishers.

## Publisher
Publisher is an application which publishes the topic in the form of messages to MQ, known as publication.
Publisher can publish and at the same time it can subscribe to some other topics as well.

## Subscriber
Subscriber is an application who will subscribe for the topics published by publisher.


