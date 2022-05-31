## 1530\. Number of Good Leaf Nodes Pairs

Medium

You are given the `root` of a binary tree and an integer `distance`. A pair of two different **leaf** nodes of a binary tree is said to be good if the length of **the shortest path** between them is less than or equal to `distance`.

Return _the number of good leaf node pairs_ in the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/09/e1.jpg)

**Input:** root = [1,2,3,null,4], distance = 3

**Output:** 1

**Explanation:** The leaf nodes of the tree are 3 and 4 and the length of the shortest path between them is 3. This is the only good pair.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/07/09/e2.jpg)

**Input:** root = [1,2,3,4,5,6,7], distance = 3

**Output:** 2

**Explanation:** The good pairs are [4,5] and [6,7] with shortest path = 2. The pair [4,6] is not good because the length of ther shortest path between them is 4.

**Example 3:**

**Input:** root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3

**Output:** 1

**Explanation:** The only good pair is [2,5].

**Constraints:**

*   The number of nodes in the `tree` is in the range <code>[1, 2<sup>10</sup>].</code>
*   `1 <= Node.val <= 100`
*   `1 <= distance <= 10`

## Solution

```java
import com_github_leetcode.TreeNode;

public class Solution {
    public int countPairs(TreeNode root, int distance) {
        if (distance < 2) {
            return 0;
        }
        return pairsAndLeaves(root, distance)[0];
    }

    private int[] pairsAndLeaves(TreeNode node, int distance) {
        int[] r = new int[distance];
        if (node == null) {
            return r;
        }
        if (node.left == null && node.right == null) {
            r[1] = 1;
            return r;
        }
        int[] rl = pairsAndLeaves(node.left, distance);
        int[] rr = pairsAndLeaves(node.right, distance);
        for (int i = 2; i < distance; i++) {
            r[i] = rl[i - 1] + rr[i - 1];
        }
        int pairs = rl[0] + rr[0];
        for (int dist = 2; dist <= distance; dist++) {
            for (int leftToNodeDist = 1; leftToNodeDist < dist; leftToNodeDist++) {
                pairs += rl[leftToNodeDist] * rr[dist - leftToNodeDist];
            }
        }
        r[0] = pairs;
        return r;
    }
}
```