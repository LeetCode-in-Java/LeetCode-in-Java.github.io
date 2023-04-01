[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1552\. Magnetic Force Between Two Balls

Medium

In the universe Earth C-137, Rick discovered a special form of magnetic force between two balls if they are put in his new invented basket. Rick has `n` empty baskets, the <code>i<sup>th</sup></code> basket is at `position[i]`, Morty has `m` balls and needs to distribute the balls into the baskets such that the **minimum magnetic force** between any two balls is **maximum**.

Rick stated that magnetic force between two different balls at positions `x` and `y` is `|x - y|`.

Given the integer array `position` and the integer `m`. Return _the required force_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/11/q3v1.jpg)

**Input:** position = [1,2,3,4,7], m = 3

**Output:** 3

**Explanation:** Distributing the 3 balls into baskets 1, 4 and 7 will make the magnetic force between ball pairs [3, 3, 6]. The minimum magnetic force is 3. We cannot achieve a larger minimum magnetic force than 3.

**Example 2:**

**Input:** position = [5,4,3,2,1,1000000000], m = 2

**Output:** 999999999

**Explanation:** We can use baskets 1 and 1000000000.

**Constraints:**

*   `n == position.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= position[i] <= 10<sup>9</sup></code>
*   All integers in `position` are **distinct**.
*   `2 <= m <= position.length`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxDistance(int[] position, int m) {
        Arrays.sort(position);
        return binarySearch(position, m);
    }

    private int binarySearch(int[] arr, int m) {
        int low = 0;
        int n = arr.length;
        int high = arr[n - 1];
        int max = -1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (check(arr, mid, m)) {
                if (max < mid) {
                    max = mid;
                }
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return max;
    }

    private boolean check(int[] arr, int mid, int m) {
        int pos = arr[0];
        int magnet = 1;
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] - pos >= mid) {
                pos = arr[i];
                magnet += 1;
                if (magnet == m) {
                    return true;
                }
            }
        }
        return false;
    }
}
```