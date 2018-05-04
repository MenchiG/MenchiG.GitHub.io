---
layout: post
title: Java8 Lambda
key: 0008
tags: Java note Lambda
category: blog
---

Java8 Lambda Note <!--more-->

## What is Lambda

Lambda expression is an anonymous function. We can store a function into a variable like

```java
aBlockOfCode = (s) -> System.out.println(s);
aBlockOfCodes = (s) -> {if(!s.isEmpty)
    					 System.out.println(s);};
```

But how to define the class for the variable `aBlockOfCode` ? Actually, `aBlockofCode` is an interface. All lambda expression is an interface in Java. Which is called **Functional Interface**

```java
@FunctionalInterface
interface MyLambdaInterface {
    void doSomething(String s);
}
```

And we have

```java
MyLambdaInterface aBlockOfCode = (s) -> System.out.println(s);
```

With Functional Interface, is not necessary to implements the interface's method by ```@Override```. Instead, the method is defined by the lambda expression when the it is assigned to the variable.

Then we can call the method.

```java
aBlockOfCode.doSomething("Hello world");
```
### As a parameter

Lambda expression is able to be transfer to a function as a parameter.

```java
public static void enact(MyLambdaInterface myLambda, String s) {
    myLambda.doSomething(s);
}
```

#### Interface

```java
@FunctionalInterface
interface MyLambdaInterface {
    void doSomething(String s);
}
```

#### Lambda Style

```java
enact(s -> System.out.println(s), "Hello World");
```

#### Traditional Style 

It is necessary to implement `MyLambdaInterface`

```java
public class MyLambdaInterfaceImp implements MyLambdaInterface {
    @Override
    public void doSomethin(String s) {
        System.out.println(s);
    }
}
```

```java
MyLambdaInterface anInterfaceImpl = new MyLambdaInterfaceImp();
enact(anInterfaceImpl, "Hello World")
```

In this case, compared with traditional style, there isn't a class `MyLambdaInterfaceImpl`to implement `MyLambdaInterface`.

## The `java.util.function` Package

Java has designed some Functional Interface with different return value types and input parameters. [More](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

- `Predicate<T>`: Represents a predicate (boolean-valued function) of one argument.
- `Consumer<T>`:Represents an operation that accepts a single input argument and returns no result.
- `Function<T,R>`: Represents a function that accepts one argument and produces a result.
- `Supplier<T>`: Provide an instance of a T (such as a factory)
- `UnaryOperator<T>`: A unary operator from T -> T
- `BinaryOperator<T>`: A binary operator from (T, T) -> T

For more function:

https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html

By this package, we do need to define `MyLambdaInterface`everytime. 

## Stream

Here is a example about map-reduce transformations on collections.

```java
Collection<Widget> widgets = Widget.generateSomeData();

int sum = widgets.stream()
                      .filter(b -> b.getColor() == RED)
                      .mapToInt(b -> b.getWeight())
                      .sum();
```

- No storage. A stream is not a data structure that stores elements; instead, it conveys elements from a source such as a data structure, an array, a generator function, or an I/O channel, through a pipeline of computational operations.

- Functional in nature. An operation on a stream produces a result, but does not modify its source. For example, filtering a `Stream` obtained from a collection produces a new `Stream` without the filtered elements, rather than removing elements from the source collection.

- Laziness-seeking. Many stream operations, such as filtering, mapping, or duplicate removal, can be implemented lazily, exposing opportunities for optimization. For example, "find the first `String` with three consecutive vowels" need not examine all the input strings. Stream operations are divided into intermediate (`Stream`-producing) operations and terminal (value- or side-effect-producing) operations. Intermediate operations are always lazy.

- Possibly unbounded. While collections have a finite size, streams need not. Short-circuiting operations such as `limit(n)` or `findFirst()` can allow computations on infinite streams to complete in finite time.

- Consumable. The elements of a stream are only visited once during the life of a stream. Like an [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html), a new stream must be generated to revisit the same elements of the source.

#### Reduction operations

- reduce : return single value
- collect : return collection

## Method reference

*And if this just calls one method, you can use* **A METHOD REFERENCE**

### Static method reference

`(args) -> Class.staticMethod(args)` ==>  `Class::staticMethod`

### Instance method reference of an object of a particular type

`(obj, args) -> obj.instanceMethod(args)` ==> `ObjectType::instanceMethod`

### Constructor method reference

`(args) -> new ClassName(args)` ==> `ClassName::new`

## Optional 

A container object which may or may not contain a non-null value. If a value is present, `isPresent()` will return `true` and `get()` will return the value. [More](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)

### `map` vs `flatMap`

map is If a value is present, apply the provided mapping function to it, and if the result is non-null, return an `Optional`describing the result. but the return value of flatMap must be `Optional` 

### `orElse` VS `orElseGet` and `orElseThrow`

`orElse` accept a String as default value

`orElseGet` and` orElseThrow` need a `Supplier` Interface to create default value or throw exception