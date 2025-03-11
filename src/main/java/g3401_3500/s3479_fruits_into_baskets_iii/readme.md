[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3479\. Fruits Into Baskets III

Medium

You are given two arrays of integers, `fruits` and `baskets`, each of length `n`, where `fruits[i]` represents the **quantity** of the <code>i<sup>th</sup></code> type of fruit, and `baskets[j]` represents the **capacity** of the <code>j<sup>th</sup></code> basket.

From left to right, place the fruits according to these rules:

*   Each fruit type must be placed in the **leftmost available basket** with a capacity **greater than or equal** to the quantity of that fruit type.
*   Each basket can hold **only one** type of fruit.
*   If a fruit type **cannot be placed** in any basket, it remains **unplaced**.

Return the number of fruit types that remain unplaced after all possible allocations are made.

**Example 1:**

**Input:** fruits = [4,2,5], baskets = [3,5,4]

**Output:** 1

**Explanation:**

*   `fruits[0] = 4` is placed in `baskets[1] = 5`.
*   `fruits[1] = 2` is placed in `baskets[0] = 3`.
*   `fruits[2] = 5` cannot be placed in `baskets[2] = 4`.

Since one fruit type remains unplaced, we return 1.

**Example 2:**

**Input:** fruits = [3,6,1], baskets = [6,4,7]

**Output:** 0

**Explanation:**

*   `fruits[0] = 3` is placed in `baskets[0] = 6`.
*   `fruits[1] = 6` cannot be placed in `baskets[1] = 4` (insufficient capacity) but can be placed in the next available basket, `baskets[2] = 7`.
*   `fruits[2] = 1` is placed in `baskets[1] = 4`.

Since all fruits are successfully placed, we return 0.

**Constraints:**

*   `n == fruits.length == baskets.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= fruits[i], baskets[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        int n = baskets.length;
        int size = 1;
        while (size < n) {
            size <<= 1;
        }
        int[] seg = new int[2 * size];
        for (int i = 0; i < n; i++) {
            seg[size + i] = baskets[i];
        }
        for (int i = n; i < size; i++) {
            seg[size + i] = 0;
        }
        for (int i = size - 1; i > 0; i--) {
            seg[i] = Math.max(seg[i << 1], seg[i << 1 | 1]);
        }
        int ans = 0;
        for (int f : fruits) {
            if (seg[1] < f) {
                ans++;
                continue;
            }
            int idx = 1;
            while (idx < size) {
                if (seg[idx << 1] >= f) {
                    idx = idx << 1;
                } else {
                    idx = idx << 1 | 1;
                }
            }
            update(seg, idx - size, 0, size);
        }
        return ans;
    }

    private void update(int[] seg, int pos, int val, int size) {
        int i = pos + size;
        seg[i] = val;
        for (i /= 2; i > 0; i /= 2) {
            seg[i] = Math.max(seg[i << 1], seg[i << 1 | 1]);
        }
    }
}
```