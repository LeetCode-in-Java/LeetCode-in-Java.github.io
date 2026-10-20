[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3947\. Maximum Number of Items From Sale II

Medium

You are given a 2D integer array `items`, where <code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code> represents the <code>i<sup>th</sup></code> item. You are also given an integer `budget`.

There are unlimited copies of each item available for purchase. You may buy any number of copies of any items such that the total cost of the purchased copies is at most `budget`.

After buying items, you may receive free copies according to the following rules:

*   Each purchased copy of item `i` can give you **at most one** free copy of another item `j`.
*   The free item must satisfy `i != j` and <code>factor<sub>i</sub></code> divides <code>factor<sub>j</sub></code>.
*   For each ordered pair `(i, j)`, you can receive a free copy of item `j` from purchases of item `i` **at most once**, regardless of how many copies of item `i` you buy.
*   The same item `j` can be received multiple times for free if it is received from purchases of different item types.

Return the **maximum total number of item copies** you can obtain, including both purchased copies and free copies, while spending at most `budget` on purchased items.

**Example 1:**

**Input:** items = \[\[1,6],[2,4],[3,5]], budget = 19

**Output:** 5

**Explanation:**

*   You can buy 2 copies of item 0 and 1 copy of item 1 for a total cost of `2 * 6 + 4 = 16`, which is not greater than `budget = 19`.
*   One purchased copy of item 0 gives 1 free copy of item 1, because <code>factor<sub>0</sub> = 1</code> divides <code>factor<sub>1</sub> = 2</code>.
*   The other purchased copy of item 0 gives 1 free copy of item 2, because <code>factor<sub>0</sub> = 1</code> divides <code>factor<sub>2</sub> = 3</code>.
*   You leave with 3 purchased copies and 2 free copies, for a total of 5 item copies.

**Example 2:**

**Input:** items = \[\[2,8],[1,10],[6,6],[4,12],[5,20],[5,17]], budget = 35

**Output:** 7

**Explanation:**

*   You can buy 2 copies of item 0, 1 copy of item 1, and 1 copy of item 2 for a total cost of `2 * 8 + 10 + 6 = 32`, which is not greater than `budget = 35`.
*   One purchased copy of item 0 gives 1 free copy of item 2, because <code>factor<sub>0</sub> = 2</code> divides <code>factor<sub>2</sub> = 6</code>.
*   The other purchased copy of item 0 gives 1 free copy of item 3, because <code>factor<sub>0</sub> = 2</code> divides <code>factor<sub>3</sub> = 4</code>.
*   The purchased copy of item 1 gives 1 free copy of item 2, because <code>factor<sub>1</sub> = 1</code> divides <code>factor<sub>2</sub> = 6</code>.
*   Buying item 2 gives no free copy, because <code>factor<sub>2</sub> = 6</code> does not divide the factor of any other item.
*   You leave with 4 purchased copies and 3 free copies, for a total of 7 item copies.

**Constraints:**

*   <code>1 <= items.length <= 10<sup>5</sup></code>
*   <code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code>
*   <code>1 <= factor<sub>i</sub> <= items.length</code>
*   <code>1 <= price<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= budget <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maximumSaleItems(int[][] items, int budget) {
        int n = items.length;
        int[] count = new int[n + 1];
        long min = Long.MAX_VALUE;
        for (int[] item : items) {
            count[item[0]]++;
            if (item[1] < min) {
                min = item[1];
            }
        }
        int[] temp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            if (count[i] > 0) {
                int m = -1;
                for (int j = i; j <= n; j += i) {
                    m += count[j];
                }
                temp[i] = m;
            }
        }
        long[] deal = new long[n];
        int dealcount = 0;
        long limit = min * 2;
        for (int[] item : items) {
            int first = item[0];
            int second = item[1];
            if (second < limit) {
                int m = temp[first];
                if (m > 0) {
                    deal[dealcount++] = ((long) second << 32) | (m & 0xFFFFFFFFL);
                }
            }
        }
        Arrays.sort(deal, 0, dealcount);
        long total = 0;
        for (int i = 0; i < dealcount; i++) {
            long deals = deal[i];
            int m = (int) (deals >>> 32);
            int counts = (int) (deals & 0xFFFFFFFFL);
            if (budget >= m) {
                long time = Math.min((long) counts, budget / m);
                budget = (int) (budget - (time * m));
                total += 2 * time;
            }
        }
        total += budget / min;
        return (int) total;
    }
}
```