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

The time complexity of this program is O(n^2), where "n" represents the number of elements in the `nums` array. Here's the breakdown:

1. The program starts by sorting the `nums` array, which has a time complexity of O(n * log(n)), where "n" is the length of the array.

2. After sorting, the program uses nested loops:
   - The outer loop runs for `i` from 0 to `len - 2`, where "len" is the length of the array. This loop iterates through each element of the array once, so it has a time complexity of O(n).

   - The inner loop uses two pointers (`l` and `r`) and runs until `r` is greater than `l`. Within the inner loop, constant-time operations are performed, such as calculating the sum of three elements and adjusting the pointers.

3. Inside the inner loop, there are additional while loops that skip duplicate elements. These while loops also perform constant-time operations.

Since the sorting step has a time complexity of O(n * log(n)), and the nested loops contribute O(n) iterations, the overall time complexity is dominated by the sorting step, resulting in O(n * log(n)).

**Space Complexity (Big O Space):**

The space complexity of this program is O(1) because it uses a constant amount of extra space. The program creates a few integer variables (`l`, `r`, `sum`, and `len`), but the space used by these variables is independent of the input size. Additionally, the `result` list stores the output, but its space is not considered part of the space complexity analysis, as it's required to store the program's output.

Therefore, the overall space complexity is O(1).
