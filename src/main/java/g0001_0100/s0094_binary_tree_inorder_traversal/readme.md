[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 94\. Binary Tree Inorder Traversal

Easy

Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

**Input:** root = [1,null,2,3]

**Output:** [1,3,2] 

**Example 2:**

**Input:** root = []

**Output:** [] 

**Example 3:**

**Input:** root = [1]

**Output:** [1] 

**Example 4:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)

**Input:** root = [1,2]

**Output:** [2,1] 

**Example 5:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg)

**Input:** root = [1,null,2]

**Output:** [1,2] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 100]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

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
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return Collections.emptyList();
        }
        List<Integer> answer = new ArrayList<>();
        inorderTraversal(root, answer);
        return answer;
    }

    public void inorderTraversal(TreeNode root, List<Integer> answer) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            inorderTraversal(root.left, answer);
        }
        answer.add(root.val);
        if (root.right != null) {
            inorderTraversal(root.right, answer);
        }
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program depends on how many nodes are present in the binary tree.

- In the worst case, if the binary tree is completely unbalanced (i.e., a skewed tree), the `inorderTraversal` function may have to traverse through all the nodes one by one, resulting in a time complexity of O(n), where 'n' is the number of nodes in the tree.

- In the best case, if the binary tree is perfectly balanced (i.e., a full binary tree), the `inorderTraversal` function will still visit all 'n' nodes, resulting in a time complexity of O(n).

- The time complexity is primarily determined by the number of nodes in the tree, so it is O(n) in both the worst and best cases.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space required for the call stack due to recursive function calls and the space used for the output list.

- Recursive Call Stack: The `inorderTraversal` function is implemented recursively. In the worst case, where the tree is completely unbalanced, the maximum depth of the call stack will be 'n' (the number of nodes in the tree), as the function makes recursive calls for each node in the tree. Therefore, the space complexity due to the call stack is O(n).

- Output List: The `answer` list stores the elements of the binary tree in inorder traversal order. It will contain 'n' elements (equal to the number of nodes in the tree), so the space complexity for the output list is O(n).

- Overall, the space complexity is dominated by the call stack and the output list, so it is O(n) in both the worst and best cases.

In summary, the time complexity of the program is O(n), and the space complexity is O(n), where 'n' is the number of nodes in the binary tree.
