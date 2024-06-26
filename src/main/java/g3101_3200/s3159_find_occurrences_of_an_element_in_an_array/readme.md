[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3159\. Find Occurrences of an Element in an Array

Medium

You are given an integer array `nums`, an integer array `queries`, and an integer `x`.

For each `queries[i]`, you need to find the index of the <code>queries[i]<sup>th</sup></code> occurrence of `x` in the `nums` array. If there are fewer than `queries[i]` occurrences of `x`, the answer should be -1 for that query.

Return an integer array `answer` containing the answers to all queries.

**Example 1:**

**Input:** nums = [1,3,1,7], queries = [1,3,2,4], x = 1

**Output:** [0,-1,2,-1]

**Explanation:**

*   For the 1<sup>st</sup> query, the first occurrence of 1 is at index 0.
*   For the 2<sup>nd</sup> query, there are only two occurrences of 1 in `nums`, so the answer is -1.
*   For the 3<sup>rd</sup> query, the second occurrence of 1 is at index 2.
*   For the 4<sup>th</sup> query, there are only two occurrences of 1 in `nums`, so the answer is -1.

**Example 2:**

**Input:** nums = [1,2,3], queries = [10], x = 5

**Output:** [-1]

**Explanation:**

*   For the 1<sup>st</sup> query, 5 doesn't exist in `nums`, so the answer is -1.

**Constraints:**

*   <code>1 <= nums.length, queries.length <= 10<sup>5</sup></code>
*   <code>1 <= queries[i] <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], x <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;

public class Solution {
    public int[] occurrencesOfElement(int[] nums, int[] queries, int x) {
        ArrayList<Integer> a = new ArrayList<>();
        for (int i = 0, l = nums.length; i < l; i++) {
            if (nums[i] == x) {
                a.add(i);
            }
        }
        int l = queries.length;
        int[] r = new int[l];
        for (int i = 0; i < l; i++) {
            r[i] = queries[i] > a.size() ? -1 : a.get(queries[i] - 1);
        }
        return r;
    }
}
```