[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1104\. Path In Zigzag Labelled Binary Tree

Medium

In an infinite binary tree where every node has two children, the nodes are labelled in row order.

In the odd numbered rows (ie., the first, third, fifth,...), the labelling is left to right, while in the even numbered rows (second, fourth, sixth,...), the labelling is right to left.

![](https://assets.leetcode.com/uploads/2019/06/24/tree.png)

Given the `label` of a node in this tree, return the labels in the path from the root of the tree to the node with that `label`.

**Example 1:**

**Input:** label = 14

**Output:** [1,3,4,14]

**Example 2:**

**Input:** label = 26

**Output:** [1,2,6,10,26]

**Constraints:**

*   `1 <= label <= 10^6`

## Solution

```java
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        List<Integer> answer = new LinkedList<>();
        while (label != 0) {
            answer.add(0, label);
            int logNode = (int) (Math.log(label) / Math.log(2));
            int levelStart = (int) (Math.pow(2, logNode));
            int diff = label - levelStart;
            int d2 = diff / 2;
            int prevEnd = levelStart - 1;
            int prevLabel = prevEnd - d2;
            label = prevLabel;
        }
        return answer;
    }
}
```