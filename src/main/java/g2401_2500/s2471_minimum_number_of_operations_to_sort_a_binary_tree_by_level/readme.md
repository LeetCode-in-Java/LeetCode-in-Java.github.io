[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2471\. Minimum Number of Operations to Sort a Binary Tree by Level

Medium

You are given the `root` of a binary tree with **unique values**.

In one operation, you can choose any two nodes **at the same level** and swap their values.

Return _the minimum number of operations needed to make the values at each level sorted in a **strictly increasing order**_.

The **level** of a node is the number of edges along the path between it and the root node_._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174006-2.png)

**Input:** root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]

**Output:** 3

**Explanation:**
- Swap 4 and 3. The 2<sup>nd</sup> level becomes [3,4]. 
- Swap 7 and 5. The 3<sup>rd</sup> level becomes [5,6,8,7]. 
- Swap 8 and 7. The 3<sup>rd</sup> level becomes [5,6,7,8]. 

We used 3 operations so return 3. It can be proven that 3 is the minimum number of operations needed.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174026-3.png)

**Input:** root = [1,3,2,7,6,5,4]

**Output:** 3

**Explanation:** 
- Swap 3 and 2. The 2<sup>nd</sup> level becomes [2,3].
- Swap 7 and 4. The 3<sup>rd</sup> level becomes [4,6,5,7]. 
- Swap 6 and 5. The 3<sup>rd</sup> level becomes [4,5,6,7]. 

We used 3 operations so return 3. It can be proven that 3 is the minimum number of operations needed.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174052-4.png)

**Input:** root = [1,2,3,4,5,6]

**Output:** 0

**Explanation:** Each level is already sorted in increasing order so return 0.

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>
*   All the values of the tree are **unique**.

## Solution

```java
import com_github_leetcode.TreeNode;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int minimumOperations(TreeNode root) {
        ArrayDeque<TreeNode> q = new ArrayDeque<>();
        int count = 0;
        if ((root.left != null && root.right != null) && (root.left.val > root.right.val)) {
            count++;
        }
        if (root.left != null) {
            q.add(root.left);
        }
        if (root.right != null) {
            q.add(root.right);
        }
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> al = new ArrayList<>();
            while (size-- > 0) {
                TreeNode node = q.poll();
                assert node != null;
                if (node.left != null) {
                    al.add(node.left.val);
                    q.add(node.left);
                }
                if (node.right != null) {
                    al.add(node.right.val);
                    q.add(node.right);
                }
            }
            count += helper(al);
        }
        return count;
    }

    private int helper(List<Integer> list) {
        int swaps = 0;
        int[] sorted = new int[list.size()];
        for (int i = 0; i < sorted.length; i++) {
            sorted[i] = list.get(i);
        }
        Arrays.sort(sorted);
        Map<Integer, Integer> ind = new HashMap<>();
        for (int i = 0; i < list.size(); i++) {
            ind.put(list.get(i), i);
        }

        for (int i = 0; i < list.size(); i++) {
            if (list.get(i) != sorted[i]) {
                swaps++;
                ind.put(list.get(i), ind.get(sorted[i]));
                list.set(ind.get(sorted[i]), list.get(i));
            }
        }
        return swaps;
    }
}
```