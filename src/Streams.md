# Stream

## Table of Content

  - [Basic]()
  - [Collectors class]()
  - [Ways to create Stream]()
  - [Intermediate Operations]()
  - [Terminal Operations]()
  - [Examples]()
  - [Other Questions]()


## Use when
* Use *for* loop when
  * Dataset is very small because they don’t have the small overhead of building a Stream pipeline.
  * Logic is simple and sequential
  * Max control over iteration, index based access 
 
* Use *stream()* when (even for small data)
  * cleaner, declarative transformations, more functional style code
  * value readability and maintainability
  * when applying multiple transformations (filter -> map -> sort -> collect)
  * to avoid mutable data structures or index based loops
  * may later switch to parallel processing — it’s just .parallelStream() away.

* Use *parallelStream()* when
  * Dataset is large (e.g., 100k+ elements)
  * Each operation is CPU-intensive (not just I/O)
  * Have multiple CPU cores

👉 Yes, you can use Stream API for small datasets.
It’s not about size — it’s about code clarity, maintainability, and style.

## Definition
- Consider it as pipeline through which our collection elements passes through.
  
- A stream pipeline consists of 
1. a source( array, collection, generator function, I/O channel etc.)
2. zero or more intermediate operations (transforms stream into another stream)
3. terminal operation (produces result)

- Streams are lazy; computation on the source data is only performed when the terminal operation is initiated, and source elements are consumed only as needed.





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


# Different ways to create stream
1. From collection
  ```
List<Integer> salartList = Arrays.asList(3000000,4000000,2500000)
Stream<Integer> streamFromList = salaryList.stream();
  ```

2. From Array

```
Integer[] salaryArray = {3000000, 40000000, 25000000}
Stream<Integer> streamFromArray = Arrays.stream(salaryArray);
```

3. From static method

```
Stream<Integer> streamFromStaticMethod = Stream.of(30000000, 25000000, 3500000)
```
4. From stream builder
```
Stream.Builder<Integer> streamBuilder = Stream.builder();
streamBuilder.add(3000000);
streamBuilder.add(25000000);

Stream<Integer> streamFromStreamBuilder = streamBuilder.build();
```

5. From stream Iterate

```
Stream<Integer> streamFromStreamIterate = Stream.iterate(1000000, (Integer n) -> n+ 10000000 ).limit(12000);
```

Provide limit() , or else it creates infinite stream.

# Intermediate operations
- We can chain multitple intermediate operations together
- Intermediate operations in a Java stream are immutable and always create and return a new stream. 
- Lazy in nature they gets executed only when terminal operation is invoked,  entire pipeline is not executed until a final result is requested.

