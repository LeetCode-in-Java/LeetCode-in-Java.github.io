[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 15\. 3Sum

Medium

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]

**Output:** [[-1,-1,2],[-1,0,1]] 

**Example 2:**

**Input:** nums = []

**Output:** [] 

**Example 3:**

**Input:** nums = [0]

**Output:** [] 

**Constraints:**

*   `0 <= nums.length <= 3000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("java:S127")
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        final int len = nums.length;
        List<List<Integer>> result = new ArrayList<>();
        int l;
        int r;
        for (int i = 0; i < len - 2; i++) {
            l = i + 1;
            r = len - 1;
            while (r > l) {
                int sum = nums[i] + nums[l] + nums[r];
                if (sum < 0) {
                    l++;
                } else if (sum > 0) {
                    r--;
                } else {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[l]);
                    list.add(nums[r]);
                    result.add(list);
                    while (l < r && nums[l + 1] == nums[l]) {
                        l++;
                    }
                    while (r > l && nums[r - 1] == nums[r]) {
                        r--;
                    }
                    l++;
                    r--;
                }
            }
            while (i < len - 1 && nums[i + 1] == nums[i]) {
                i++;
            }
        }
        return result;
    }
}
```

**Time Complexity (Big O Time):**

1. Sorting: The program sorts the input array `nums`. Sorting typically takes O(n * log(n)) time complexity, where 'n' is the length of the array.

2. Main Loop: The program uses nested loops. The outer loop runs from 0 to `len - 2`, where `len` is the length of the sorted array `nums`. The inner while loop has two pointers (`l` and `r`) that move towards each other. In the worst case, the inner loop can go through the entire array, so its time complexity is O(n).

3. The code inside the inner loop contains constant-time operations, such as addition, comparison, and list creation.

Overall, the dominant time complexity is determined by the sorting step, which is O(n * log(n)). The loop inside the sorting step has a complexity of O(n), but it doesn't dominate the overall complexity.

So, the total time complexity of the program is O(n * log(n)) due to the sorting step.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the space used for the output list `result`. In the worst case, when there are many unique triplets that sum to zero, the size of `result` can be significant.

1. The input array `nums` and the variables `len`, `l`, and `r` all use constant space.

2. The `result` list stores the triplets, and its size can be at most O(n^2) when there are many unique triplets.

3. Other variables and temporary lists used inside the loops use constant space.

So, the space complexity of the program is O(n^2) for the `result` list when considering the worst case.

In summary, the provided program has a time complexity of O(n * log(n)) due to sorting and a space complexity of O(n^2) for the `result` list when considering the worst-case scenario.
