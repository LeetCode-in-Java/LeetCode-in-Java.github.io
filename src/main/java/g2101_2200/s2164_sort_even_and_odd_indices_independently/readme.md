[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2164\. Sort Even and Odd Indices Independently

Easy

You are given a **0-indexed** integer array `nums`. Rearrange the values of `nums` according to the following rules:

1.  Sort the values at **odd indices** of `nums` in **non-increasing** order.
    *   For example, if <code>nums = [4,**1**,2,**3**]</code> before this step, it becomes <code>[4,**3**,2,**1**]</code> after. The values at odd indices `1` and `3` are sorted in non-increasing order.
2.  Sort the values at **even indices** of `nums` in **non-decreasing** order.
    *   For example, if <code>nums = [**4**,1,**2**,3]</code> before this step, it becomes <code>[**2**,1,**4**,3]</code> after. The values at even indices `0` and `2` are sorted in non-decreasing order.

Return _the array formed after rearranging the values of_ `nums`.

**Example 1:**

**Input:** nums = [4,1,2,3]

**Output:** [2,3,4,1]

**Explanation:** 

First, we sort the values present at odd indices (1 and 3) in non-increasing order. 

So, nums changes from [4,**1**,2,**3**] to [4,**3**,2,**1**]. 

Next, we sort the values present at even indices (0 and 2) in non-decreasing order. So, nums changes from [**4**,1,**2**,3] to [**2**,3,**4**,1]. 

Thus, the array formed after rearranging the values is [2,3,4,1]. 

**Example 2:**

**Input:** nums = [2,1]

**Output:** [2,1]

**Explanation:** Since there is exactly one odd index and one even index, no rearrangement of values takes place. 

The resultant array formed is [2,1], which is the same as the initial array. 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] sortEvenOdd(int[] nums) {
        int[] odd = new int[nums.length / 2];
        int[] even = new int[(nums.length + 1) / 2];
        int o = 0;
        int e = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i % 2 == 0) {
                even[e] = nums[i];
                ++e;
            } else {
                odd[o] = nums[i];
                ++o;
            }
        }
        Arrays.sort(odd);
        Arrays.sort(even);
        e = 0;
        o = odd.length - 1;
        for (int i = 0; i < nums.length; i++) {
            if (i % 2 == 0) {
                nums[i] = even[e];
                ++e;
            } else {
                nums[i] = odd[o];
                --o;
            }
        }
        return nums;
    }
}
```