|  Operation | Description | Input | Return Type | Example|  Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| stream() | Converts a collection into a stream |  |  |  |  |
| filter()  | Filters the element  | Predicate | Stream |  stream.filter((String) name -> name.length >=5) | |
| map()  | Transform each element | Function | Stream | stream.map((String) name -> name.toLowerCase())| |
|  flatMap() |  Iterate over each element of complex collection(e.g. list of list), and helps to flatten it | Function |Stream |  stream.flatMap(List<String> sentence -> sentence.stream()); // given List<List<String>> stream and it returns Stream<String>| |
|  distinct() |  Removes duplicate elements from stream| NA | Stream | stream.distinct() | |
| sorted()  |Elements sorted according to natural order. | NA | Stream | stream.sorted()| |
| sorted()  | Elements sorted according to provided comparator.| Comparator | Stream | stream.sorted((Integer val1, Integer val2) -> val2-val1 // sorts in desc order)| |
|  limit() |  Truncate the stream, to given limit size | Long | Stream | stream.limit(3) | |
| skip()  |  Skips first given elements of stream | Long | Stream | stream.skip(3) | |
| peek()  | helps see intermediate result of stream which is getting processed  | Consumer| Stream | stream.peek((Integer value)-> System.out.println(val));| |
| mapToObj()  |  Returns an object-valued Stream consisting of the results of applying the given function to the elements of this stream. |IntFunction | Stream | | |
| mapToInt()  |  Returns an IntStream consisting of the results of applying the given function to the elements of this stream. |ToIntFunction | IntStream | | |
| mapToLong()  |  Returns an LongStream consisting of the results of applying the given function to the elements of this stream. |ToLongFunction | LongStream | | |
| mapToDouble()  |  Returns an DoubleStream consisting of the results of applying the given function to the elements of this stream. |ToDoubleFunction | DoubleStream | | |
| boxed()  |   | |  | | |

## mapToInt
Below is one way to create primitive data type stream
```
int[] numArray = {1, 2, 3, 4};
IntStream streamOfInt = Arrays.stream(numArray);
```

mapToInt used to create primitive data type stream as below
```
List<String> numbers = Arrays.asList("1", "3", "7", "12");
IntStream integerStream = numbers.stream().mapToInt((String val) -> Integer.parseInt(val));
```

## Difference between map() and flatMap()
- map() transforms each element of the stream into another element. It is a one-to-one mapping.
  - Use map() when you have a stream of elements and want to apply a function to each element.

- flatMap() transforms + flattens the resulting streams into one stream, usually when dealing with collections of collections.
  - Use flatMap() when each element can be mapped to zero or more elements (when dealing with nested collections)

```

        class Person {
            String name;
            List<String> colors;
            Person(String name, List<String> colors) {
                this.name = name;
                this.colors = colors;
            }
            public String toString() { return name; }
            public List<String> getColors() { return colors; }
        }

        List<Person> people = Arrays.asList(
          new Person("Alice", Arrays.asList("Red","Blue")),
          new    Person("Bob", Arrays.asList("Green","Yellow"))
        );

        List<List<String>> colorsListByMap = people.stream().map(Person::getColors).collect(Collectors.toList());
        System.out.println(colorsListByMap);

        List<String> colorsListByFlatMap = people.stream().flatMap(person -> person.getColors().stream()).collect(Collectors.toList());
        System.out.println(colorsListByFlatMap);
```
# Terminal Operations
- It triggers processing of stream
- One terminal operation is used on a stream , it is closed/consumed and cannot be used again for other terminal operation, If stream is re-used it throws IllegalStateException

|  Operation | Description | Input | Return Type | Example|    Output|
| ------------- | ------------- |------------- |------------- |-------------  |-------------  |
| forEach()  | performs action for each element  | Consumer| NA | stream.forEach(System.out::println) | |
| toArray()  | collects elements of stream into an array  |NA | Object[]) | Object[] ans = stream.toArray()| |
| toArray()  | collects elements of stream into an array  |IntFunction | A[] where  A is the element type of the resulting array | Person[] men = people.stream().toArray(Person[]::new); Integer[] intArray = numberStream.toArray((int size)-> new Integer[size]);| |
|  reduce() |  does reduction on elements , using provided assosciative aggregation function | BinaryOperator|Optional or value |stream.reduce((Integer val1, Integer val2) -> val1+val2) | |
|  reduce() |  does reduction on elements , using provided assosciative aggregation function and provided identity value| T identity, BinaryOperator|T |stream.reduce(0, Integer::sum) | |
|  collect() | collects element into collection (e.g. list, set)  | Collector | Collection| stream.collect(Collectos.toList())| |
| min()  | find minimum  element in stream based on comparator or natural sorting  | Comparator | Optional | stream.min(Comarator.naturalOrder())| |
|  max() | find maximum element in stream based on comparator or natural sorting   | Comparator | Optional | stream.max(Comparator.comparing(ClassName::methodName))| |
| count()  |  Number of elements present in stream | NA| long| stream.count()| |
| anyMatch()  |  Checks if any value in stream match given predicate  | Predicate| Boolean | stream.anyMatch((Integer val)-> val >3)| |
| allMatch()  |  Checks if all value in stream match given predicate   | Predicate | Boolean | | |
| noneMatch()  | Checks if none value in stream match given predicate    |Predicate | Boolean| | |
| findFirst()  | Find first element of stream | NA|Optional |stream.findFirst() | |
|  findAny() | Returns any element, useful in parallel stream | NA | Optional | stream.findAny() | |

## collect()

# Parallel stream
Helps to perform operation on stream concurrently, taking advantage of multi core CPU.
ParallelStream() method is used instead of regular stream() method.
Internally it does:
- Task splitting : it uses "spliterator" function to split the data into multiple chunks.
- Task submission and parallel processing : Uses Fork-Join pool technique.
  
# Comparator Interface
- It is functional interface
- for defining custom sorting logic for objects
- Typically passed to sorting methods like _Collections.sort()_ , _Arrays.sort()_, terminal operations in Stream API such as _min()_, _max()_
- 

