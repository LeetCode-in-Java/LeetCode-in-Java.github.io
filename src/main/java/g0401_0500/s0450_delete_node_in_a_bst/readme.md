[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 450\. Delete Node in a BST

Medium

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

1.  Search for a node to remove.
2.  If the node is found, delete the node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

**Input:** root = [5,3,6,2,4,null,7], key = 3

**Output:** [5,4,6,2,null,null,7]

**Explanation:**

    Given key to delete is 3. So we find the node with value 3 and delete it.
    One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
    Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.
![](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

**Example 2:**

**Input:** root = [5,3,6,2,4,null,7], key = 0

**Output:** [5,3,6,2,4,null,7]

**Explanation:** The tree does not contain a node with value = 0.

**Example 3:**

**Input:** root = [], key = 0

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>
*   Each node has a **unique** value.
*   `root` is a valid binary search tree.
*   <code>-10<sup>5</sup> <= key <= 10<sup>5</sup></code>

**Follow up:** Could you solve it with time complexity `O(height of tree)`?

## Solution

```java
import com_github_leetcode.TreeNode;

/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    /*
     * Steps:
     * 1. Recursively find the node that has the same value as the key, while setting the left/right nodes equal
     * to the returned subtree
     * 2. Once the node is found, have to handle the below 4 cases
     * a. node doesn't have left or right - return null
     * b. node only has left subtree- return the left subtree
     * c. node only has right subtree- return the right subtree
     * d. node has both left and right - find the minimum value in the right subtree, set that value
     * to the currently found node, then recursively delete the minimum value in the right subtree
     */
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return root;
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
        } else {
            if (root.left == null) {
                return root.right;
            } else if (root.right == null) {
                return root.left;
            }
            TreeNode minNode = getMin(root.right);
            root.val = minNode.val;
            root.right = deleteNode(root.right, root.val);
        }
        return root;
    }

    private TreeNode getMin(TreeNode node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }
}
```