# TEMPLATE: System tests for Pulsar releases

This document describes the system tests that are to be executed on a candidate Pulsar release.

The developer standalone version can be used for sanity tests, but the majority of the tests must be run against Docker images running in a real (ie not Minikube/kind) Kubernetes cluster. There also needs to be multiple of each component (zookeeper, bookkeeper, broker, proxy etc). This is a very important port, since most testing is done with a single instance of each component on a developer laptop or in the automated test pipeline. These tests are meant to test a fully deployed Pulsar cluster in order to determine if the load is suitable for production deployments.

## Version under test

* Repository:
* Tag: 
* Docker Images:

## Kubernetes tests

### Deployment in using kind/minikube

Perf client in these tests can be connected using bastion

- [ ] Docker images install in Minikube/kind using Helm
- [ ] Helm upgrade from previous release to new Docker images
- [ ] Rollout restart Zookeeper with connected perf client
- [ ] Rollout restart BookKeeper with connected perf client
- [ ] Rollout restart Broker with connected perf client
- [ ] Rollout restart Proxy with connected perf client

### Deployment using cloud-based Kubernetes cluster

Perf client in these tests must be external to the cluster

- [ ] Upgrade from previous release to new Docker images
- [ ] Rollout restart Zookeeper with connected perf client
- [ ] Rollout restart BookKeeper with connected perf client
- [ ] Rollout restart Broker with connected perf client
- [ ] Rollout restart Proxy with connected perf client

## Pulsar features

### Basic client plaintext

This can be tested using Pulsar standalone.

- [ ] Websocket client consume connect and send messages
- [ ] Websocket client reader connect and send messages
- [ ] Websocket client reader replay messages

### Subscription types

- [ ] Connect Exclusive subscription
- [ ] Only one client on exclusive subscription
- [ ] Connect Failover subscription
- [ ] Multiple clients on failover subscription
- [ ] Only one client on failover sub receives messages

- [ ] Connect Shared subscription
- [ ] Multiple clients on shared subscription, round robin delivery
- [ ] Add/remove clients to shared subscription while sending messages

### Topic creation/deletion

- [ ] Manual topic create, send messages on topic
- [ ] Attempt to delete topic with active producers/consumers
- [ ] Force delete topic with active producers/consumers

- [ ] Topic create with partitions
- [ ] Send messages to partition topic
- [ ] Receive messages with client connected to all partitions
- [ ] Receive messages with client connected to a single partition
- [ ] Topic delete with partitions
- [ ] Delete one partition from topic. Can still send/receive messages

- [ ] Deleting topic deletes all partitions
- [ ] Deleting topic with some partitions already deleted removed remaining partitions
- [ ] Stats for non-partitioned topics are correct
- [ ] Stats for partitioned topics are correct

### Subscription creation/deletion

- [ ] Create subscription on non-partition topic
- [ ] Create multiple subscriptions on non-partitioned topic
- [ ] Subscription type is set when consumer connects

- [ ] Create subscription with earliest message
- [ ] Create subscription starting with specific message

- [ ] Create subscription on partitioned topic; subscription shows on all partitions
- [ ] Delete subscription on partitioned topic; subscription deleted on all partitions
- [ ] Stats for subscriptions are correct

### Subscription peek, skip, and rewind

- [ ] Peek at messages in a subscription
- [ ] Skip all messages in a subscription
- [ ] Skip some messages in a subscription
- [ ] Rewind messages in a subscription to a time
- [ ] Rewind messages ins a subscription to a message ID

- [ ] Peek at messages in a subscription on partitioned topic
- [ ] Skip all messages in a subscription on partitioned topic
- [ ] Skip some messages in a subscription on partitioned topic
- [ ] Rewind messages in a subscription to a time on partitioned topic
- [ ] Rewind messages ins a subscription to a message ID on partitioned topic

### WebSocket Secure client

- [ ] Connect wss client with token-based authentication
- [ ] Producer on wss client
- [ ] Consumer on wss client
- [ ] Reader on wss client

### Python (C++ wrapped) client

Since new client version won't be released, use previous version

- [ ] Connect Python client with TLS and token-based authentication
- [ ] Producer on secure Python client
- [ ] Consumer on secure Python client
- [ ] Reader on secure Python client

### Java client

Since new client version won't be released, use previous version

- [ ] Connect Java client with TLS and token-based authentication
- [ ] Producer on secure Java client
- [ ] Consumer on secure Java client
- [ ] Reader on secure Java client

### Message TTL

- [ ] Message TTL can be configured on namespace
- [ ] Unacked messages are removed from topic when TTL expires

### Schema

- [ ] Add schema with producer
- [ ] Add schema with consumer
- [ ] Add schema with admin API
- [ ] Delete schema
- [ ] Update existing schema on topic using admin API
- [ ] Schema evolution backward compatibility (remove field)

### TLS

- [ ] Clients can connect with TLS enabled

### Token-based authentication

- [ ] Clients can connect with token-based authentication enabled
- [ ] Clients with incorrect token are rejected

### Message retention

- [ ] Messages can be configured to be retained after they are acknowledged
- [ ] Retained messages can be added back to subscription
- [ ] Readers can retrieve all retained messages

### Non persistent messages

- [ ] Non-persistent messages can be sent and received
- [ ] Stats for non-persistent messages are correct

### Tiered storage

- [ ] AWS S3. Messages offloaded to tiered storage and retrieved from tiered storage
- [ ] Google Cloud Storage. Messages offloaded to tiered storage and retrieved from tiered storage


### Functions

- [ ] Upload Java function
- [ ] Upload Python function
- [ ] Delete Java function
- [ ] Delete Python function
- [ ] Update Java function
- [ ] Update Python function
- [ ] Trigger Java function
- [ ] Trigger Python function
- [ ] Create function with multiple instances
- [ ] Restart all instances of a function
- [ ] Restart one instance of a function
- [ ] Stop all instances of a function
- [ ] Stop one instance of a function
- [ ] Start all instances of a function
- [ ] Start one instance of a function

### Sinks

- [ ] Add sink from built-ins
- [ ] Delete a sink
- [ ] Update a sink
- [ ] Create sink with multiple instances
- [ ] Restart all instances of a sink
- [ ] Restart one instance of a sink
- [ ] Stop all instances of a sink
- [ ] Stop one instance of a sink
- [ ] Start all instances of a sink
- [ ] Start one instance of a sink
- [ ] End-to-end sink test using JDBC sink. Publish messages that populate mysql table.
