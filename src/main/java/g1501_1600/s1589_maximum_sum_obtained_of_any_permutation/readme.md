[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1589\. Maximum Sum Obtained of Any Permutation

Medium

We have an array of integers, `nums`, and an array of `requests` where <code>requests[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. The <code>i<sup>th</sup></code> request asks for the sum of <code>nums[start<sub>i</sub>] + nums[start<sub>i</sub> + 1] + ... + nums[end<sub>i</sub> - 1] + nums[end<sub>i</sub>]</code>. Both <code>start<sub>i</sub></code> and <code>end<sub>i</sub></code> are _0-indexed_.

Return _the maximum total sum of all requests **among all permutations** of_ `nums`.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3,4,5], requests = \[\[1,3],[0,1]]

**Output:** 19

**Explanation:** One permutation of nums is [2,1,3,4,5] with the following result:

requests[0] -> nums[1] + nums[2] + nums[3] = 1 + 3 + 4 = 8

requests[1] -> nums[0] + nums[1] = 2 + 1 = 3

Total sum: 8 + 3 = 11.

A permutation with a higher total sum is [3,5,4,2,1] with the following result:

requests[0] -> nums[1] + nums[2] + nums[3] = 5 + 4 + 2 = 11

requests[1] -> nums[0] + nums[1] = 3 + 5 = 8

Total sum: 11 + 8 = 19, which is the best that you can do.

**Example 2:**

**Input:** nums = [1,2,3,4,5,6], requests = \[\[0,1]]

**Output:** 11

**Explanation:** A permutation with the max total sum is [6,5,4,3,2,1] with request sums [11].

**Example 3:**

**Input:** nums = [1,2,3,4,5,10], requests = \[\[0,2],[1,3],[1,1]]

**Output:** 47

**Explanation:** A permutation with the max total sum is [4,10,5,3,2,1] with request sums [19,18,10].

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= requests.length <= 10<sup>5</sup></code>
*   `requests[i].length == 2`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> < n</code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxSumRangeQuery(int[] nums, int[][] requests) {
        Arrays.sort(nums);
        int l = nums.length;
        int[] tempArr = new int[l];
        // requests[i][0] incrementing index element by 1 and for requests[i][1]+1 decrementing by 1
        // this will help me get the freq of occurance of each index of array 'nums' in
        // all 'requests' intervals when I compute the sum array of tempArr.
        for (int[] request : requests) {
            int a = request[0];
            int b = request[1] + 1;
            tempArr[a]++;
            if (b < l) {
                tempArr[b]--;
            }
        }
        int prev = 0;
        for (int i = 0; i < l; i++) {
            tempArr[i] += prev;
            prev = tempArr[i];
        }
        Arrays.sort(tempArr);
        int index = l - 1;
        long ans = 0;
        while (index >= 0) {
            if (tempArr[index] == 0) {
                break;
            }
            long x = tempArr[index] % 1000000007;
            long y = nums[index] % 1000000007;
            index--;
            ans += x * y;
        }
        return (int) (ans % 1000000007);
    }
}
```