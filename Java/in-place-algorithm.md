An `in-place algorithm` operates directly on its input and changes it, instead of creating and returning a new object. This is sometimes called destructive, since the original input is "destroyed" when it's edited to create the new output.

Careful: "In-place" does not mean "without creating any additional variables"! Rather, it means "without creating a new copy of the input." In general, an in-place function will only create additional variables that are O(1) space.

Here are two functions that do the same operation, except one is in-place and the other is out-of-place:

```java
public int[] squareArrayInPlace(int[] intArray) {

    for (int i = 0; i < intArray.length; i++) {
        intArray[i] *= intArray[i];
    }

    // NOTE: we don't /need/ to return anything
    // this is just a convenience
    return intArray;
}

public int[] squareArrayOutOfPlace(int[] intArray) {

    // we allocate a new array with the length of the input array
    int[] squaredArray = new int[intArray.length];

    for (int i = 0; i < intArray.length; i++) {
        squaredArray[i] = (int) Math.pow(intArray[i], 2);
    }

    return squaredArray;
}

```

Working in-place is a good way to save space. An in-place algorithm will generally have O(1) space cost.

But be careful: an in-place algorithm can cause side effects. Your input is "destroyed" or "altered," which can affect code outside of your function. For example:

```java
int[] originalArray = new int[]{2, 3, 4, 5};
int[] squaredArray  = squareArrayInPlace(originalArray);

System.out.println("squared: " + Arrays.toString(squaredArray));
// prints: squared: [4, 9, 16, 25]

System.out.println("original array: " + Arrays.toString(originalArray));
// prints: original array: [4, 9, 16, 25], confusingly!

// and if squareArrayInPlace() didn't return anything,
// which it could reasonably do, squaredArray would be null!

```

Generally, out-of-place algorithms are considered safer because they avoid side effects. You should only use an in-place algorithm if you're very space constrained or you're positive you don't need the original input anymore, even for debugging.