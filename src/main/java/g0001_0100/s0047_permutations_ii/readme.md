[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 47\. Permutations II

Medium

Given a collection of numbers, `nums`, that might contain duplicates, return _all possible unique permutations **in any order**._

**Example 1:**

**Input:** nums = [1,1,2]

**Output:** [[1,1,2], [1,2,1], [2,1,1]] 

**Example 2:**

**Input:** nums = [1,2,3]

**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]] 

**Constraints:**

*   `1 <= nums.length <= 8`
*   `-10 <= nums[i] <= 10`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private List<List<Integer>> ans;

    public List<List<Integer>> permuteUnique(int[] nums) {
        ans = new ArrayList<>();
        permute(nums, 0);
        return ans;
    }

    private void permute(int[] nums, int p) {
        if (p >= nums.length - 1) {
            List<Integer> t = new ArrayList<>(nums.length);
            for (int n : nums) {
                t.add(n);
            }
            ans.add(t);
            return;
        }
        permute(nums, p + 1);
        boolean[] used = new boolean[30];
        for (int i = p + 1; i < nums.length; i++) {
            if (nums[i] != nums[p] && !used[10 + nums[i]]) {
                used[10 + nums[i]] = true;
                swap(nums, p, i);
                permute(nums, p + 1);
                swap(nums, p, i);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```