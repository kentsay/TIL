# Lambda Expression

### syntax
The main syntax of a lambda expression is “parameters -> body”. The compiler can usually use the context of the lambda expression to determine the functional interface2 being used and the types of the parameters. There are four important rules to the syntax:

	* Declaring the types of the parameters is optional.
	* Using parentheses around the parameter is optional if you have only one parameter.
	* Using curly braces is optional (unless you need multiple statements).
	* The “return” keyword is optional if you have a single expression that returns a value.

```java
1 () -> System.out.println(this)
2 (String str) -> System.out.println(str)
3 str -> System.out.println(str)
4 (String s1, String s2) -> { return s2.length() - s1.length(); }
5 (s1, s2) -> s2.length() - s1.length()

```	

### examples
Printing out a list of Strings

```java
// Java 7
for (String s : list) {
   System.out.println(s);
}
//Java 8
list.forEach(System.out::println);
```


