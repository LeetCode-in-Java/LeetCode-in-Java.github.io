[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2772\. Apply Operations to Make All Array Elements Equal to Zero

Medium

You are given a **0-indexed** integer array `nums` and a positive integer `k`.

You can apply the following operation on the array **any** number of times:

*   Choose **any** subarray of size `k` from the array and **decrease** all its elements by `1`.

Return `true` _if you can make all the array elements equal to_ `0`_, or_ `false` _otherwise_.

A **subarray** is a contiguous non-empty part of an array.

**Example 1:**

**Input:** nums = [2,2,3,1,1,0], k = 3

**Output:** true

**Explanation:**

We can do the following operations:

- Choose the subarray [2,2,3]. The resulting array will be nums = [**<ins>1</ins>**,**<ins>1</ins>**,**<ins>2</ins>**,1,1,0].

- Choose the subarray [2,1,1]. The resulting array will be nums = [1,1,**<ins>1</ins>**,**<ins>0</ins>**,**<ins>0</ins>**,0].

- Choose the subarray [1,1,1]. The resulting array will be nums = [<ins>**0**</ins>,<ins>**0**</ins>,<ins>**0**</ins>,0,0,0]. 

**Example 2:**

**Input:** nums = [1,3,1,1], k = 2

**Output:** false

**Explanation:** It is not possible to make all the array elements equal to 0. 

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public boolean checkArray(int[] nums, int k) {
        int cur = 0;
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (cur > nums[i]) {
                return false;
            }
            nums[i] -= cur;
            cur += nums[i];
            if (i >= k - 1) {
                cur -= nums[i - k + 1];
            }
        }
        return cur == 0;
    }
}
```