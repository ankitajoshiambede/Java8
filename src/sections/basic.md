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


## @FunctionalInterface

Runnable, Comparable, ActionListender, Callable are functional interface. All these interfaces contain single abstract method(SAM). An interface which contains SAM is functional interface.
<br><br>
Normal interface and Functional interface can contain any number of static and default method from 1.8, but functional interface should contain exactly one abstract method.
<br><br>
@FunctionalInterface annotation can be used to specify explicitly that interface is functional interface , if annotation is added and if more than one abstract method are defined then compiler will give error, but annotation is not mandatory. If interface contains exactly one abstract method then it is functional interface.

- following is valid, since B contains only one abstract method inherited from interface A

```
@FunctionalInterface
interface A{
   public void m1();
}

@FunctionalInterface
interface B extends A{
}

```

- following is valid, since B overrides m1

```
@FunctionalInterface
interface A{
   public void m1();
}

@FunctionalInterface
interface B extends A{
   public void m1();
}

```

- Following Functional Interface B is invalid since it contains two abstract method, one of its own and one overriden from interface A

```
@FunctionalInterface
interface A{
   public void m1();
}

@FunctionalInterface
interface B extends A{
   public void m2();
}
```

- Following is valid , since B is normal interface it can contain any number of abstract methods


```
@FunctionalInterface
interface A{
   public void m1();
}

interface B extends A{
   public void m2();
}

```

## Lambda expression with Functional interface

- Consider below interface and implementation of interface
```
interface DemoInterface{
   public int m1();
}

class DemoClass extends DemoInterface{
   public int m1(){
      System.out.println("Hi...");
   }
}

class Test{
   public static void main(){
      DemoInterface d = new DemoClass();
      d.m1();
   }
}
```

We can implement functional interface with lambda expression as below, instead of creating DemoClass

As we learnt above , we will convert implementation of m1 to lambda expression as follow
```
()->System.out.println("Hi...");
```

and implementation is called in Test class as follow
```
class Test{
   public static void main(){
      DemoInterface d = ()->System.out.println("Hi...");
      d.m1();
   }
}
```

Consider follwing example

```
interface DemoAddInterface{
   public int add(int a, int b);
}

class DemoClass extends DemoInterface{
   public int add(int a, int b){
      return a+b;
   }
}

class Test{
   public static void main(){
      DemoInterface d = new DemoClass();
      d.m1(10,20);
   }
}
```

We will convert add to lambda expression

```
interface DemoAddInterface{
   public int add(int a, int b);
}

class Test{
   public static void main(){
      DemoInterface d = (a,b)->a+b;
      d.m1(10,20);
   }
}
```

In above example since Functional interface contain exactly one abstract method, name of abstract method and data type of arguments passed to abstract method need not be written in lambda expression.

Above will not affect performance since lambda expression are resolved one time during compilation.

