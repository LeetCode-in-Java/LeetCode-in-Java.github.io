[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 645\. Set Mismatch

Easy

You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in **repetition of one** number and **loss of another** number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return _them in the form of an array_.

**Example 1:**

**Input:** nums = [1,2,2,4]

**Output:** [2,3]

**Example 2:**

**Input:** nums = [1,1]

**Output:** [1,2]

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] ans = new int[2];
        int i = 0;
        while (i < nums.length) {
            int correct = nums[i] - 1;
            if (nums[i] != nums[correct]) {
                int temp = nums[i];
                nums[i] = nums[correct];
                nums[correct] = temp;
            } else {
                i++;
            }
        }
        for (int a = 0; a < nums.length; a++) {
            if (nums[a] != a + 1) {
                ans[0] = nums[a];
                ans[1] = a + 1;
                break;
            }
        }
        return ans;
    }
}
```