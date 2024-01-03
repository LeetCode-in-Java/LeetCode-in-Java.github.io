[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2931\. Maximum Spending After Buying Items

Hard

You are given a **0-indexed** `m * n` integer matrix `values`, representing the values of `m * n` different items in `m` different shops. Each shop has `n` items where the <code>j<sup>th</sup></code> item in the <code>i<sup>th</sup></code> shop has a value of `values[i][j]`. Additionally, the items in the <code>i<sup>th</sup></code> shop are sorted in non-increasing order of value. That is, `values[i][j] >= values[i][j + 1]` for all `0 <= j < n - 1`.

On each day, you would like to buy a single item from one of the shops. Specifically, On the <code>d<sup>th</sup></code> day you can:

*   Pick any shop `i`.
*   Buy the rightmost available item `j` for the price of `values[i][j] * d`. That is, find the greatest index `j` such that item `j` was never bought before, and buy it for the price of `values[i][j] * d`.

**Note** that all items are pairwise different. For example, if you have bought item `0` from shop `1`, you can still buy item `0` from any other shop.

Return _the **maximum amount of money that can be spent** on buying all_ `m * n` _products_.

**Example 1:**

**Input:** values = \[\[8,5,2],[6,4,1],[9,7,3]]

**Output:** 285

**Explanation:** On the first day, we buy product 2 from shop 1 for a price of values[1][2] \* 1 = 1. 

On the second day, we buy product 2 from shop 0 for a price of values[0][2] \* 2 = 4. 

On the third day, we buy product 2 from shop 2 for a price of values[2][2] \* 3 = 9. 

On the fourth day, we buy product 1 from shop 1 for a price of values[1][1] \* 4 = 16.

On the fifth day, we buy product 1 from shop 0 for a price of values[0][1] \* 5 = 25. 

On the sixth day, we buy product 0 from shop 1 for a price of values[1][0] \* 6 = 36.

On the seventh day, we buy product 1 from shop 2 for a price of values[2][1] \* 7 = 49. 

On the eighth day, we buy product 0 from shop 0 for a price of values[0][0] \* 8 = 64. 

On the ninth day, we buy product 0 from shop 2 for a price of values[2][0] \* 9 = 81. 

Hence, our total spending is equal to 285. 

It can be shown that 285 is the maximum amount of money that can be spent buying all m \* n products.

**Example 2:**

**Input:** values = \[\[10,8,6,4,2],[9,7,5,3,2]]

**Output:** 386

**Explanation:** On the first day, we buy product 4 from shop 0 for a price of values[0][4] \* 1 = 2. 

On the second day, we buy product 4 from shop 1 for a price of values[1][4] \* 2 = 4. 

On the third day, we buy product 3 from shop 1 for a price of values[1][3] \* 3 = 9. 

On the fourth day, we buy product 3 from shop 0 for a price of values[0][3] \* 4 = 16. 

On the fifth day, we buy product 2 from shop 1 for a price of values[1][2] \* 5 = 25. 

On the sixth day, we buy product 2 from shop 0 for a price of values[0][2] \* 6 = 36. 

On the seventh day, we buy product 1 from shop 1 for a price of values[1][1] \* 7 = 49. 

On the eighth day, we buy product 1 from shop 0 for a price of values[0][1] \* 8 = 64 

On the ninth day, we buy product 0 from shop 1 for a price of values[1][0] \* 9 = 81. 

On the tenth day, we buy product 0 from shop 0 for a price of values[0][0] \* 10 = 100. 

Hence, our total spending is equal to 386. 

It can be shown that 386 is the maximum amount of money that can be spent buying all m \* n products.

**Constraints:**

*   `1 <= m == values.length <= 10`
*   <code>1 <= n == values[i].length <= 10<sup>4</sup></code>
*   <code>1 <= values[i][j] <= 10<sup>6</sup></code>
*   `values[i]` are sorted in non-increasing order.

## Solution

```java
public class Solution {
    private static class Node {
        int val = -1;
        Node next = null;

        public Node(int val) {
            this.val = val;
        }

        public Node() {}
    }

    public long maxSpending(int[][] values) {
        int m = values.length;
        int n = values[0].length;
        Node head = new Node();
        Node node = head;
        for (int j = n - 1; j >= 0; j--) {
            node.next = new Node(values[0][j]);
            node = node.next;
        }
        for (int i = 1; i < m; i++) {
            node = head;
            for (int j = n - 1; j >= 0; j--) {
                while (node.next != null && node.next.val <= values[i][j]) {
                    node = node.next;
                }
                Node next = node.next;
                node.next = new Node(values[i][j]);
                node = node.next;
                node.next = next;
            }
        }
        long res = 0;
        long day = 1;
        node = head.next;
        while (node != null) {
            res += day * node.val;
            node = node.next;
            day++;
        }
        return res;
    }
}
```