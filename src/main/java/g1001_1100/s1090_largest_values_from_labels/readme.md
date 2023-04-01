[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1090\. Largest Values From Labels

Medium

There is a set of `n` items. You are given two integer arrays `values` and `labels` where the value and the label of the <code>i<sup>th</sup></code> element are `values[i]` and `labels[i]` respectively. You are also given two integers `numWanted` and `useLimit`.

Choose a subset `s` of the `n` elements such that:

*   The size of the subset `s` is **less than or equal to** `numWanted`.
*   There are **at most** `useLimit` items with the same label in `s`.

The **score** of a subset is the sum of the values in the subset.

Return _the maximum **score** of a subset_ `s`.

**Example 1:**

**Input:** values = [5,4,3,2,1], labels = [1,1,2,2,3], numWanted = 3, useLimit = 1

**Output:** 9

**Explanation:** The subset chosen is the first, third, and fifth items.

**Example 2:**

**Input:** values = [5,4,3,2,1], labels = [1,3,3,3,2], numWanted = 3, useLimit = 2

**Output:** 12

**Explanation:** The subset chosen is the first, second, and third items.

**Example 3:**

**Input:** values = [9,8,8,7,6], labels = [0,0,0,1,1], numWanted = 3, useLimit = 1

**Output:** 16

**Explanation:** The subset chosen is the first and fourth items.

**Constraints:**

*   `n == values.length == labels.length`
*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>0 <= values[i], labels[i] <= 2 * 10<sup>4</sup></code>
*   `1 <= numWanted, useLimit <= n`

## Solution

```java
import java.util.HashMap;
import java.util.PriorityQueue;

public class Solution {
    private static class Node {
        int val;
        int label;

        Node(int val, int label) {
            this.val = val;
            this.label = label;
        }
    }

    public int largestValsFromLabels(int[] values, int[] labels, int numWanted, int useLimit) {
        PriorityQueue<Node> maxHeap =
                new PriorityQueue<>((a, b) -> b.val != a.val ? b.val - a.val : a.label - b.label);
        int n = values.length;
        for (int i = 0; i < n; i++) {
            maxHeap.offer(new Node(values[i], labels[i]));
        }
        int ans = 0;
        HashMap<Integer, Integer> labelAddedCount = new HashMap<>();
        while (!maxHeap.isEmpty() && numWanted > 0) {
            Node cur = maxHeap.poll();
            if (labelAddedCount.containsKey(cur.label)
                    && labelAddedCount.get(cur.label) >= useLimit) {
                continue;
            }
            if (cur.val > 0) {
                ans += cur.val;
                labelAddedCount.put(cur.label, labelAddedCount.getOrDefault(cur.label, 0) + 1);
                numWanted--;
            }
        }
        return ans;
    }
}
```