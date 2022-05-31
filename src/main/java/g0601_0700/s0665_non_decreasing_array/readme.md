## 665\. Non-decreasing Array

Medium

Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most one element**.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (**0-based**) such that (`0 <= i <= n - 2`).

**Example 1:**

**Input:** nums = [4,2,3]

**Output:** true

**Explanation:** You could modify the first `4` to `1` to get a non-decreasing array.

**Example 2:**

**Input:** nums = [4,2,1]

**Output:** false

**Explanation:** You can't get a non-decreasing array by modify at most one element.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public boolean checkPossibility(int[] nums) {
        int count = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < nums[i - 1]) {
                if (i - 2 >= 0 && nums[i] < nums[i - 2]) {
                    nums[i] = nums[i - 1];
                }
                count++;
            }
            if (count >= 2) {
                return false;
            }
        }
        return true;
    }
}
```