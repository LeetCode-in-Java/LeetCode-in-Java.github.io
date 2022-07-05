[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 18\. 4Sum

Medium

Given an array `nums` of `n` integers, return _an array of all the **unique** quadruplets_ `[nums[a], nums[b], nums[c], nums[d]]` such that:

*   `0 <= a, b, c, d < n`
*   `a`, `b`, `c`, and `d` are **distinct**.
*   `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,0,-1,0,-2,2], target = 0

**Output:** [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]] 

**Example 2:**

**Input:** nums = [2,2,2,2,2], target = 8

**Output:** [[2,2,2,2]] 

**Constraints:**

*   `1 <= nums.length <= 200`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedHashSet;
import java.util.List;

@SuppressWarnings("java:S135")
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList<>();
        int i;
        int j;
        int k;
        int l;
        Arrays.sort(nums);
        for (i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (j = i + 1; j < nums.length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                k = j + 1;
                l = nums.length - 1;
                while (k < l) {
                    int sum = nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        ArrayList<Integer> l1 = new ArrayList<>();
                        l1.add(nums[i]);
                        l1.add(nums[j]);
                        l1.add(nums[k]);
                        l1.add(nums[l]);
                        list.add(l1);
                        l--;
                        if (k < l && nums[l] == nums[l + 1]) {
                            l--;
                        }
                    } else if (sum > target) {
                        l--;
                    } else {
                        k++;
                    }
                }
            }
        }
        list = new ArrayList<>(new LinkedHashSet<>(list));
        return list;
    }
}
```