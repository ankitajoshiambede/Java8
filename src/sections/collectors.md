
# Collectors class

- Implementation of Collector interface that implements reduction operations (accumulating elements into collection, summarize elements according to some criteria)

|  Operation | Description | Input | Return Type | Example| Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| toList() | 	Returns a Collector that accumulates the input elements into a new List. | NA | ? | stream.collect(Collectors.toList())| |
| toSet() | Returns a Collector that accumulates the input elements into a new Set. | NA | ? | stream.collect(Collectors.toSet()) | |
| toCollection() | Returns a Collector that accumulates the input elements into a new Collection, in encounter order. | Supplier| Collector | stream.collect(Collectors.toCollection(()->new TreeSet<String>())| |
| counting() | Returns a Collector accepting elements of type T that counts the number of input elements.  | NA | Collector accepting elements of type T that counts the number of input elements.  |  stream.collect(Collectors.counting())| |
| | | | | | | 
| | | | | | |

Following are overloaded methods of toMap which returns a Collector that accumulates elements into a Map whose keys and values are the result of applying the provided mapping functions to the input elements.
|  Operation | Description | Input | Return Type | Example|  Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| toMap()| If the mapped keys contains duplicates (according to Object.equals(Object)), an IllegalStateException is thrown  | Function, Function | Map | stream.collect(Function.identity(), s->s.length())| |
| toMap()| If the mapped keys contains duplicates (according to Object.equals(Object)), the value mapping function is applied to each equal element, and the results are merged using the provided merging  function.| Function keyMapper, Function valueMapper, BinaryOperator mergingFunction| ? | stream.collect(Function.identity(), String::length, (s,a)->s+a)| |


Following are overloaded methods of joining() which returns a Collector that concatenates the input elements into a String, in encounter order.
|  Operation | Description | Input | Return Type | Example|  Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| joining() | joins without and delimiter prefix or suffix | NA| | stream.collect(Collector.joining()) | |
| joining() | separated by the specified delimiter | CharSequence delimeter| | stream.collector(Collector.joining(" , ")) | |
| joining() | separated by the specified delimiter, with the specified prefix and suffix | CharSequence delimeter, CharSequence prefix, CharSequence suffix| | stream.collect(Collector.joining(" , ", "{", "}"))| |


Following are overloaded methods of partitioningBy() which returns a Collector which partitions the input elements according to a Predicate, There are no guarantees on the type, mutability, serializability, or thread-safety of the Map returned.
|  Operation | Description | Input | Return Type | Example|  Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| partitioningBy() | | Predicate | Collector implementing the partitioning operation | stream.collect(Collectors.partitioningBy(s->s.contains("fruit")));| |
| partitioningBy() | reduces the values in each partition according to another Collector | Predicate, Collector | Collector implementing the cascaded partitioning operation | stream.collect(Collectors.partitioningBy(s->s.contains("fruit"), Collectors.counting())| |



Following are overloaded methods of groupingBy() which returns a Collector implementing a "group by" operation on input elements , grouping elements according to a classification functio , There are no guarantees on the type, mutability, serializability, or thread-safety of the Map returned.

|  Operation | Description | Input | Return Type | Example|  Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| groupingBy() | | | | | 
| groupingBy() | implementing a cascaded "group by" operation on input elements, grouping elements according to a classification function, and then performing a reduction operation on the values associated with a given key using the specified downstream Collector. | Function, Collector | | |
| groupingBy() |  implementing a cascaded "group by" operation on input elements, grouping elements according to a classification function, and then performing a reduction operation on the values associated with a given key using the specified downstream Collector. The Map produced by the Collector is created with the supplied factory function. | Function, Supplier, Collector | | stream().collect(groupingBy(Person::getCity, TreeMap::new,
                                              mapping(Person::getLastName, toSet())) |

