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

### example
consider following 
```
class MyRunnable implements Runnable{
   public int run(){
      for(int i = 0; i < 10; i++){
         System.out.println("Child thread");
      }
   }
}

class Test{
   public static void main(){
      MyRunnable r = new MyRunnable();
      Thread t = new Thread(r);
      t.start();

      for(int i = 0; i < 10; i++){
         System.out.println("Parent thread");
      }
   }
}
```

Converting above to lambda expression

```
class Test{
   public static void main(){
      MyRunnable r = ()->{
         for(int i = 0; i < 10; i++){
               System.out.println("Child thread");
            }
      };
      Thread t = new Thread(r);
      t.start();

      for(int i = 0; i < 10; i++){
         System.out.println("Parent thread");
      }
   }
}
```

### example
A Comparator in Java is an interface used to define custom sorting logic for objects. It is part of the java.util package.
<br>
Comparator has only one abstract method, so it can be used with lambda expressions.

```
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

compare(T o1, T o2) method
- Returns negative → o1 comes before o2
- Returns zero → equal elements
- Returns positive → o1 comes after o2

 Consider the following example which sorts list in ascending order using comparator

```
class MyComparator implements Comparator<Integer>{
   public int compare(Integer I1, Integer I2){
      if(I1 < I2){
         return -1;
      }
      if(I1 > I2){
         return 1;
      }
      else{
         return 0;
      }
   }
}

class Test{
   public static void main(){
      ArrayList<Integer> list = new ArrayList<Integer>();
      l.add(10);
      l.add(25);
      l.add(20);
      l.add(5);
      l.add(87);
      l.add(92);

      Collections.sort(list, new MyComparator());
      System.out.println("List in ascending order : "+ l);
   }
}
```


We will convert above in lambda expression as below 

```
class Test{
   public static void main(){
      ArrayList<Integer> list = new ArrayList<Integer>();
      l.add(10);
      l.add(25);
      l.add(20);
      l.add(5);
      l.add(87);
      l.add(92);
      Comparator<Integer> lambdaComparator = (I1, I2)-> (I1 <I2) ? -1: (I1 > I2) ? 1:0;
      Collections.sort(list, lambdaComparator);
      System.out.println("List in ascending order : "+ l);
   }
}
```

### example
Consider following example, we want to sort employee based on ascending order of employee number.

```
class Employee{
   String name;
   int eno;

   Employee(String name, int no){
      this.name = name;
      this.eno = eno;
   }

   public String toString(){
      return name+" "+eno;
   }
}

class Test{
   public static void main(){
      ArrayList<Employee> empList = new ArrayList<>();
      empList.add(new Employee("Ankita", 3));
      empList.add(new Employee("Kiran", 1));
      empList.add(new Employee("Aishwarya", 4));
      empList.add(new Employee("Shubham", 2));
      Collections.sort(empList, (e1, e2)-> (e1.eno < e2.eno) ? -1 : ((e1.eno > e2.eno) ? 1: 0));
   }
}
```


### example 
Consider following example, we want to sort employee based on ascending alphabetical order of employee name.

Comparing Strings Lexicographically (Alphabetical Order): compareTo()
The compareTo() method compares two strings lexicographically (based on the Unicode value of each character). It returns:
-  A negative integer if the calling string comes before the argument string.
- Zero if the strings are equal.
- A positive integer if the calling string comes after the argument string.
  
```
class Employee{
   String name;
   int eno;

   Employee(String name, int no){
      this.name = name;
      this.eno = eno;
   }

   public String toString(){
      return name+" "+eno;
   }
}

class Test{
   public static void main(){
      ArrayList<Employee> empList = new ArrayList<>();
      empList.add(new Employee("Ankita", 3));
      empList.add(new Employee("Kiran", 1));
      empList.add(new Employee("Aishwarya", 4));
      empList.add(new Employee("Shubham", 2));
      Collections.sort(empList, (e1, e2)-> (e1.name.compareTo(e2.name);
   }
}
```

Comparable and Comparator are Functional interfaces and we can use lambda expression for sorting.

## Anonymous inner class and Lambda expression

Anonymous inner class: class which does not have name
