# [LeetCode 7] Reverse Integer 

### Question you should ask
* What if the number after reverse starts with 0?
* What should we return if overflow happen?

Base on the question you aksed, you should realize that convert the Integer into a String and then reverse it is probably not a good soltuion. It needs extra space and it's not efficient.

### Solution
From math perspective, we try to extract the last digit at a time, then try to move it forward until it's the first digit. Also in every step, check if the integer went overflow.  

```java
public int solution(int x) {
  int revert = 0;
  while(x != 0) {
    if (Math.abs(revert) > Integer.MAX_VALUE) {
      return 0;  
    }
    revert = revert * 10 + x % 10;
    x /= 10;
  }
  return revert;
}

```
### Reference
* [shuati blog](http://www.shuatiblog.com/blog/2014/04/28/reverse-integer/)




