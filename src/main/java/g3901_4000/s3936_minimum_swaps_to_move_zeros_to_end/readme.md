[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3936\. Minimum Swaps to Move Zeros to End

Easy

You are given an integer array `nums`.

In one operation, you can choose any two **distinct** indices `i` and `j` and swap `nums[i]` and `nums[j]`.

Return an integer denoting the **minimum** number of operations required to move all 0s to the end of the array.

**Example 1:**

**Input:** nums = [0,1,0,3,12]

**Output:** 2

**Explanation:**

We perform the following swap operations:

*   Swap `nums[0]` and `nums[3]`, giving `nums = [3, 1, 0, 0, 12]`.
*   Swap `nums[2]` and `nums[4]`, giving `nums = [3, 1, 12, 0, 0]`.

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [0,1,0,2]

**Output:** 1

**Explanation:**

We perform the following swap operations:

*   Swap `nums[0]` and `nums[3]`, giving `nums = [2, 1, 0, 0]`.

Thus, the answer is 1.

**Example 3:**

**Input:** nums = [1,2,0]

**Output:** 0

**Explanation:**

The array already satisfies the condition. Therefore, no swap operations are needed.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    public int minimumSwaps(int[] nums) {
        int n = nums.length;
        int count = 0;
        int l = 0;
        int r = n - 1;
        while (l < r) {
            if (nums[l] == 0 && nums[r] != 0) {
                swap(nums, l, r);
                count++;
                l++;
                r--;
            } else if (nums[l] != 0) {
                l++;
            } else if (nums[r] == 0) {
                r--;
            }
        }
        return count;
    }
}
```