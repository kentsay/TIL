# Java 8 Streams

### What is Streams in Java 8
A Stream is a pipeline of functions that can be evaluated. Most importantly, a stream isn’t a data structure. You can often create a stream from collections to apply a number of functions on a data structure, but a stream itself is not a data structure. A stream can be composed of multiple functions that create a pipeline that data that flows through. This data cannot be mutated. That is to say the original data structure doesn’t change.

We stated that a stream is a pipeline of functions, or operations. These operations can either be classed as an intermediate operation or a terminal operation. The difference between the two is in the output which the operation creates. If an operation outputs another stream, to which you could apply a further operation, we call it an intermediate operation. However, if the operation outputs a concrete type or produces a side effect, it is a terminal type. A subsequent stream operation cannot follow a terminal operation, obviously, as a stream is not returned by the terminal operation!

### Intermediate operations
An intermediate operation is always lazily executed. That is to say they are not run until the point a terminal operation is reached. For example, if you run the following code, nothing will be print out on the screen.

```java

list.stream()
      .filter(x -> {
          System.out.println("filtering element: " + x);
          return true;        
      })
```
Here are some most useful intermediate operations in java stream:

* filter: the filter operation returns a stream of elements that satisfy the predicate passed in as a parameter to the operation.

```java
List<Integer> list = new ArrayList<Integer>();
list.add(4);
list.add(40);
list.add(132);
list.stream()
        .filter(x -> x>10)
        .forEach(System.out::println);
```

* map: the map operation returns a stream of elements after they have been processed by the function passed in as a parameter.

```java
list.stream()
        .filter(x -> x>10)
        .map(y -> y*2)
        .forEach(System.out::println);
```

* distinct: the distinct operation is a special case of the filter operation. Distinct returns a stream of elements such that each element is unique in the stream, based on the equals method of the elements.

```java
list.stream()
        .filter(x -> x>10)
        .map(y -> y*2)
        .distinct()
        .forEach(System.out::println);
```

### Terminate operations
A terminal operation is always eagerly executed. This operation will kick off the execution of all previous lazy operations present in the stream.

* reduce: The reduction operation combines all elements of the stream into a single result.

* collect: Collect is an extremely useful terminal operation to transform the elements of the stream into a different kind of result, e.g. a `List`, `Set` or `Map`. Collect accepts a Collector which consists of four different operations: a supplier, an accumulator, a combiner and a finisher.

```java
List<Person> filtered =
    persons
        .stream()
        .filter(p -> p.name.startsWith("P"))
        .collect(Collectors.toList());

        System.out.println(filtered);    
        // [Peter, Pamela]

Map<Integer, List<Person>> personsByAge = persons
        .stream()
        .collect(Collectors.groupingBy(p -> p.age));

        personsByAge
            .forEach((age, p) -> System.out.format("age %s: %s\n", age, p));

        // age 18: [Max]
        // age 23: [Peter, Pamela]
        // age 12: [David]        
```

* forEach:

### Parallel Streams
You can parallelise the work you do in a stream in a couple of ways. Firstly you can get a parallel stream directly from your source by calling the `parallelStream()`. Alternatively, you can call an intermediate operation `parallel()` on an existing stream which spawns off threads and executes further operations in parallel.

One important thing to note is that parallel streams achieve parallelism through threads using the existing common ForkJoinPool. Using parallel streams can cause concurrency issues depending on what you’re doing in your stream as well.

### Reference
* [Java 8 streams cheat sheet](https://zeroturnaround.com/rebellabs/java-8-streams-cheat-sheet/)
* [Java 8 streams tutorial](http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/)
