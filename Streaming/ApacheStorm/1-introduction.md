# Apache Storm

According to Nathan Marz Apache Storm "is a distributed, fault-tolerant, and high-performance realtime computation system that provides strong guarantees on the processing of data" ([MARZ13]).

## Introduction

As a streaming framework Storm provides a set of primitives, aggregates and transformation functions. Storm consists of a scheduler, the supervisor and a ui frontend. The Scheduler is called nimbus and is used for job distribution. A Apache Zookeeper cluster is beeing used for the worker nodes. In the ui frontend it is possible to see the current state of the storm cluster. Following table lists facts about Storm:

| Fact                | Description |
| :--- | :--- |
| Software developer  | Nathan Marz (Project lead) |
| Stable release      | version 0.8.2, date 11 Jan 2013 |
| Development status  | Active |
| Development release | 0.9.0 release candidate 3 |
| Language            | Clojure |
| Operating system    | Cross-platform (Microsoft Windows - Cygwin environment) |
| Type                | Complex Event Processing |
| Subtype             | Streaming |
| License             | Eclipse Public License 1.0 (Moving to Apache License version 2.0 (Incubator process)) |
| Website             | [Storm-project](http://storm-project.net), [Apache incubator-storm source code](https://github.com/apache/incubator-storm) |
| Community           | [GitHub](https://github.com/nathanmarz/storm), [Apache Incubator Project Storm](http://incubator.apache.org/projects/storm.html), [Apache Incubator Storm Proposal](http://wiki.apache.org/incubator/StormProposal) |


[MARZ13]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#MARZ13  "Apache Storm Proposal"