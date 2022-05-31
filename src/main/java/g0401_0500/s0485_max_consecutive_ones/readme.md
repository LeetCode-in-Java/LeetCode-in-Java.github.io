## 485\. Max Consecutive Ones

Easy

Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array_.

**Example 1:**

**Input:** nums = [1,1,0,1,1,1]

**Output:** 3

**Explanation:** The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

**Example 2:**

**Input:** nums = [1,0,1,1,0,1]

**Output:** 2

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```java
public class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int maxLen = 0;
        int i = 0;
        while (i < nums.length) {
            int currentLen = 0;
            while (i < nums.length && nums[i] == 1) {
                i++;
                currentLen++;
            }
            maxLen = Math.max(maxLen, currentLen);
            i++;
        }
        return maxLen;
    }
}
```