# System test resulsts Pulsar 2.6.3 

This document describes the system tests that are to be executed on a candidate Pulsar release.

The developer standalone version can be used for sanity tests, but the majority of the tests must be run against Docker images running in a real (ie not Minikube/kind) Kubernetes cluster. There also needs to be multiple of each component (zookeeper, bookkeeper, broker, proxy etc). This is a very important port, since most testing is done with a single instance of each component on a developer laptop or in the automated test pipeline. These tests are meant to test a fully deployed Pulsar cluster in order to determine if the load is suitable for production deployments.

## Version under test

* Repository: [kafkaesque-io/pulsar](https://github.com/kafkaesque-io/pulsar)
* Branch: kesque_2.6.3 
* Docker Images: [kafkaesqueio/pulsar-all](https://hub.docker.com/repository/docker/kafkaesqueio/pulsar-all)
* Tag: 2.6.3_kesque_1 

* Repository: [datastax/pulsar](https://github.com/datastax/pulsar)
* Branch: 2.6.3_ds
* Docker Images: [kafkaesqueio/pulsar-all](https://hub.docker.com/repository/docker/datastax/pulsar-all)
* Tag: 2.6.3_ds_f4fdc0c1cc_1229


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
- [x] Peek at messages in a subscription on partitioned topic
- [x] Skip all messages in a subscription on partitioned topic
- [x] Skip some messages in a subscription on partitioned topic
- [x] Rewind messages in a subscription to a time on partitioned topic
- [x] Rewind messages ins a subscription to a message ID on partitioned topic

### Producer and Consumer Limits
- [x] Producers can be limited on topic
- [x] Consumers can be limited on subscription
- [x] Consumers can be limited on topic
- [x] Publish rate can be limited on topic
- [x] Dispatch rate can be limited on topic
- [x] Dispatch rate can be limited on subscription

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

- [x] Connect Java client with TLS and token-based authentication
- [x] Producer on secure Java client
- [x] Consumer on secure Java client
- [x] Reader on secure Java client

### Message TTL

- [x] Message TTL can be configured on namespace
- [x] Unacked messages are removed from topic when TTL expires

### Schema

- [x] Add schema with producer
- [x] Add schema with consumer
- [x] Add schema with admin API
- [x] Delete schema
- [x] Update existing schema on topic using admin API
- [x] Schema evolution backward compatibility (remove field)

### TLS

- [x] Clients can connect with TLS enabled

### Token-based authentication

- [x] Clients can connect with token-based authentication enabled
- [x] Clients with incorrect token are rejected

### Message retention

- [x] Messages can be configured to be retained after they are acknowledged
- [x] Retained messages can be added back to subscription
- [x] Readers can retrieve all retained messages

### Non persistent messages

- [x] Non-persistent messages can be sent and received
- [x] Stats for non-persistent messages are correct

### Resource clean up
- [x] Persistent topics with no consumers/producers/subscriptions are removed
- [x] Non-persistent topics with no consumers/producers/subscriptions are removed
- [x] Partitioned topics with no consumers/producers/subscriptions are removed     
- [x] Subscriptions with no backlog are removed (when enabled)  
- [x] Persistent topics that have been cleaned up do not re-appear 
- [x] Non-persistent topics that have been cleaned up do not re-appear 

Non-persistent topics re-appear after cleanup if the stats endpoint is called against them.

### Tiered storage

- [x] AWS S3. Messages offloaded to tiered storage and retrieved from tiered storage
- [x] Google Cloud Storage. Messages offloaded to tiered storage and retrieved from tiered storage
- [x] Tardigrade Cloud Storage. Messages offloaded to tiered storage and retrieved from tiered storage
- [x] Automatic offload works for globally configured offload
- [x] Manual offload works for globally configured offload   
- [x] Per namespace offload policies can be configured
- [x] Automatic offload works for namespace configured offload
- [x] Manual offload works for namespace configured offload



### Functions

- [x] Upload Java function
- [x] Upload Python function
- [x] Delete Java function
- [x] Delete Python function
- [x] Update Java function
- [x] Update Python function
- [x] Trigger Java function
- [x] Trigger Python function
- [x] Create function with multiple instances
- [x] Restart all instances of a function
- [x] Restart one instance of a function
- [x] Stop all instances of a function
- [x] Stop one instance of a function
- [x] Start all instances of a function
- [x] Start one instance of a function
- [x] Function with multiple input topics
- [x] Windowed function
- [x] Function with state storage
- [x] Function logs display correctly
- [x] Function stats display correctly
- [x] Process runtime
- [x] Kubernetes runtime

### Sinks

- [x] Add sink from built-ins
- [x] Delete a sink
- [x] Update a sink
- [x] Create sink with multiple instances
- [x] Restart all instances of a sink
- [x] Restart one instance of a sink
- [x] Stop all instances of a sink
- [x] Stop one instance of a sink
- [x] Start all instances of a sink
- [x] Start one instance of a sink
- [x] End-to-end sink test using JDBC sink. Publish messages that populate mysql table.
- [x] Sink stats display correctly


### Source

- [x] Add source from built-ins
- [x] Delete a source
- [x] Update a source
- [x] Create source with multiple instances
- [x] Restart all instances of a source
- [x] Restart one instance of a source
- [x] Stop all instances of a source
- [x] Stop one instance of a source
- [x] Start all instances of a source
- [x] Start one instance of a source
- [x] End-to-end sink test using source. Send messages that end up in Pulsar topic.
- [x] Source stats display correctly

