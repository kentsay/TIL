# Invert Binary Tree


### Problem of invert a binary tree

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

to

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

This problem was inspired by this original tweet by Max Howell:
```
Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.
```

### Recursive Solution

It's acutally quite easy if you understand the problem. A recursive algorithm can be used to solve this.

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
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode newLeft  = invertTree(root.right);
        TreeNode newRight = invertTree(root.left);

        root.left  = newLeft;
        root.right = newRight;

        return root;
    }
}
```

### Iterative Solution

The idea is that we need to swap the left and right child of all nodes in the tree. So we create a queue to store nodes whose left and right child have not been swapped yet. Initially, only the root is in the queue. As long as the queue is not empty, remove the next node from the queue, swap its children, and add the children to the queue. Null nodes are not added to the queue. Eventually, the queue will be empty and all the children swapped, and we return the original root.

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
    public TreeNode invertTree(TreeNode root) {
        Queue<TreeNode> queue = new LinkList<TreeNode>();
        queue.add(root);
        while(!queue.isEmpty()) {
          TreeNode current = queue.poll();
          TreeNode temp = current.left;
          current.left = current.right;
          current.right = temp;
          if (current.left != null) queue.add(current.left);
          if (current.right != null) queue.add(current.right);
        }
        return root;
    }
}
```
