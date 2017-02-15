### Sort Characters By Frequency

I found this problem was interesting because of one condition: the same characters must be together (see example 2). In the beginning I was trying to use 2 stack to solve this, but it turns out stack just can cut it. Two interesting solution was found on the internet and it's really easy. A very good example to understand how to use Java Arrays.sort.

https://leetcode.com/problems/sort-characters-by-frequency/
Given a string, sort it in decreasing order based on the frequency of characters.
```
Example 1:
Input:
"tree"

Output:
"eert"
```

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.


```
Example 2:
Input:
"cccaaa"

Output:
"cccaaa"
```

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.

```
Example 3:
Input:
"Aabb"

Output:
"bbAa"
```

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.

#### Solution 1
Count the frequency of each character, and then sort the characters by frequency and out put the characters. We can use a two dimension array for the easy counting and sorting.

```java
public String frequencySort(String s) {
  int[][] count = new int[256][2];

  for(char c: s.toCharArray()) {
    count[c][0] = c;
    count[c][1]++;
  }

  Arrays.sort(count, new Comparator<int[]>() {
    public int compare(int[] a, int[] b) {
      return a[1] == b[1] ? 0 : (a[1] < b[1] ? 1 : -1);
    }
  });


  StringBuilder sb = new StringBuilder();
  for(int i=0; i<256; i++) {
    if (count[i][1] > 0) {
      for(int j=0; j<count[i][1]; j++) {
        sb.append((char)count[i][0]);
      }
    }
  }

  return sb.toString();
}
```

#### Solution 2
Use a array to record the frequency of characters. Then use a HashMap of structure `Integer` and `List<Character>` to store frequency map to the characters. Last, print out the sorted string based on their frequency.

```java
public String frequencySort(String s) {
    char[] arr = s.toCharArray();

    // bucket sort
    int[] count = new int[256];
    for(char c : arr) count[c]++;

    // count values and their corresponding letters
    Map<Integer, List<Character>> map = new HashMap<>();//\\
    for(int i = 0; i < 256; i++){
        if(count[i] == 0) continue;
        int cnt = count[i];
        if(!map.containsKey(cnt)){
            map.put(cnt, new ArrayList<Character>());
        }
        map.get(cnt).add((char)i);
    }

    // loop through possible count values
    StringBuilder sb = new StringBuilder();
    for(int cnt = arr.length; cnt > 0; cnt--){ //\\
        if(!map.containsKey(cnt)) continue;
        List<Character> list = map.get(cnt);
        for(Character c: list){
            for(int i = 0; i < cnt; i++){
                sb.append(c);
            }
        }
    }
    return sb.toString();
}
```
