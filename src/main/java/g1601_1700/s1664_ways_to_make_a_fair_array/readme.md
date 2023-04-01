[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1664\. Ways to Make a Fair Array

Medium

You are given an integer array `nums`. You can choose **exactly one** index (**0-indexed**) and remove the element. Notice that the index of the elements may change after the removal.

For example, if `nums = [6,1,7,4,1]`:

*   Choosing to remove index `1` results in `nums = [6,7,4,1]`.
*   Choosing to remove index `2` results in `nums = [6,1,4,1]`.
*   Choosing to remove index `4` results in `nums = [6,1,7,4]`.

An array is **fair** if the sum of the odd-indexed values equals the sum of the even-indexed values.

Return the _**number** of indices that you could choose such that after the removal,_ `nums` _is **fair**._

**Example 1:**

**Input:** nums = [2,1,6,4]

**Output:** 1

**Explanation:**

Remove index 0: [1,6,4] -> Even sum: 1 + 4 = 5. Odd sum: 6. Not fair.

Remove index 1: [2,6,4] -> Even sum: 2 + 4 = 6. Odd sum: 6. Fair.

Remove index 2: [2,1,4] -> Even sum: 2 + 4 = 6. Odd sum: 1. Not fair.

Remove index 3: [2,1,6] -> Even sum: 2 + 6 = 8. Odd sum: 1. Not fair.

There is 1 index that you can remove to make nums fair.

**Example 2:**

**Input:** nums = [1,1,1]

**Output:** 3

**Explanation:** You can remove any index and the remaining array is fair.

**Example 3:**

**Input:** nums = [1,2,3]

**Output:** 0

**Explanation:** You cannot make a fair array after removing any index.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int waysToMakeFair(int[] nums) {
        int res = 0;
        int[] even = new int[nums.length];
        int[] odd = new int[nums.length];
        int oddSum = 0;
        int evenSum = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i % 2 == 0) {
                evenSum += nums[i];
            } else {
                oddSum += nums[i];
            }
            even[i] = evenSum;
            odd[i] = oddSum;
        }
        for (int i = 0; i < nums.length; i++) {
            if (i == 0) {
                evenSum = odd[nums.length - 1] - odd[0];
                oddSum = even[nums.length - 1] - even[0];
            } else {
                oddSum = odd[i - 1] + even[nums.length - 1] - even[i];
                evenSum = even[i - 1] + odd[nums.length - 1] - odd[i];
            }
            if (evenSum == oddSum) {
                res++;
            }
        }
        return res;
    }
}
```