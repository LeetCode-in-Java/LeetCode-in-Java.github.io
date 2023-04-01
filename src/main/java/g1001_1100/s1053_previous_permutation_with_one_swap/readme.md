[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1053\. Previous Permutation With One Swap

Medium

Given an array of positive integers `arr` (not necessarily distinct), return _the lexicographically largest permutation that is smaller than_ `arr`, that can be **made with exactly one swap** (A _swap_ exchanges the positions of two numbers `arr[i]` and `arr[j]`). If it cannot be done, then return the same array.

**Example 1:**

**Input:** arr = [3,2,1]

**Output:** [3,1,2]

**Explanation:** Swapping 2 and 1.

**Example 2:**

**Input:** arr = [1,1,5]

**Output:** [1,1,5]

**Explanation:** This is already the smallest permutation.

**Example 3:**

**Input:** arr = [1,9,4,6,7]

**Output:** [1,7,4,6,9]

**Explanation:** Swapping 9 and 7.

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int[] prevPermOpt1(int[] arr) {
        for (int i = arr.length - 1; i >= 0; i--) {
            int diff = Integer.MAX_VALUE;
            int index = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[i] - arr[j] > 0 && diff > arr[i] - arr[j]) {
                    diff = arr[i] - arr[j];
                    index = j;
                }
            }
            if (diff != Integer.MAX_VALUE) {
                int temp = arr[i];
                arr[i] = arr[index];
                arr[index] = temp;
                break;
            }
        }
        return arr;
    }
}
```