[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2341\. Maximum Number of Pairs in Array

Easy

You are given a **0-indexed** integer array `nums`. In one operation, you may do the following:

*   Choose **two** integers in `nums` that are **equal**.
*   Remove both integers from `nums`, forming a **pair**.

The operation is done on `nums` as many times as possible.

Return _a **0-indexed** integer array_ `answer` _of size_ `2` _where_ `answer[0]` _is the number of pairs that are formed and_ `answer[1]` _is the number of leftover integers in_ `nums` _after doing the operation as many times as possible_.

**Example 1:**

**Input:** nums = [1,3,2,1,3,2,2]

**Output:** [3,1]

**Explanation:**

Form a pair with nums[0] and nums[3] and remove them from nums. Now, nums = [3,2,3,2,2].

Form a pair with nums[0] and nums[2] and remove them from nums. Now, nums = [2,2,2].

Form a pair with nums[0] and nums[1] and remove them from nums. Now, nums = [2].

No more pairs can be formed. A total of 3 pairs have been formed, and there is 1 number leftover in nums.

**Example 2:**

**Input:** nums = [1,1]

**Output:** [1,0]

**Explanation:** Form a pair with nums[0] and nums[1] and remove them from nums. Now, nums = [].

No more pairs can be formed. A total of 1 pair has been formed, and there are 0 numbers leftover in nums.

**Example 3:**

**Input:** nums = [0]

**Output:** [0,1]

**Explanation:** No pairs can be formed, and there is 1 number leftover in nums. 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] numberOfPairs(int[] nums) {
        Arrays.sort(nums);
        int pairs = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                nums[i] = -1;
                nums[i - 1] = -1;
                pairs++;
            }
        }
        return new int[] {pairs, nums.length - (2 * pairs)};
    }
}
```