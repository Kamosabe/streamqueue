# Apache S4

On the Apache web page S4 is described as 'a general-purpose, distributed, scalable, fault-tolerant, pluggable platform that allows programmers to easily develop applications for processing continuous unbounded streams of data' ([JUNQ11]).

## Introduction

In S4 there is a Processing Element (PE) which consumes an event, emit one or more events or publish the results. Emitted events can be consumed by other Processing Elements. The implementation of a Processing Element can be for example a quote splitter and another Processing Element based on the quote splitter can be a word counter.

| Fact                | Description |
| :--- | :--- |
| Software developer  | Matthieu Morel (Lead developer) |
| Stable release      | version 0.5.0, date 15 Aug 2012 |
| Development status  | Active |
| Development release | 0.6, 0.7 |
| Language            | Java |
| Operating system    | Cross-platform (Microsoft Windows - Cygwin environment not supported) |
| Type                | Complex Event Processing |
| Category            | Streaming |
| License             | Apaching License version 2.0 |
| Website             | [S4](http://incubator.apache.org/s4/), [Apache incubator-s4 source code](https://github.com/apache/incubator-s4) |
| Community           | [GitHub](https://github.com/s4/s4), [Apache Incubator Project S4](http://incubator.apache.org/projects/s4.html), [Apache Incubator S4 Proposal](http://wiki.apache.org/incubator/S4Proposal) |


[JUNQ11]: https://github.com/edlich/streamqueue/blob/master/Streaming/references.md#JUNQ11  "Apache S4 Proposal"