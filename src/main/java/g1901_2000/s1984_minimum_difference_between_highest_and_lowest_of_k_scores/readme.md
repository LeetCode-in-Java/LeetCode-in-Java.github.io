[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1984\. Minimum Difference Between Highest and Lowest of K Scores

Easy

You are given a **0-indexed** integer array `nums`, where `nums[i]` represents the score of the <code>i<sup>th</sup></code> student. You are also given an integer `k`.

Pick the scores of any `k` students from the array so that the **difference** between the **highest** and the **lowest** of the `k` scores is **minimized**.

Return _the **minimum** possible difference_.

**Example 1:**

**Input:** nums = [90], k = 1

**Output:** 0

**Explanation:** There is one way to pick score(s) of one student: 

- \[**90**]. The difference between the highest and lowest score is 90 - 90 = 0. 
  
The minimum possible difference is 0.

**Example 2:**

**Input:** nums = [9,4,1,7], k = 2

**Output:** 2

**Explanation:** There are six ways to pick score(s) of two students: 

- \[**9**,**4**,1,7]. The difference between the highest and lowest score is 9 - 4 = 5. 

- \[**9**,4,**1**,7]. The difference between the highest and lowest score is 9 - 1 = 8. 

- \[**9**,4,1,**7**]. The difference between the highest and lowest score is 9 - 7 = 2. 

- \[9,**4**,**1**,7]. The difference between the highest and lowest score is 4 - 1 = 3. 

- \[9,**4**,1,**7**]. The difference between the highest and lowest score is 7 - 4 = 3. 

- \[9,4,**1**,**7**]. The difference between the highest and lowest score is 7 - 1 = 6. 
  
The minimum possible difference is 2.

**Constraints:**

*   `1 <= k <= nums.length <= 1000`
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumDifference(int[] nums, int k) {
        Arrays.sort(nums);
        int minDiff = nums[nums.length - 1];
        for (int i = 0; i <= nums.length - k; i++) {
            minDiff = Math.min(minDiff, nums[i + k - 1] - nums[i]);
        }
        return minDiff;
    }
}
```