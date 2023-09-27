[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2584\. Split the Array to Make Coprime Products

Hard

You are given a **0-indexed** integer array `nums` of length `n`.

A **split** at an index `i` where `0 <= i <= n - 2` is called **valid** if the product of the first `i + 1` elements and the product of the remaining elements are coprime.

*   For example, if `nums = [2, 3, 3]`, then a split at the index `i = 0` is valid because `2` and `9` are coprime, while a split at the index `i = 1` is not valid because `6` and `3` are not coprime. A split at the index `i = 2` is not valid because `i == n - 1`.

Return _the smallest index_ `i` _at which the array can be split validly or_ `-1` _if there is no such split_.

Two values `val1` and `val2` are coprime if `gcd(val1, val2) == 1` where `gcd(val1, val2)` is the greatest common divisor of `val1` and `val2`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)

**Input:** nums = [4,7,8,15,3,5]

**Output:** 2

**Explanation:** The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i. The only valid split is at index 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)

**Input:** nums = [4,7,15,8,3,5]

**Output:** -1

**Explanation:** The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i. There is no valid split.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@SuppressWarnings("unchecked")
public class Solution {
    private final int[] divideTo = new int[(int) (1e6) + 1];

    private void fillDivideArray() {
        for (int i = 1; i < divideTo.length; i++) {
            divideTo[i] = i;
        }
        for (int i = 2; i * i < divideTo.length; i++) {
            if (divideTo[i] != i) {
                continue;
            }
            for (int j = i + i; j < divideTo.length; j += i) {
                if (divideTo[j] == j) {
                    divideTo[j] = i;
                }
            }
        }
    }

    public int findValidSplit(int[] nums) {
        if (divideTo[1] == 0) {
            fillDivideArray();
        }
        Map<Integer, Integer> dMap = new HashMap<>();
        List<Integer>[] dividers = new List[nums.length];
        for (int i = 0; i < nums.length; i++) {
            dividers[i] = new ArrayList<>();
            while (nums[i] != 1) {
                dividers[i].add(divideTo[nums[i]]);
                dMap.put(divideTo[nums[i]], i);
                nums[i] = nums[i] / divideTo[nums[i]];
            }
        }
        int nextEnd = 0;
        int i = 0;
        while (i <= nextEnd) {
            for (int j = 0; j < dividers[i].size(); j++) {
                nextEnd = Math.max(nextEnd, dMap.get(dividers[i].get(j)));
            }
            i++;
        }
        if (i == nums.length) {
            return -1;
        }
        return i - 1;
    }
}
```