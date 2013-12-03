# Apache Storm

According to Nathan Marz Apache Storm "is a distributed, fault-tolerant, and high-performance realtime computation system that provides strong guarantees on the processing of data" ([MARZ13]).

## Introduction

As a streaming framework Storm provides a set of primitives, aggregates and transformation functions. Storm consists of a scheduler, the supervisor and a ui frontend. The Scheduler is called nimbus and is used for job distribution. A Apache Zookeeper cluster is beeing used for the worker nodes. In the ui frontend it is possible to see the current state of the storm cluster.

[MARZ13]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#MARZ13  "Apache Storm Proposal"