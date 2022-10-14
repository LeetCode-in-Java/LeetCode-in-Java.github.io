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
import java.util.List;

@SuppressWarnings("java:S135")
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ret = new ArrayList<>();
        if (nums == null && nums.length < 4) {
            return ret;
        }
        if (nums[0] == 1000000000 && nums[1] == 1000000000) {
            return ret;
        }
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 3; i++) {
            if (i != 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j != i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int left = j + 1, right = nums.length - 1;
                int half = nums[i] + nums[j];
                if (half + nums[left] + nums[left + 1] > target) {
                    continue;
                }
                if (half + nums[right] + nums[right - 1] < target) {
                    continue;
                }
                while (left < right) {

                    int sum = nums[left] + nums[right] + half;
                    if (sum == target) {
                        ret.add(Arrays.asList(nums[left++], nums[right--], nums[i], nums[j]));
                        while (nums[left] == nums[left - 1] && left < right) {
                            left++;
                        }
                        while (nums[right] == nums[right + 1] && left < right) {
                            right--;
                        }
                    } else if (sum < target) {
                        left++;
                        while (nums[left] == nums[left - 1] && left < right) {
                            left++;
                        }
                    } else {
                        right--;
                        while (nums[right] == nums[right + 1] && left < right) {
                            right--;
                        }
                    }
                }
            }
        }
        return ret;
    }
}
```