[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2681\. Power of Heroes

Hard

You are given a **0-indexed** integer array `nums` representing the strength of some heroes. The **power** of a group of heroes is defined as follows:

*   Let <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, ... ,<code>i<sub>k</sub></code> be the indices of the heroes in a group. Then, the power of this group is <code>max(nums[i<sub>0</sub>], nums[i<sub>1</sub>], ... ,nums[i<sub>k</sub>])<sup>2</sup> * min(nums[i<sub>0</sub>], nums[i<sub>1</sub>], ... ,nums[i<sub>k</sub>])</code>.

Return _the sum of the **power** of all **non-empty** groups of heroes possible._ Since the sum could be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [2,1,4]

**Output:** 141

**Explanation:** 

1<sup>st</sup> group: [2] has power = 2<sup>2</sup> \* 2 = 8. 

2<sup>nd</sup> group: [1] has power = 1<sup>2</sup> \* 1 = 1. 

3<sup>rd</sup> group: [4] has power = 4<sup>2</sup> \* 4 = 64. 

4<sup>th</sup> group: [2,1] has power = 2<sup>2</sup> \* 1 = 4. 

5<sup>th</sup> group: [2,4] has power = 4<sup>2</sup> \* 2 = 32. 

6<sup>th</sup> group: [1,4] has power = 4<sup>2</sup> \* 1 = 16. 

7<sup>th</sup> group: [2,1,4] has power = 4<sup>2</sup> \* 1 = 16. 

The sum of powers of all groups is 8 + 1 + 64 + 4 + 32 + 16 + 16 = 141.

**Example 2:**

**Input:** nums = [1,1,1]

**Output:** 7

**Explanation:** A total of 7 groups are possible, and the power of each group will be 1. Therefore, the sum of the powers of all groups is 7.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int sumOfPower(int[] nums) {
        Arrays.sort(nums);
        long sumOfMin = 0L;
        long res = 0L;
        for (int num : nums) {
            long mod = 1_000_000_007L;
            long max = (long) num * num % mod;
            long mySumOfMin = (sumOfMin + num) % mod;
            res = (res + max * mySumOfMin) % mod;
            sumOfMin = (sumOfMin + mySumOfMin) % mod;
        }
        return (int) res;
    }
}
```