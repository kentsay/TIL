### Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

Example:

```
    3
   / \
  9  20
    /  \
   15   7

```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

#### Solution

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {

    /**
      We only need to consider node.left. The rest node just all recursive check.
     */
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) return 0;
        int sum = 0;
        if (root.left != null) {                      
          if (root.left.left == null && root.left.right == null) {
            sum += root.left.val;
          } else {
            sum += sumOfLeftLeaves(root.left);  
          }                                        
        }
        if (root.right != null) {
          sum += sumOfLeftLeaves(root.right);
        }
        return sum;       
    }
}

```
