### Binary Search tree

Implement BST Operations:
* insert
* find
* find level of node
* lowest common ancestor (LCA)
* distance between 2 nodes.

```java
public class BSTPractice {
    public static void main(String[] args) {
        BSTPractice test= new BSTPractice();
        test.solution();
    }

    public void solution() {
        int[] values = {5, 3, 2, 4, 1};
        int nodeNumbers = 5;

        TreeNode root = new TreeNode(values[0]);
        for (int i = 1; i < nodeNumbers; i++) {
            root.insert(values[i]);
        }

        System.out.println(root.getNodeLevel(root, 2, 1));
        System.out.println(root.getNodeLevel(root, 1, 1));
        System.out.println(root.findNode(3));
        System.out.println(root.findNode(8));
        System.out.println(root.LCA(new TreeNode(1), new TreeNode(4)).value);
        System.out.println(root.nodeDistance(new TreeNode(1), new TreeNode(6)));
    }
}

class TreeNode {
    public int value;
    public TreeNode right;
    public TreeNode left;
    public TreeNode parent;
    private int size;

    public TreeNode(int data) {
        this.value = data;
        size=1;
    }

    public void setLeftNode(TreeNode leftNode) {
        this.left = leftNode;
        if (leftNode != null)
            leftNode.parent = this;
    }

    public void setRightNode(TreeNode rightNode) {
        this.right = rightNode;
        if (rightNode != null)
            rightNode.parent = this;
    }

    public void insert(int data) {
        if (data >= value) {
            if (right == null) {
                setRightNode(new TreeNode(data));
            } else {
                right.insert(data);
            }
        } else {
            if (left == null)
                setLeftNode(new TreeNode(data));
            else {
                left.insert(data);
            }
        }
    }

    public boolean findNode(int data) {
        if (value == data) return true;
        else if (value > data) {
            return left != null ? left.findNode(data): false;
        } else if (value < data) {
            return right != null ? right.findNode(data): false;
        }
        return false;
    }

    public int getNodeLevel(TreeNode node, int key, int level) {
        if (node == null) return 0;
        if (node.value == key) return level;

        int downlevel;
        if (key > node.value)
            downlevel = getNodeLevel(node.right, key, level+1);
        else
            downlevel = getNodeLevel(node.left, key, level+1);

        return downlevel;
    }

    public TreeNode LCA(TreeNode a, TreeNode b) {
        int large = Math.max(a.value, b.value);
        int small = Math.min(a.value, b.value);

        if (value > small && value < large) {
            return this;
        } else if (value > small && value > large) {
            return left.LCA(a, b);
        } else if (value < small && value < large) {
            return right.LCA(a, b);
        }

        return this;
    }

    public int nodeDistance(TreeNode node1, TreeNode node2) {
        if (findNode(node1.value) && findNode(node2.value)) {
            int distance = 0;
            //make sure both node exists in tree
            int node1_level = getNodeLevel(this, node1.value, 1);
            int node2_level = getNodeLevel(this, node2.value, 1);
            int lca_level = getNodeLevel(this, LCA(node1, node2).value, 1);

            distance = (node1_level - lca_level) + (node2_level-lca_level);
            return distance;
        } else {
            return -1;
        }
    }

}
```
