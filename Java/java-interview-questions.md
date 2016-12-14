# Java Interview Questions
I found many of these interesting questions on the internet while preparing my job interview, and feel making a collection of these questions will be fun. If you think you have a well understanding of Java, which I believe many devleopers, including me, "feel" I do, try to answer these questions and test yourself. If you didn't answer well, it's totally fine. Because you learn something new today :)

#### Why isn’t String‘s .length() accurate?
It isn’t accurate because it will only account for the number of characters within the String. In other words, it will fail to account for code points outside of what is called the BMP (Basic Multilingual Plane), that is, code points with a value of U+10000 or greater.

The reason is historical: when Java was first defined, one of its goal was to treat all text as Unicode; but at this time, Unicode did not define code points outside of the BMP. By the time Unicode defined such code points, it was too late for char to be changed.

This means that code points outside the BMP are represented with two chars in Java, in what is called a surrogate pair. Technically, a char in Java is a UTF-16 code unit.

The correct way to count the real numbers of characters within a String, i.e. the number of code points, is either:

```java
someString.codePointCount(0, someString.length())
```

or, with Java 8:

```java
someString.codePoints().count()
```

#### What is the problem with this code: final byte[] bytes = someString.getBytes();

There are, in fact, two problems:

the code relies on the default Charset of the JVM;
it supposes that this default Charset can handle all characters.
While the second problem is rarely a concern, the first certainly is a concern.

For instance, in most Windows installations, the default charset is CP1252; but on Linux installations, the default charset will be UTF-8.

As such, such a simple string as “é” will give a different result for this operation depending on whether this code is run on Windows or Linux.

The solution is to always specify a Charset, as in, for instance:

```java
final byte[] bytes = someString.getBytes(StandardCharsets.UTF_8);
```

#### Give real world examples of when to use an ArrayList and when to use LinkedList
ArrayList is preferred when there are more get(int), or when search operations need to be performed as every search operation runtime is O(1).

If an application requires more insert(int) and delete(int) operations, then LinkedList is preferred, as LinkedList does not need to maintain back and forth to preserve continued indices as arraylist does. Overall this question tests the proper usage of collections.

#### What is the difference between an Iterator and a ListIterator ? 
This question tests the proper usage of collection iterators. We can only use ListIterator to traverse Lists, and we cannot traverse a Set using ListIterator.

What’s more, we can only traverse in a forward direction using Iterators. Using ListIterator, we can traverse a List in both the directions (forward and backward).

We cannot obtain indexes while using Iterator. We can obtain indexes at any point of time while traversing a list using ListIterator. The methods nextIndex() and previousIndex() are used for this purpose.


#### What is the advantage of generic collection?
They enable stronger type checks at compile time.

A Java compiler applies strong type checking to generic code, and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be difficult to find.
