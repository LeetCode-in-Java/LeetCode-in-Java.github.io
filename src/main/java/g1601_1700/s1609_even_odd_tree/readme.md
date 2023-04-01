[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1609\. Even Odd Tree

Medium

A binary tree is named **Even-Odd** if it meets the following conditions:

*   The root of the binary tree is at level index `0`, its children are at level index `1`, their children are at level index `2`, etc.
*   For every **even-indexed** level, all nodes at the level have **odd** integer values in **strictly increasing** order (from left to right).
*   For every **odd-indexed** level, all nodes at the level have **even** integer values in **strictly decreasing** order (from left to right).

Given the `root` of a binary tree, _return_ `true` _if the binary tree is **Even-Odd**, otherwise return_ `false`_._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/sample_1_1966.png)

**Input:** root = [1,10,4,3,null,7,9,12,8,6,null,null,2]

**Output:** true

**Explanation:** The node values on each level are: 

Level 0: [1] 

Level 1: [10,4] 

Level 2: [3,7,9]

Level 3: [12,8,6,2] 

Since levels 0 and 2 are all odd and increasing and levels 1 and 3 are all even and decreasing, the tree is Even-Odd.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/15/sample_2_1966.png)

**Input:** root = [5,4,2,3,3,7]

**Output:** false

**Explanation:** The node values on each level are:

Level 0: [5]

Level 1: [4,2] 

Level 2: [3,3,7] 

Node values in level 2 must be in strictly increasing order, so the tree is not Even-Odd.

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/09/22/sample_1_333_1966.png)

**Input:** root = [5,9,1,3,5,7]

**Output:** false

**Explanation:** Node values in the level 1 should be even integers.

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>6</sup></code>

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private final List<Integer> comp = new ArrayList<>();

    public boolean isEvenOddTree(TreeNode root) {
        return find(root, 0);
    }

    private boolean find(TreeNode root, int height) {
        if (root == null) {
            return true;
        }
        if ((height % 2 == 0 && root.val % 2 == 0) || (height % 2 == 1 && root.val % 2 == 1)) {
            return false;
        }
        if (comp.size() == height) {
            comp.add(root.val);
        } else {
            if (height % 2 == 0) {
                if (comp.get(height) >= root.val) {
                    return false;
                } else {
                    comp.set(height, root.val);
                }
            } else {
                if (comp.get(height) <= root.val) {
                    return false;
                } else {
                    comp.set(height, root.val);
                }
            }
        }
        return find(root.left, height + 1) && find(root.right, height + 1);
    }
}
```