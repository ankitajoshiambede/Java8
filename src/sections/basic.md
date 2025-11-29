## Definitions
Functional programming is the art of composing programs with functions. This programming paradigm treats functions as “first-class citizens,” which in computer science means you can bind them to names, pass them as arguments, and return them from other functions just like any other data type.

## 1.8 version
Released on March 18, 2014,  <br><br>It introduced revolutionary features that transformed the way Java is used.
<br><br>It offered following benefits <br>
1. To simplify programming using functional programming features

   - Streams make code more readable, concise, and expressive.

   - They reduce boilerplate compared to traditional loops.

2. To bring functional programming benefits into an object-oriented language
    - Java is primarily object-oriented, not functional.

    - To enjoy functional-style operations (map, filter, reduce), Java 8 introduced streams and lambda expressions.

3. To enable easy parallel programming

   - Before Java 8, Java wasn’t efficiently using multi-core processors for parallel tasks.

   - Java 8 introduced parallel streams, making it easier to perform operations in parallel without complex thread handling.

#### Major Features in Java 1.8 version
- Lambda expressions
- Functional interfaces
- Default and static methods in interfaces
- Method references
- Streams API
- Optional class
- Date and Time API (java.time package)

#### Java 1.2 version
- Collections Framework
- Swing GUI toolkit
- Java 2D API
- JIT Compiler
- Java Plug-in and Java Web Start

#### Java 1.5 version
- Generics
- Enhanced for loop (for-each loop)
- Autoboxing and unboxing
- Varargs
- Annotations
- Enumerations (enums)
- Static imports
- Concurrency utilities (java.util.concurrent package)    

---

## Basic lambda expressions

- Enables functional programming in Java by allowing functions to be treated as first-class citizens (can be passed as an argument, returned from a function, and assigned to a variable).
- Provides a clear and concise way to represent a single method interface using an expression.
- Syntax: `(parameters) -> expression` or `(parameters) -> { statements; }`
- To use APIs that support functional programming, such as the Streams API.

Lambda expression is anonymous function (function without name, return type and modifier)

#### Example 1

Consider following method which I want to convert in lambda expression
```
public void m1(){
  System.out.println("Hello World");
}
```

Above method is converted into lambda expression as follow:
```
() -> { System.out.println("Hello World"); }
```

If body of lambda expression contain only one line then curly braces are optional
```
() ->  System.out.println("Hello World"); 
```


#### Example 2

Consider following method which I want to convert in lambda expression
```
public void m1(int a , int b){
  System.out.println("Hello World");
}
```
Above method is converted into lambda expression as follow: data type of arguments can be skipped as in some cases compiler can guess type automatically
```
(a,b) ->  System.out.println(a+b); 
```

#### Example 2
Consider following method which I want to convert in lambda expression
```
public void m1(int a , int b){
  return a+b
}
```
Above method is converted into lambda expression as follow: When writing lambda expressions, if you omit the curly braces, you cannot use the return keyword — the expression itself is the return value.
However, if you include curly braces in the lambda body, then you must explicitly use the return statement to return a value.
```
(a,b) ->  a+b; 
```
#### Example 2
Consider following method which I want to convert in lambda expression
```
public void m1(int n){
  return n*n
}
```
Above method is converted into lambda expression as follow: If only one parameter is used we can skip () paranthesis
```
n->n*n
```

#### Example 2
Consider following method which I want to convert in lambda expression
```
public void strLength(int str){
  return str.length()
}
```
Above method is converted into lambda expression as follow
```
s->s.length()
```

#### To call lambda expression

To invoke lambda expression , functional interface is required

Runnable, Comparable, ActionListender, Callable are functional interface. All these interfaces contain single abstract method(SAM). An interface which contains SAM is functional interface.
