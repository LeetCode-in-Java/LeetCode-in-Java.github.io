[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 835\. Image Overlap

Medium

You are given two images, `img1` and `img2`, represented as binary, square matrices of size `n x n`. A binary matrix has only `0`s and `1`s as values.

We **translate** one image however we choose by sliding all the `1` bits left, right, up, and/or down any number of units. We then place it on top of the other image. We can then calculate the **overlap** by counting the number of positions that have a `1` in **both** images.

Note also that a translation does **not** include any kind of rotation. Any `1` bits that are translated outside of the matrix borders are erased.

Return _the largest possible overlap_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/09/overlap1.jpg)

**Input:** img1 = \[\[1,1,0],[0,1,0],[0,1,0]], img2 = \[\[0,0,0],[0,1,1],[0,0,1]]

**Output:** 3

**Explanation:** We translate img1 to right by 1 unit and down by 1 unit. ![](https://assets.leetcode.com/uploads/2020/09/09/overlap_step1.jpg) The number of positions that have a 1 in both images is 3 (shown in red). ![](https://assets.leetcode.com/uploads/2020/09/09/overlap_step2.jpg)

**Example 2:**

**Input:** img1 = \[\[1]], img2 = \[\[1]]

**Output:** 1

**Example 3:**

**Input:** img1 = \[\[0]], img2 = \[\[0]]

**Output:** 0

**Constraints:**

*   `n == img1.length == img1[i].length`
*   `n == img2.length == img2[i].length`
*   `1 <= n <= 30`
*   `img1[i][j]` is either `0` or `1`.
*   `img2[i][j]` is either `0` or `1`.

## Solution

```java
public class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {
        int[] bits1 = bitwise(img1);
        int[] bits2 = bitwise(img2);
        int n = img1.length;
        int res = 0;
        for (int hori = -1 * n + 1; hori < n; hori++) {
            for (int veti = -1 * n + 1; veti < n; veti++) {
                int curOverLapping = 0;
                if (veti < 0) {
                    for (int i = -1 * veti; i < n; i++) {
                        if (hori < 0) {
                            curOverLapping +=
                                    Integer.bitCount(
                                            (bits1[i] << -1 * hori) & bits2[i - -1 * veti]);
                        } else {
                            curOverLapping +=
                                    Integer.bitCount((bits1[i] >> hori) & bits2[i - -1 * veti]);
                        }
                    }
                } else {
                    for (int i = 0; i < n - veti; i++) {
                        if (hori < 0) {
                            curOverLapping +=
                                    Integer.bitCount((bits1[i] << -1 * hori) & bits2[veti + i]);
                        } else {
                            curOverLapping +=
                                    Integer.bitCount((bits1[i] >> hori) & bits2[veti + i]);
                        }
                    }
                }
                res = Math.max(res, curOverLapping);
            }
        }
        return res;
    }

    private int[] bitwise(int[][] img) {
        int[] bits = new int[img.length];
        for (int i = 0; i < img.length; i++) {
            int cur = 0;
            for (int j = 0; j < img[0].length; j++) {
                cur = cur * 2 + img[i][j];
            }
            bits[i] = cur;
        }
        return bits;
    }
}
```