[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 832\. Flipping an Image

Easy

Given an `n x n` binary matrix `image`, flip the image **horizontally**, then invert it, and return _the resulting image_.

To flip an image horizontally means that each row of the image is reversed.

*   For example, flipping `[1,1,0]` horizontally results in `[0,1,1]`.

To invert an image means that each `0` is replaced by `1`, and each `1` is replaced by `0`.

*   For example, inverting `[0,1,1]` results in `[1,0,0]`.

**Example 1:**

**Input:** image = \[\[1,1,0],[1,0,1],[0,0,0]]

**Output:** [[1,0,0],[0,1,0],[1,1,1]]

**Explanation:**

First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].

Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]] 

**Example 2:**

**Input:** image = \[\[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]

**Output:** [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

**Explanation:**

First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].

Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]] 

**Constraints:**

*   `n == image.length`
*   `n == image[i].length`
*   `1 <= n <= 20`
*   `images[i][j]` is either `0` or `1`.

## Solution

```java
public class Solution {
    public int[][] flipAndInvertImage(int[][] image) {
        int m = image.length;
        int n = image[0].length;
        int[][] result = new int[m][n];
        for (int i = 0; i < m; i++) {
            int[] flipped = (reverse(image[i]));
            result[i] = invert(flipped);
        }
        return result;
    }

    private int[] invert(int[] flipped) {
        int[] result = new int[flipped.length];
        for (int i = 0; i < flipped.length; i++) {
            if (flipped[i] == 0) {
                result[i] = 1;
            } else {
                result[i] = 0;
            }
        }
        return result;
    }

    private int[] reverse(int[] nums) {
        for (int i = 0, j = nums.length - 1; i < j; i++, j--) {
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```