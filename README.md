# Pulsar Test Results

This repository stores the test results from candidate and official builds of Apache Pulsar. For candidate builds, testing
is done using Docker images built based on the candidate release tag. For official builds, the tests are run against
the official Docker images released by the project. For testing done on forked repositories, the GitHub tag and Docker Hub images references are
indicated.

## Test Plan Template

The tests are run primarily against Docker images running in Kubernetes to simulate a real production environment with
multiple copies brokers, bookies, etc to find issues that cannot be found in unit tests or when running in 
standalone mode. 

The following is the test plan template that we use for each release:

<insert link>

We strive to continually expand and improve this test plan to ensure the highest quality releases of Apache Pulsar. Comments and suggests are welcome.

## Results

### Apache Pulsar 2.5.1 Candidate 4

* Repository: [apache/pulsar](https://github.com/apache/pulsar)
* Tag: v2.5.1-candidate-2
* Results: <insert link>
