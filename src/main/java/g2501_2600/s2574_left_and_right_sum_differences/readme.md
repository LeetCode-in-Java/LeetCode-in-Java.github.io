[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2574\. Left and Right Sum Differences

Easy

Given a **0-indexed** integer array `nums`, find a **0-indexed** integer array `answer` where:

*   `answer.length == nums.length`.
*   `answer[i] = |leftSum[i] - rightSum[i]|`.

Where:

*   `leftSum[i]` is the sum of elements to the left of the index `i` in the array `nums`. If there is no such element, `leftSum[i] = 0`.
*   `rightSum[i]` is the sum of elements to the right of the index `i` in the array `nums`. If there is no such element, `rightSum[i] = 0`.

Return _the array_ `answer`.

**Example 1:**

**Input:** nums = [10,4,8,3]

**Output:** [15,1,11,22]

**Explanation:** The array leftSum is [0,10,14,22] and the array rightSum is [15,11,3,0]. The array answer is [\|0 - 15\|,\|10 - 11\|,\|14 - 3\|,\|22 - 0\|] = [15,1,11,22].

**Example 2:**

**Input:** nums = [1]

**Output:** [0]

**Explanation:** The array leftSum is [0] and the array rightSum is [0]. The array answer is [\|0 - 0\|] = [0].

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] leftRightDifference(int[] nums) {
        int ls = 0;
        int rs = 0;
        for (int i : nums) {
            rs += i;
        }
        for (int i = 0; i < nums.length; ++i) {
            ls += nums[i];
            rs -= nums[i];
            nums[i] = Math.abs(rs - (ls - nums[i]));
        }
        return nums;
    }
}
```