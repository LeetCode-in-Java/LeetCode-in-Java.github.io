[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 41\. First Missing Positive

Hard

Given an unsorted integer array `nums`, return the smallest missing positive integer.

You must implement an algorithm that runs in `O(n)` time and uses constant extra space.

**Example 1:**

**Input:** nums = [1,2,0]

**Output:** 3 

**Example 2:**

**Input:** nums = [3,4,-1,1]

**Output:** 2 

**Example 3:**

**Input:** nums = [7,8,9,11,12]

**Output:** 1 

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>5</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public int firstMissingPositive(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] <= 0 || nums[i] > nums.length || nums[i] == i + 1) {
                continue;
            }
            dfs(nums, nums[i]);
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return nums.length + 1;
    }

    private void dfs(int[] nums, int val) {
        if (val <= 0 || val > nums.length || val == nums[val - 1]) {
            return;
        }
        int temp = nums[val - 1];
        nums[val - 1] = val;
        dfs(nums, temp);
    }
}
```

**Time Complexity (Big O Time):**

1. The program contains two loops:
   - The first loop iterates through the array `nums` once to perform a depth-first search (DFS) on the elements.
   - The second loop iterates through the modified `nums` array once to find the first missing positive integer.

2. In the worst case, the first loop may visit every element in the `nums` array, which has a length of n (the number of elements in the array).

3. The DFS recursion also runs in the worst case for every element in the array. In the worst case, the recursion depth could be n (for example, if the array contains all positive integers from 1 to n).

4. The second loop always iterates through the entire array once.

5. Therefore, the overall time complexity of the program is O(n), where n is the length of the input array `nums`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the space used for the call stack during the DFS recursion and the constant space used for variables.

2. The maximum depth of the DFS recursion is bounded by n in the worst case (when the array contains all positive integers from 1 to n). Therefore, the space used for the call stack is O(n).

3. The program uses a few integer variables (`i`, `val`, `temp`) and does not use any additional data structures that scale with the input size.

4. Therefore, the overall space complexity of the program is O(n) due to the space used for the call stack, where n is the length of the input array `nums`.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(n), where n is the number of elements in the input array `nums`.
