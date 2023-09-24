[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 300\. Longest Increasing Subsequence

Medium

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]

**Output:** 4

**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]

**Output:** 4 

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]

**Output:** 1 

**Constraints:**

*   `1 <= nums.length <= 2500`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

## Solution

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int[] dp = new int[nums.length + 1];
        // prefill the dp table
        for (int i = 1; i < dp.length; i++) {
            dp[i] = Integer.MAX_VALUE;
        }
        int left = 1;
        int right = 1;
        for (int curr : nums) {
            int start = left;
            int end = right;
            // binary search, find the one that is lower than curr
            while (start + 1 < end) {
                int mid = start + (end - start) / 2;
                if (dp[mid] > curr) {
                    end = mid;
                } else {
                    start = mid;
                }
            }
            // update our dp table
            if (dp[start] > curr) {
                dp[start] = curr;
            } else if (curr > dp[start] && curr < dp[end]) {
                dp[end] = curr;
            } else if (curr > dp[end]) {
                dp[++end] = curr;
                right++;
            }
        }
        return right;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses dynamic programming to find the LIS. It iterates through the input array `nums`, which has 'n' elements.
2. Inside the loop, a binary search is performed on the `dp` array to find the appropriate position to update. Binary search has a time complexity of O(log n).
3. The binary search is performed for each element in `nums`, so the total time complexity for the binary search part is O(n * log n).
4. The loop also includes constant-time operations, so the overall time complexity of the program is O(n * log n).

**Space Complexity (Big O Space):**

1. The program uses an additional integer array `dp` of length `nums.length + 1` to store intermediate results. Therefore, the space complexity is O(n) for the `dp` array.

In summary, the provided program has a time complexity of O(n * log n) and a space complexity of O(n) due to the dynamic programming array `dp`.