## comparing() method
- takes Function as input

  ```
  class Book{
    private String title;
    private Int price;

    Book(String title, Int price){this.title = title; this.price = price;}
    public String getTitle(){return title;}
    public Int getTitle(){return title;}
  }

  List<Books> books = new ArrayList<>();
  books.add("Java", 20000);
  books.add("Head First Java" , 20000);

  // method reference syntax
  Collections.sort(books, Comparator.comparing(Book::getTitle));

  // lambda syntax
  Collections.sort(books,s Comparator.comparing((Book) b -> b.getTitle()));
  ```

## naturalOrder()  
- default sorting order for objects
- static method that returns Comparator instance, which compares objects that implements Comparable interface based on natural ordering

  ```
  List<Integer> numbers = Arrays.asList(5, 2, 8, 1);
  numbers.sort(Comparator.naturalOrder()); // Sorts the list in ascending order
  numbers.forEach(System.out::println);
  ```
## reverseOrder() 

## Stream and immutability
- Streams encourage immutability — you create a new collection as output instead of mutating the original. The original collection remains unchanged. This makes stream operations safe, predictable, and thread-friendly.

- When you run a stream pipeline, Java expects the source collection to remain unchanged during processing. If you modify it while streaming, you’ll usually get a ConcurrentModificationException or unexpected behavior.
Why?
  - The Stream internally uses an iterator.
  - When you modify the list (add/remove) during iteration, the iterator detects that the structure changed → throws exception to prevent corruption.

✅ Safe Stream Practice

- Never modify the source collection during a stream.
- Use .map() or .collect() to produce new immutable data.
- If mutation is needed, do it after the stream finishes.
- For debugging, use .peek() — but only for logging, not changing values.


# Other
- The chars() method is available on String, StringBuffer, and CharBuffer objects. This method returns an IntStream representing the Unicode code points of the characters within the sequence.


# Questions 
- in Java 17 you can use .toList() instead of .collect(Collectors.toList()) on a stream, whats difference (??), difference in Collectors.toCollection() and Collectors.toList()
 .toList() guarantees the returned List is immutable. However, Collectors.toList() makes no promises about immutability. The result might be immutable.

I dint find in stream documentation toList method https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

---
- Difference in Collectors.counting() and count() which one to use when

---
- Difference between peek() and map()
```
class Person {
    String name;
    Person(String name) { this.name = name; }
    public String toString() { return name; }
}

List<Person> people = new ArrayList<>(List.of(new Person("Ankita"), new Person("Raj")));

people.stream()
    .peek(p -> p.name = p.name.toUpperCase()) // ❌ mutating inside stream
    .forEach(System.out::println);
```


Output:

ANKITA
RAJ


It works — but it mutated the original objects!
If another part of your code uses people, it will now see modified names.

✅ Better way (Immutable Approach):
```
List<String> upperNames = people.stream()
    .map(p -> p.name.toUpperCase()) // returns new values
    .toList();

System.out.println(upperNames); // [ANKITA, RAJ]
```

No mutation, safe and predictable.

---
## primitive stream vs normal stream
In addition to Stream ( Stream of object references),  there are primitive specializations for IntStream, LongStream, and DoubleStream.

- Primitive streams are generally more efficient for processing primitive data types as they avoid the overhead of autoboxing/unboxing.
- Primitive streams offer specialized terminal operations like sum(), average(), max(), and min() which are tailored for numerical calculations.
- While primitive streams can be collected into primitive arrays using toArray(), collecting them into standard Java collections (like List<Integer>) requires boxing them first.

- Use primitve streams (*IntStream*, *LongStream*, *DoubleStream*) for heavy numeric work, since there is no boxing/unboxing, faster comutation and less memory usage, Use normal stream for object processing or when working with collections of complex data types.


-Converting Between Normal and Primitive Streams
To convert a Stream<WrapperType> (e.g., Stream<Integer>) to its corresponding primitive stream (IntStream), you use the mapTo methods:
 - mapToInt(): Converts Stream<Integer> to IntStream.
 - mapToLong(): Converts Stream<Long> to LongStream.
 - mapToDouble(): Converts Stream<Double> to DoubleStream.

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
IntStream intStream = numbers.stream().mapToInt(Integer::intValue);
```

- Converting a Primitive Stream to a Normal Stream:
  use boxed() method: this method performs autoboxing, converting primitive values to their corresponding wrapper objects.

```
IntStream intStream = IntStream.of(1, 2, 3, 4, 5);
Stream<Integer> integerStream = intStream.boxed();
```

---

To generate stream of random numbers 
```
Stream.generate(Math::random).limit(10);
```
