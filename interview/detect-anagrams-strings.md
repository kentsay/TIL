# Write a function to detect if two strings are anagrams (for example, SAVE and VASE)

This is my go-to first interview question. It helps me gauge a candidate’s ability to understand a problem and write an algorithm to solve it.

If someone has not solved the problem before, I expect to see some code with loops and if/then’s. Maybe some HashMaps. The things I look for are ability to break down the problem to see what you need to check, what the edge cases are, and whether your code meets those criteria.

The naive solution is often to loop through the letters of the first string and see if they’re all in the second string.
The next thing to look for is, don’t you need to do that in reverse too (check string 1 for string 2’s letters)?
The next thing to look for is, what about strings with duplicate letters, like VASES?
If you can realize that these are all required and create a functional, non-ridiculous solution, I am happy.

Of course, you can solve it trivially by sorting both strings and comparing them. If someone catches this right away, usually they have seen the problem before. But that’s a good sign that someone cares enough to do prep work. Then we can tackle a harder problem.

```java
public static boolean isAcronym(String s1, String s2) {

  if (s1.length() != s2.length()) return false;

  HashMap<Character, Integer> charCounts = new HashMap<>();

  // Calculate chracter counts

  for (int i = 0; i < s1.length(); i++) {
    if (charCounts.containsKey(s1.charAt(i))) {
      charCounts.put(s1.charAt(i), charCounts.get(s1.charAt(i)) + 1);
    } else {
      charCounts.put(s1.charAt(i), 1);
    }
  }

  // Compare counts with characters in s2

  for (int i = 0; i < s2.length(); i++) {
    if (charCounts.containsKey(s2.charAt(i))) {
      charCounts.put(s2.charAt(i), charCounts.get(s2.charAt(i)) - 1);
    } else {
      return false;
    }
  }

  // Check all letters matched
  for (int count : charCounts.values()) {
    if (count != 0) return false;
  }

  return true;
}
```
Here is one way to implement a better solution, comparing sorted strings:

```java
public static boolean isAcronymMoreBetter(String s1, String s2) {
  char[] s1Chars = s1.toCharArray();
  char[] s2Chars = s2.toCharArray();
  Arrays.sort(s1Chars);
  Arrays.sort(s2Chars);
  return Arrays.equals(s1Chars, s2Chars);
}

```
