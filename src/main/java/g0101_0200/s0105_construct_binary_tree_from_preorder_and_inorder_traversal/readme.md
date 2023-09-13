[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 105\. Construct Binary Tree from Preorder and Inorder Traversal

Medium

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]

**Output:** [3,9,20,null,null,15,7] 

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]

**Output:** [-1] 

**Constraints:**

*   `1 <= preorder.length <= 3000`
*   `inorder.length == preorder.length`
*   `-3000 <= preorder[i], inorder[i] <= 3000`
*   `preorder` and `inorder` consist of **unique** values.
*   Each value of `inorder` also appears in `preorder`.
*   `preorder` is **guaranteed** to be the preorder traversal of the tree.
*   `inorder` is **guaranteed** to be the inorder traversal of the tree.

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.HashMap;
import java.util.Map;

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
    private int j;
    private Map<Integer, Integer> map = new HashMap<>();

    public int get(int key) {
        return map.get(key);
    }

    private TreeNode answer(int[] preorder, int[] inorder, int start, int end) {
        if (start > end || j > preorder.length) {
            return null;
        }
        int value = preorder[j++];
        int index = get(value);
        TreeNode node = new TreeNode(value);
        node.left = answer(preorder, inorder, start, index - 1);
        node.right = answer(preorder, inorder, index + 1, end);
        return node;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        j = 0;
        for (int i = 0; i < preorder.length; i++) {
            map.put(inorder[i], i);
        }
        return answer(preorder, inorder, 0, preorder.length - 1);
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program can be analyzed as follows:

- The `buildTree` function initializes a map (`map`) and populates it with values from the `inorder` array. The population process takes O(N) time, where N is the number of nodes in the binary tree.

- The `answer` function is a recursive function that constructs the binary tree. It uses the `preorder` array to determine the root of the current subtree. For each subtree, it finds the index of the root value in the `inorder` array using the `map`. This operation takes O(1) time.

- In each recursive call, the program divides the problem into two subproblems: constructing the left subtree and constructing the right subtree. The division and recursive calls are performed for each node in the tree.

- Since each node is processed once in the recursive calls, and there are N nodes in the binary tree, the overall time complexity of the program is O(N).

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for data structures and the call stack. Here's the analysis of space complexity:

- The `map` data structure is used to store mappings between values and their indices in the `inorder` array. The space used by this map is proportional to the number of nodes in the binary tree, which is O(N).

- The recursive function `answer` utilizes the call stack to keep track of the recursive calls. In the worst case, when the binary tree is completely unbalanced (e.g., a skewed tree), the maximum depth of the call stack can be equal to the height of the tree, which is N in the worst case.

- Therefore, the space complexity due to the call stack is O(N) in the worst case.

- Additionally, there are a few integer variables and temporary variables used in the program. However, the space used by these variables is constant and does not depend on the input size.

- Overall, the space complexity of the program is O(N) due to the map and the call stack.

In summary, the time complexity of the program is O(N), and the space complexity is O(N) in the worst case. The space complexity is primarily determined by the map and the maximum depth of the call stack during the recursive construction of the binary tree.
