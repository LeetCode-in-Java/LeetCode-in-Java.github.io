[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1707\. Maximum XOR With an Element From Array

Hard

You are given an array `nums` consisting of non-negative integers. You are also given a `queries` array, where <code>queries[i] = [x<sub>i</sub>, m<sub>i</sub>]</code>.

The answer to the <code>i<sup>th</sup></code> query is the maximum bitwise `XOR` value of <code>x<sub>i</sub></code> and any element of `nums` that does not exceed <code>m<sub>i</sub></code>. In other words, the answer is <code>max(nums[j] XOR x<sub>i</sub>)</code> for all `j` such that <code>nums[j] <= m<sub>i</sub></code>. If all elements in `nums` are larger than <code>m<sub>i</sub></code>, then the answer is `-1`.

Return _an integer array_ `answer` _where_ `answer.length == queries.length` _and_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query._

**Example 1:**

**Input:** nums = [0,1,2,3,4], queries = \[\[3,1],[1,3],[5,6]]

**Output:** [3,3,7]

**Explanation:** 

1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3. 

2) 1 XOR 2 = 3. 

3) 5 XOR 2 = 7.

**Example 2:**

**Input:** nums = [5,2,4,6,6,3], queries = \[\[12,4],[8,1],[6,3]]

**Output:** [15,-1,5]

**Constraints:**

*   <code>1 <= nums.length, queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= nums[j], x<sub>i</sub>, m<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {
    static class QueryComparator implements Comparator<int[]> {
        public int compare(int[] a, int[] b) {
            return Integer.compare(a[1], b[1]);
        }
    }

    static class Node {
        Node zero;
        Node one;

        public Node() {
            this.zero = null;
            this.one = null;
        }
    }

    public int[] maximizeXor(int[] nums, int[][] queries) {
        Arrays.sort(nums);
        int len = queries.length;
        int[][] queryWithIndex = new int[len][3];
        for (int i = 0; i < len; i++) {
            queryWithIndex[i][0] = queries[i][0];
            queryWithIndex[i][1] = queries[i][1];
            queryWithIndex[i][2] = i;
        }
        Arrays.sort(queryWithIndex, new QueryComparator());
        int numId = 0;
        int[] ans = new int[len];
        Node root = new Node();
        for (int i = 0; i < len; i++) {
            while (numId < nums.length && nums[numId] <= queryWithIndex[i][1]) {
                addNumToTree(nums[numId], root);
                numId++;
            }
            ans[queryWithIndex[i][2]] = maxXOR(queryWithIndex[i][0], root);
        }
        return ans;
    }

    private void addNumToTree(int num, Node node) {
        for (int i = 31; i >= 0; i--) {
            int digit = (num >> i) & 1;
            if (digit == 1) {
                if (node.one == null) {
                    node.one = new Node();
                }
                node = node.one;
            } else {
                if (node.zero == null) {
                    node.zero = new Node();
                }
                node = node.zero;
            }
        }
    }

    private int maxXOR(int num, Node node) {
        if (node.one == null && node.zero == null) {
            return -1;
        }
        int ans = 0;
        for (int i = 31; i >= 0 && node != null; i--) {
            int digit = (num >> i) & 1;
            if (digit == 1) {
                if (node.zero != null) {
                    ans += (1 << i);
                    node = node.zero;
                } else {
                    node = node.one;
                }
            } else {
                if (node.one != null) {
                    ans += (1 << i);
                    node = node.one;
                } else {
                    node = node.zero;
                }
            }
        }
        return ans;
    }
}
```