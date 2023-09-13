[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 53\. Maximum Subarray

Easy

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return _its sum_.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]

**Output:** 6

**Explanation:** [4,-1,2,1] has the largest sum = 6. 

**Example 2:**

**Input:** nums = [1]

**Output:** 1 

**Example 3:**

**Input:** nums = [5,4,-1,7,8]

**Output:** 23 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

## Solution

```java
public class Solution {
    public int maxSubArray(int[] nums) {
        int maxi = Integer.MIN_VALUE;
        int sum = 0;
        for (int num : nums) {
            // calculating sub-array sum
            sum += num;
            maxi = Math.max(sum, maxi);
            if (sum < 0) {
                // there is no point to carry a -ve subarray sum. hence setting to 0
                sum = 0;
            }
        }
        return maxi;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a single loop to iterate through the elements of the `nums` array. Let's assume there are 'n' elements in the array.

2. In each iteration of the loop, the program performs constant time operations such as addition, comparison, and calls to `Math.max`.

3. Therefore, the time complexity of the program is O(n), where 'n' is the number of elements in the input array `nums`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array.

2. The program uses a few integer variables (`maxi` and `sum`) to keep track of the maximum subarray sum and the current subarray sum, respectively. These variables require constant space.

3. The space complexity does not depend on the size of the input array `nums`.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where 'n' is the number of elements in the input array `nums`. The program efficiently finds the maximum subarray sum using a single pass through the array.
