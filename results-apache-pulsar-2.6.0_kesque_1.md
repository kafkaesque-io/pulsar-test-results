# TEMPLATE: System tests for Pulsar releases

This document describes the system tests that are to be executed on a candidate Pulsar release.

The developer standalone version can be used for sanity tests, but the majority of the tests must be run against Docker images running in a real (ie not Minikube/kind) Kubernetes cluster. There also needs to be multiple of each component (zookeeper, bookkeeper, broker, proxy etc). This is a very important port, since most testing is done with a single instance of each component on a developer laptop or in the automated test pipeline. These tests are meant to test a fully deployed Pulsar cluster in order to determine if the load is suitable for production deployments.

## Version under test

* Repository: [kafkaesque-io/pulsar](https://github.com/kafkaesque-io/pulsar)
* Tag: 2.6.0_kesque_1
* Docker Images: [kafkaesqueio/pulsar-all](https://hub.docker.com/repository/docker/kafkaesqueio/pulsar-all)

## Kubernetes tests

### Deployment in using kind/minikube

Perf client in these tests can be connected using bastion

- [x] Docker images install in Minikube/kind using Helm
- [x] Helm upgrade from previous release to new Docker images
- [x] Rollout restart Zookeeper with connected perf client
- [x] Rollout restart BookKeeper with connected perf client
- [x] Rollout restart Broker with connected perf client
- [x] Rollout restart Proxy with connected perf client

### Deployment using cloud-based Kubernetes cluster

Perf client in these tests must be external to the cluster

- [x] Upgrade from previous release to new Docker images
- [x] Rollout restart Zookeeper with connected perf client
- [x] Rollout restart BookKeeper with connected perf client
- [x] Rollout restart Broker with connected perf client
- [x] Rollout restart Proxy with connected perf client

## Pulsar features

### Basic client plaintext

This can be tested using Pulsar standalone.

- [x] Websocket client consume connect and send messages
- [x] Websocket client reader connect and send messages
- [x] Websocket client reader replay messages

### Subscription types

- [x] Connect Exclusive subscription
- [x] Only one client on exclusive subscription
- [x] Connect Failover subscription
- [x] Multiple clients on failover subscription
- [x] Only one client on failover sub receives messages
- [x] Connect Shared subscription
- [x] Multiple clients on shared subscription, round robin delivery
- [x] Add/remove clients to shared subscription while sending messages
- [x] Connect Key Shared subscription
- [x] Multiple clients on key shared subscription, delivery based on key
- [x] Add/remove clients to key shared subscription while sending messages delivery based on key

### Topic creation/deletion

- [x] Manual topic create, send messages on topic
- [x] Attempt to delete topic with active producers/consumers
- [x] Force delete topic with active producers/consumers
- [x] Topic create with partitions
- [x] Send messages to partition topic
- [x] Receive messages with client connected to all partitions
- [x] Receive messages with client connected to a single partition
- [x] Topic delete with partitions
- [x] Delete one partition from topic. Can still send/receive messages
- [x] Deleting topic deletes all partitions
- [x] Deleting topic with some partitions already deleted removed remaining partitions
- [x] Stats for non-partitioned topics are correct
- [x] Stats for partitioned topics are correct

### Subscription creation/deletion

- [x] Create subscription on non-partition topic
- [x] Create multiple subscriptions on non-partitioned topic
- [x] Subscription type is set when consumer connects
- [x] Create subscription with earliest message
- [x] Create subscription starting with specific message
- [x] Create subscription on partitioned topic; subscription shows on all partitions
- [x] Delete subscription on partitioned topic; subscription deleted on all partitions
- [x] Stats for subscriptions are correct

### Subscription peek, skip, and rewind

- [x] Peek at messages in a subscription
- [x] Skip all messages in a subscription
- [x] Skip some messages in a subscription
- [x] Rewind messages in a subscription to a time
- [x] Rewind messages ins a subscription to a message ID
- [X] Peek at messages in a subscription on partitioned topic
- [x] Skip all messages in a subscription on partitioned topic
- [x] Skip some messages in a subscription on partitioned topic
- [x] Rewind messages in a subscription to a time on partitioned topic
- [x] Rewind messages ins a subscription to a message ID on partitioned topic

### WebSocket Secure client

- [x] Connect wss client with token-based authentication
- [x] Producer on wss client
- [x] Consumer on wss client
- [x] Reader on wss client

### Python (C++ wrapped) client

- [x] Connect Python client with TLS and token-based authentication
- [x] Producer on secure Python client
- [x] Consumer on secure Python client
- [x] Reader on secure Python client

### Java client

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

- [x] Clients can connect with TLS enabled

### Token-based authentication

- [x] Clients can connect with token-based authentication enabled
- [ ] Clients with incorrect token are rejected

### Message retention

- [ ] Messages can be configured to be retained after they are acknowledged
- [ ] Retained messages can be added back to subscription
- [ ] Readers can retrieve all retained messages

### Non persistent messages

- [ ] Non-persistent messages can be sent and received
- [ ] Stats for non-persistent messages are correct

### Resource clean up
- [ ] Persistent topics with no consumers/producers/subscriptions are removed
- [ ] Non-persistent topics with no consumers/producers/subscriptions are removed
- [ ] Partitioned topics with no consumers/producers/subscriptions are removed     
- [ ] Subscriptions with no backlog are removed (when enabled)  
- [ ] Persistent topics that have been cleaned up do not re-appear 
- [ ] Non-persistent topics that have been cleaned up do not re-appear 
- [ ] Non-persistent topics with schema that have been cleaned up do not re-appear 

### Tiered storage

- [ ] AWS S3. Messages offloaded to tiered storage and retrieved from tiered storage
- [ ] Google Cloud Storage. Messages offloaded to tiered storage and retrieved from tiered storage
- [ ] Tardigrade Cloud Storage. Messages offloaded to tiered storage and retrieved from tiered storage


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
- [ ] Function with multiple input topics
- [ ] Windowed function
- [ ] Function with state storage
- [ ] Function with metrics

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
- [x] End-to-end sink test using JDBC sink. Publish messages that populate mysql table.

### Source

- [ ] Add source from built-ins
- [ ] Delete a source
- [ ] Update a source
- [ ] Create source with multiple instances
- [ ] Restart all instances of a source
- [ ] Restart one instance of a source
- [ ] Stop all instances of a source
- [ ] Stop one instance of a source
- [ ] Start all instances of a source
- [ ] Start one instance of a source 
- [x] End-to-end sink test using source. Send messages that end up in Pulsar topic. 
