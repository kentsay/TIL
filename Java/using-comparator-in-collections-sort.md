# Using Collections.sort and Comparator in Java

Sorting a collection in Java is easy, just use the Collections.sort(Collection) to sort your values. The following code shows an example for this.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
 
public class Simple {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add(5);
        list.add(4);
        list.add(3);
        list.add(7);
        list.add(2);
        list.add(1);
        Collections.sort(list);
        for (Integer integer : list) {
            System.out.println(integer);
        }
    }
} 

```

This is possible because Integer implements the Comparable interface. This interface defines the method compare which performs pairwise comparison of the elements and returns -1 if the element is smaller then the compared element, 0 if it is equal and 1 if it is larger.

If what to sort differently you can define your own implementation based on the Comparator interface.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Simple2 {
  public static void main(String[] args) {
    List<Integer> list = new ArrayList<Integer>();
    list.add(5);
    list.add(4);
    list.add(3);
    list.add(7);
    list.add(2);
    list.add(1);
    Collections.sort(list, new Comparator<Integer>() {
    	public int compare(Integer o1, Integer o2) {
    		return (o1>o2 ? -1 : (o1==o2 ? 0 : 1));
    	}
	});
    for (Integer integer : list) {
      System.out.println(integer);
    }
  }
} 
```

This approach is that you then sort any object by any attribute or even a combination of attributes. For example if you have objects of type Person with the attributes called income and dataOfBirth you could define different implementations of Comparator and sort the objects according to your needs.

