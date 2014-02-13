## APIs

In this chapter the using of the storm api is shown.

At the point of this writing only the programming languages [java](http://www.java.com) and [clojure](http://clojure.org/) are supported in writing program code. Because *clojure* targets on a Java Virtual Machine, only the java programming language is considered.

### Trident

In chapter *description* was shown how to use a basic topology by parsing sentences and counting words. Theres is also a high-level abstraction layer over storm called Trident. Because Trident is an abstraction all primitives like topology, spouts and bolts are available. The extension in Trident is a more flexible and easier processing of data in a small description of a transformation of data. It has joins, aggregations, grouping, functions, and filters. Following source code shows the Trident version of the WordCountTopology which was shown in chapter *description*.

```
TridentTopology tridentTopology = new TridentTopology();
TridentState wordCounts = tridentTopology
				.newStream("spout1", new RandomSentenceSpout())
				.each(new Fields("sentence"), new Split(), new Fields("word"))
				.groupBy(new Fields("word"))
				.persistentAggregate(MemcachedState.transactional(location), new Count(), new Fields("count"))
				.parallelismHint(6);
```

The RandomSentenceSpout() class was used from the WordCountTopology example before, which uses the multilang feature to import data with python. A basic storm function splits a sentences by the space char. Manipulation of a Tuple can be done by using of a implementation of a BaseFunction or a BaseFilter. Following source code shows the extended BaseFunction Split.

````
public class Split extends BaseFunction {
	public void execute(TridentTuple tuple, TridentCollector collector) {
		String sentence = tuple.getString(0);
		for(String word: sentence.split(" ")) {
			collector.emit(new Values(word));
		}
	}
}
```

The persistentAggregate() function built on top of partitionPersist takes an argument to store a source of state. A memcahed store is used as a store. For Aggregators Trident provides following primitives: CombinerAggregator, ReducerAggregator, BaseAggregator. The *parallelism hint* means a starting number of threads of a component. Default property is one one task in one thread.