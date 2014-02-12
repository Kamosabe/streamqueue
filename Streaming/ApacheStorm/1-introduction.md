# Apache Storm

According to Nathan Marz Apache Storm "is a distributed, fault-tolerant, and high-performance realtime computation system that provides strong guarantees on the processing of data" ([MARZ13]).

## Introduction

As a streaming framework Storm provides a set of primitives, aggregates and transformation functions. Storm consists of a scheduler, the supervisor and a ui frontend. The Scheduler is called nimbus and is used for job distribution. An Apache Zookeeper cluster is used for the worker nodes. In the ui frontend it is possible to see the current state of the storm cluster. The maintanance of storm is done in configuration files and is simple to use. Via command tools an administrator can scale or redeploy a storm topology in a running cluster. Every task is executed by a worker node which is executed on an Apache Zookeeper node. Apache Zookeeper ist meant to fail fast. In a supervision the worker will be restarted from the scheduler *storm nimbus*. Extra monitoring tools like nagios will increase stability in running a storm topolgy. Following table lists facts about Storm:

| Fact                | Description |
| :--- | :--- |
| Software developer  | Nathan Marz (Project lead) |
| Stable release      | version 0.9.0.1, date 12 Feb 2014 |
| Development status  | Active |
| Development release | 0.9.1 |
| Language            | Clojure |
| Operating system    | Cross-platform (Microsoft Windows - Cygwin environment) |
| Type                | Complex Event Processing |
| Subtype             | Streaming |
| License             | Eclipse Public License 1.0 (Moving to Apache License version 2.0 (Incubator process)) |
| Website             | [Storm-project](http://storm-project.net), [Apache incubator-storm source code](https://github.com/apache/incubator-storm) |
| Community           | [Apache Incubator Project Storm](http://incubator.apache.org/projects/storm.html), [Apache Storm Bugtracker](https://issues.apache.org/jira/browse/STORM), [Apacher Storm Source code](https://git-wip-us.apache.org/repos/asf?p=incubator-storm.git), [Apache Incubator Storm Proposal](http://wiki.apache.org/incubator/StormProposal), [GitHub old Source code (until 13 Dec 2013)](https://github.com/nathanmarz/storm) |


[MARZ13]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#MARZ13  "Apache Storm Proposal"


