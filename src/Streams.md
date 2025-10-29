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


# Collectors class

- Implementation of Collector interface that implements reduction operations (accumulating elements into collection, summarize elements according to some criteria)

|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| toList() | 	Returns a Collector that accumulates the input elements into a new List. | NA | ? | stream.collect(Collectors.toList())|
| toSet() | Returns a Collector that accumulates the input elements into a new Set. | NA | ? | stream.collect(Collectors.toSet()) |
| toCollection() | Returns a Collector that accumulates the input elements into a new Collection, in encounter order. | Supplier| Collector | |


Following are overloaded methods of toMap which returns a Collector that accumulates elements into a Map whose keys and values are the result of applying the provided mapping functions to the input elements.
|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| toMap()| If the mapped keys contains duplicates (according to Object.equals(Object)), an IllegalStateException is thrown  | Function, Function | Map | stream.collect(Function.identity(), s->s.length())|
| toMap()| If the mapped keys contains duplicates (according to Object.equals(Object)), the value mapping function is applied to each equal element, and the results are merged using the provided merging function.| Function keyMapper, Function valueMapper, BinaryOperator mergingFunction| ? | stream.collect(Function.identity(), String::length, (s,a)->s+a)|



# Intermediate operations
- We can chain multitple intermediate operations together 

|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| filter()  | Filters the element  | Predicate | Stream |  stream.filter((String) name -> name.length >=5) |
| map()  | Transform each element | Function | Stream | stream.map((String) name -> name.toLowerCase())|
|  flatMap() |  Iterate over each element of complex collection(e.g. list of list), and helps to flatten it | Function |Stream |  stream.flatMap(List<String> sentence -> sentence.stream()); // given List<List<String>> stream and it returns Stream<String>|
|  distinct() |  Removes duplicate elements from stream| NA | Stream | stream.distinct() |
| sorted()  |Elements sorted according to natural order. | NA | Stream | stream.sorted()|
| sorted()  | Elements sorted according to provided comparator.| Comparator | Stream | stream.sorted((Integer val1, Integer val2) -> val2-val1 // sorts in desc order)|
|  limit() |  Truncate the stream, to given limit size | Long | Stream | stream.limit(3) |
| skip()  |  Skips first given elements of stream | Long | Stream | stream.skip(3) |
| peek()  | helps see intermediate result of stream which is getting processed  | Consumer| Stream | stream.peek((Integer value)-> System.out.println(val));|
| mapToObj()  |  Returns an object-valued Stream consisting of the results of applying the given function to the elements of this stream. |IntFunction | Stream | |
| mapToInt()  |  Returns an IntStream consisting of the results of applying the given function to the elements of this stream. |ToIntFunction | IntStream | |
| mapToLong()  |  Returns an LongStream consisting of the results of applying the given function to the elements of this stream. |ToLongFunction | LongStream | |
| mapToDouble()  |  Returns an DoubleStream consisting of the results of applying the given function to the elements of this stream. |ToDoubleFunction | DoubleStream | |

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

# Terminal Operations
- It triggers processing of stream
- One terminal operation is used on a stream , it is closed/consumed and cannot be used again for other terminal operation

|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| forEach()  | performs action for each element  | Consumer| NA | stream.forEach(System.out::println) |
| toArray()  | collects elements of stream into an array  |NA | Object[]) | Object[] ans = stream.toArray()|
| toArray()  | collects elements of stream into an array  |IntFunction | A[] where  A is the element type of the resulting array | Person[] men = people.stream().toArray(Person[]::new); Integer[] intArray = numberStream.toArray((int size)-> new Integer[size]);|
|  reduce() |  does reduction on elements , using provided assosciative aggregation function | BinaryOperator|Optional or value |stream.reduce((Integer val1, Integer val2) -> val1+val2) |
|  reduce() |  does reduction on elements , using provided assosciative aggregation function and provided identity value| T identity, BinaryOperator|T |stream.reduce(0, Integer::sum) |
|  collect() | collects element into collection (e.g. list, set)  | Collector | Collection| stream.collect(Collectos.toList())|
| min()  | find minimum  element in stream based on comparator or natural sorting  | Comparator | Optional | stream.min(Comarator.naturalOrder())|
|  max() | find maximum element in stream based on comparator or natural sorting   | Comparator | Optional | stream.max(Comparator.comparing(ClassName::methodName))|
| count()  |  Number of elements present in stream | NA| long| stream.count()|
| anyMatch()  |  Checks if any value in stream match given predicate  | Predicate| Boolean | stream.anyMatch((Integer val)-> val >3)|
| allMatch()  |  Checks if all value in stream match given predicate   | Predicate | Boolean | |
| noneMatch()  | Checks if none value in stream match given predicate    |Predicate | Boolean| |
| findFirst()  | Find first element of stream | NA|Optional |stream.findFirst() |
|  findAny() | Returns any element, useful in parallel stream | NA | Optional | stream.findAny() |

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

# Other
- The chars() method is available on String, StringBuffer, and CharBuffer objects. This method returns an IntStream representing the Unicode code points of the characters within the sequence.


# Questions 
- in Java 17 you can use .toList() instead of .collect(Collectors.toList()) on a stream, whats difference (??), difference in Collectors.toCollection() and Collectors.toList()
 .toList() guarantees the returned List is immutable. However, Collectors.toList() makes no promises about immutability. The result might be immutable. 
I dint find in stream documentation toList method https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

- Difference in Collectors.counting() and count() which one to use when
- 
