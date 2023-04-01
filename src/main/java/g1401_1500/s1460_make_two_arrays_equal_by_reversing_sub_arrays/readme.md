[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1460\. Make Two Arrays Equal by Reversing Sub-arrays

Easy

You are given two integer arrays of equal length `target` and `arr`. In one step, you can select any **non-empty sub-array** of `arr` and reverse it. You are allowed to make any number of steps.

Return `true` _if you can make_ `arr` _equal to_ `target`_or_ `false` _otherwise_.

**Example 1:**

**Input:** target = [1,2,3,4], arr = [2,4,1,3]

**Output:** true

**Explanation:** You can follow the next steps to convert arr to target: 

1- Reverse sub-array [2,4,1], arr becomes [1,4,2,3] 

2- Reverse sub-array [4,2], arr becomes [1,2,4,3] 

3- Reverse sub-array [4,3], arr becomes [1,2,3,4] 

There are multiple ways to convert arr to target, this is not the only way to do so.

**Example 2:**

**Input:** target = [7], arr = [7]

**Output:** true

**Explanation:** arr is equal to target without any reverses.

**Example 3:**

**Input:** target = [3,7,9], arr = [3,7,11]

**Output:** false

**Explanation:** arr does not have value 9 and it can never be converted to target.

**Constraints:**

*   `target.length == arr.length`
*   `1 <= target.length <= 1000`
*   `1 <= target[i] <= 1000`
*   `1 <= arr[i] <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean canBeEqual(int[] target, int[] arr) {
        int n = target.length;
        Arrays.sort(target);
        Arrays.sort(arr);
        int count = 0;
        for (int i = 0; i < target.length; i++) {
            if (target[i] == arr[i]) {
                count++;
            }
        }
        return count == n;
    }
}
```