# Java 8 Streams

## Different ways to create stream
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

### Comparator Interface
- It is functional interface
- for defining custom sorting logic for objects
- Typically passed to sorting methods like _Collections.sort()_ , _Arrays.sort()_, terminal operations in Stream API such as _min()_, _max()_
- 

#### comparing() method
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

#### naturalOrder()  
- default sorting order for objects
- static method that returns Comparator instance, which compares objects that implements Comparable interface based on natural ordering

  ```
  List<Integer> numbers = Arrays.asList(5, 2, 8, 1);
  numbers.sort(Comparator.naturalOrder()); // Sorts the list in ascending order
  numbers.forEach(System.out::println);
  ```
#### reverseOrder() 

### Other
- The chars() method is available on String, StringBuffer, and CharBuffer objects. This method returns an IntStream representing the Unicode code points of the characters within the sequence.


### Intermediate operations
- We can chain multitple intermediate operations together 

|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| filter()  | Filters the element  | Predicate | Stream |   |
| map()  |  | | | |
|  flatMap() |   | | | |
|  distinct() |  Removes duplicate elements | NA | Stream | stream.distinct() |
| sorted()  |Elements sorted according to natural order. | NA | Stream | |
| sorted()  | Elements sorted according to provided comparator.| Comparator | Stream | |
|  limit() |   |  |  | |
| skip()  |   | | | |
| peek()  |   | | | |
| mapToObj()  |  Returns an object-valued Stream consisting of the results of applying the given function to the elements of this stream. |IntFunction | Stream | |




### Terminal Operations


|  Operation | Description | Input | Return Type | Example| 
| ------------- | ------------- |------------- |------------- |-------------  |
| forEach()  | performs action for each element  | | | |
| toArray()  | collects element into an array  | | | |
|  reduce() |   | | | |
|  collect() | collects element into collection (e.g. list, set)  | | Collection| |
| min()  | find minimum  element in stream based on comparator or natural sorting  | Comparator | Optional | stream.min(Comarator.naturalOrder())|
|  max() | find maximum element in stream based on comparator or natural sorting   | Comparator | Optional | stream.max(Comparator.comparing(ClassName::methodName))|
| count()  |   | | | |
| anyMatch()  |   | | | |
| allMatch()  |   | | | |
| noneMatch()  |   | | | |
| findFirst()  |   | | | |
|  findAny() |   | | | |

#### collect()



