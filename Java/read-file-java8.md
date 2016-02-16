# Read File in Java 8

### Java 8 Stream feature 

  * **No storage**. A stream is not a data structure that stores elements; instead, it conveys elements from a source such as a data structure, an array, a generator function, or an I/O channel, through a pipeline of computational operations.
  * **Functional in nature**. An operation on a stream produces a result, but does not modify its source. For example, filtering a Stream obtained from a collection produces a new Stream without the filtered elements, rather than removing elements from the source collection.
  * **Laziness-seeking**. Many stream operations, such as filtering, mapping, or duplicate removal, can be implemented lazily, exposing opportunities for optimization. Stream operations are divided into intermediate (Stream-producing) operations and terminal (value- or side-effect-producing) operations. Intermediate operations are always lazy.
  * **Possibly unbounded**. While collections have a finite size, streams need not. Short-circuiting operations such as limit(n) or findFirst() can allow computations on infinite streams to complete in finite time.
  * **Consumable**. The elements of a stream are only visited once during the life of a stream. Like an Iterator, a new stream must be generated to revisit the same elements of the source.

### Read the file line by line

```java
public static void main(String args[]) {

  String fileName = "c://lines.txt";

  //read file into stream, try-with-resources
  try (Stream<String> stream = Files.lines(Paths.get(fileName))) {
    stream.forEach(System.out::println);
  } catch (IOException e) {
    e.printStackTrace();
  }
}

```

### Read the file into a String

```java
public static void main(String[] args) throws IOException {
  String content = new String(Files.readAllBytes(Paths.get("duke.java")));
}

```
