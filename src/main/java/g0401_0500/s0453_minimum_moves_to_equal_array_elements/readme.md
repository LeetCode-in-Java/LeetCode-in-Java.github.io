[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 453\. Minimum Moves to Equal Array Elements

Medium

Given an integer array `nums` of size `n`, return _the minimum number of moves required to make all array elements equal_.

In one move, you can increment `n - 1` elements of the array by `1`.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 3

**Explanation:** Only three moves are needed (remember each move increments two elements): [1,2,3] => [2,3,3] => [3,4,3] => [4,4,4]

**Example 2:**

**Input:** nums = [1,1,1]

**Output:** 0

**Constraints:**

*   `n == nums.length`
*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   The answer is guaranteed to fit in a **32-bit** integer.

## Solution

```java
public class Solution {
    public int minMoves(int[] nums) {
        int min = nums[0];
        int sum = nums[0];
        // determining the total sum and smallest element of the input array
        for (int i = 1; i <= nums.length - 1; i++) {
            sum += nums[i];
            min = Math.min(min, nums[i]);
        }
        return sum - (min * nums.length);
    }
}
```