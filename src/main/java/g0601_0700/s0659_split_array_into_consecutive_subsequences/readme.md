[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 659\. Split Array into Consecutive Subsequences

Medium

You are given an integer array `nums` that is **sorted in non-decreasing order**.

Determine if it is possible to split `nums` into **one or more subsequences** such that **both** of the following conditions are true:

*   Each subsequence is a **consecutive increasing sequence** (i.e. each integer is **exactly one** more than the previous integer).
*   All subsequences have a length of `3` **or more**.

Return `true` _if you can split_ `nums` _according to the above conditions, or_ `false` _otherwise_.

A **subsequence** of an array is a new array that is formed from the original array by deleting some (can be none) of the elements without disturbing the relative positions of the remaining elements. (i.e., `[1,3,5]` is a subsequence of `[1,2,3,4,5]` while `[1,3,2]` is not).

**Example 1:**

**Input:** nums = [1,2,3,3,4,5]

**Output:** true

**Explanation:** nums can be split into the following subsequences: 

[**1**,**2**,**3**,3,4,5] --> 1, 2, 3 

[1,2,3,**3**,**4**,**5**] --> 3, 4, 5

**Example 2:**

**Input:** nums = [1,2,3,3,4,4,5,5]

**Output:** true

**Explanation:** nums can be split into the following subsequences: 

[**1**,**2**,**3**,3,**4**,4,**5**,5] --> 1, 2, 3, 4, 5 

[1,2,3,**3**,4,**4**,5,**5**] --> 3, 4, 5

**Example 3:**

**Input:** nums = [1,2,3,4,4,5]

**Output:** false

**Explanation:** It is impossible to split nums into consecutive increasing subsequences of length 3 or more.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   `nums` is sorted in **non-decreasing** order.

## Solution

```java
public class Solution {
    public boolean isPossible(int[] nums) {
        int[] element = new int[2001];
        for (int num : nums) {
            element[num + 1000] += 1;
        }
        for (int i = 0; i < element.length; i++) {
            while (element[i] > 0) {
                int length = 1;
                while (i + length < element.length
                        && element[i + length] >= element[i + length - 1]) {
                    length += 1;
                }
                if (length < 3) {
                    return false;
                } else {
                    for (int j = i; j < i + length; j++) {
                        element[j] -= 1;
                    }
                }
            }
        }
        return true;
    }
}
```