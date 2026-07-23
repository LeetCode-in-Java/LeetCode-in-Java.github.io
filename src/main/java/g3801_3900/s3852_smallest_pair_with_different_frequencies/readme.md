[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3852\. Smallest Pair With Different Frequencies

Easy

You are given an integer array `nums`.

Consider all pairs of **distinct** values `x` and `y` from `nums` such that:

*   `x < y`
*   `x` and `y` have different frequencies in `nums`.

Among all such pairs:

*   Choose the pair with the smallest possible value of `x`.
*   If multiple pairs have the same `x`, choose the one with the smallest possible value of `y`.

Return an integer array `[x, y]`. If no valid pair exists, return `[-1, -1]`.

**Example 1:**

**Input:** nums = [1,1,2,2,3,4]

**Output:** [1,3]

**Explanation:**

The smallest value is 1 with a frequency of 2, and the smallest value greater than 1 that has a different frequency from 1 is 3 with a frequency of 1. Thus, the answer is `[1, 3]`.

**Example 2:**

**Input:** nums = [1,5]

**Output:** [-1,-1]

**Explanation:**

Both values have the same frequency, so no valid pair exists. Return `[-1, -1]`.

**Example 3:**

**Input:** nums = [7]

**Output:** [-1,-1]

**Explanation:**

There is only one value in the array, so no valid pair exists. Return `[-1, -1]`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int[] minDistinctFreqPair(int[] nums) {
        int[] freq = new int[101];
        for (int num : nums) {
            freq[num]++;
        }
        int first = -1;
        int second = -1;
        for (int i = 1; i <= 100; i++) {
            if (freq[i] != 0) {
                if (first == -1) {
                    first = i;
                } else if (freq[i] != freq[first]) {
                    second = i;
                    break;
                }
            }
        }
        return second == -1 ? new int[] {-1, -1} : new int[] {first, second};
    }
}
```