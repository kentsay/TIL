# Read File in Java 8

### Java 8 Stream feature 


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
