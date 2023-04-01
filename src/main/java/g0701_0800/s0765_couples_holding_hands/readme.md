[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 765\. Couples Holding Hands

Hard

There are `n` couples sitting in `2n` seats arranged in a row and want to hold hands.

The people and seats are represented by an integer array `row` where `row[i]` is the ID of the person sitting in the <code>i<sup>th</sup></code> seat. The couples are numbered in order, the first couple being `(0, 1)`, the second couple being `(2, 3)`, and so on with the last couple being `(2n - 2, 2n - 1)`.

Return _the minimum number of swaps so that every couple is sitting side by side_. A swap consists of choosing any two people, then they stand up and switch seats.

**Example 1:**

**Input:** row = [0,2,1,3]

**Output:** 1

**Explanation:** We only need to swap the second (row[1]) and third (row[2]) person.

**Example 2:**

**Input:** row = [3,2,0,1]

**Output:** 0

**Explanation:** All couples are already seated side by side.

**Constraints:**

*   `2n == row.length`
*   `2 <= n <= 30`
*   `n` is even.
*   `0 <= row[i] < 2n`
*   All the elements of `row` are **unique**.

## Solution

```java
public class Solution {
    public int minSwapsCouples(int[] row) {
        int swaps = 0;
        for (int i = 0; i < row.length - 1; i += 2) {
            int coupleValue = row[i] % 2 == 0 ? row[i] + 1 : row[i] - 1;
            if (row[i + 1] != coupleValue) {
                swaps++;
                int coupleIndex = findIndex(row, coupleValue);
                swap(row, coupleIndex, i + 1);
            }
        }
        return swaps;
    }

    private void swap(int[] row, int i, int j) {
        int tmp = row[i];
        row[i] = row[j];
        row[j] = tmp;
    }

    private int findIndex(int[] row, int value) {
        for (int i = 0; i < row.length; i++) {
            if (row[i] == value) {
                return i;
            }
        }
        return -1;
    }
}
```