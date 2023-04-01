[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1354\. Construct Target Array With Multiple Sums

Hard

You are given an array `target` of n integers. From a starting array `arr` consisting of `n` 1's, you may perform the following procedure :

*   let `x` be the sum of all elements currently in your array.
*   choose index `i`, such that `0 <= i < n` and set the value of `arr` at index `i` to `x`.
*   You may repeat this procedure as many times as needed.

Return `true` _if it is possible to construct the_ `target` _array from_ `arr`_, otherwise, return_ `false`.

**Example 1:**

**Input:** target = [9,3,5]

**Output:** true

**Explanation:** Start with arr = [1, 1, 1] 

[1, 1, 1], sum = 3 choose index 1 

[1, 3, 1], sum = 5 choose index 2 

[1, 3, 5], sum = 9 choose index 0 

[9, 3, 5] Done

**Example 2:**

**Input:** target = [1,1,1,2]

**Output:** false

**Explanation:** Impossible to create target array from [1,1,1,1].

**Example 3:**

**Input:** target = [8,5]

**Output:** true

**Constraints:**

*   `n == target.length`
*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= target[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public boolean isPossible(int[] target) {
        long sum = target[0];
        int maxIndex = 0;
        for (int i = 1; i < target.length; i++) {
            sum += target[i];
            if (target[i] > target[maxIndex]) {
                maxIndex = i;
            }
        }
        long remainingSum = sum - target[maxIndex];
        if (target[maxIndex] == 1 || remainingSum == 1) {
            return true;
        }
        if (remainingSum >= target[maxIndex]
                || remainingSum == 0
                || target[maxIndex] % remainingSum == 0) {
            return false;
        }
        target[maxIndex] %= remainingSum;
        return isPossible(target);
    }
}
```