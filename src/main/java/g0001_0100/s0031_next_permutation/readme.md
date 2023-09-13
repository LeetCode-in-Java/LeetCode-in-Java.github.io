[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 31\. Next Permutation

Medium

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [1,3,2] 

**Example 2:**

**Input:** nums = [3,2,1]

**Output:** [1,2,3] 

**Example 3:**

**Input:** nums = [1,1,5]

**Output:** [1,5,1] 

**Example 4:**

**Input:** nums = [1]

**Output:** [1] 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public void nextPermutation(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        int i = nums.length - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.length - 1;
            while (nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1, nums.length - 1);
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private void reverse(int[] nums, int i, int j) {
        while (i < j) {
            swap(nums, i++, j--);
        }
    }
}
```

**Time Complexity (Big O Time):**

1. The program first iterates backward through the array to find the first element `i` where `nums[i] < nums[i+1]`. This operation takes O(n) time, where n is the length of the array.

2. If such an element `i` is found, the program then iterates backward again to find the smallest element `j` to the right of `i` such that `nums[j] > nums[i]`. This operation also takes O(n) time, where n is the length of the array.

3. After finding `i` and `j`, the program swaps these two elements, which takes constant time O(1).

4. Finally, the program reverses the subarray to the right of `i` (from index `i+1` to the end of the array), which takes O(n) time in the worst case.

Overall, the time complexity of the program is dominated by the two linear scans through the array, so it's O(n), where n is the length of the input array `nums`.

**Space Complexity (Big O Space):**

The space complexity of the program is O(1) because it uses a constant amount of additional space for variables like `i`, `j`, and `temp`. Regardless of the size of the input array, the space used by these variables remains constant.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where n is the length of the input array `nums`.
