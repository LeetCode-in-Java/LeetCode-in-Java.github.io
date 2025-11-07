[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 238\. Product of Array Except Self

Medium

Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** [24,12,8,6] 

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]

**Output:** [0,0,9,0,0] 

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   `-30 <= nums[i] <= 30`
*   The input is generated such that `answer[i]` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

## Solution

```java
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        int prefixProduct = 1;
        for (int i = 0; i < nums.length; i++) {
            res[i] = prefixProduct;
            prefixProduct *= nums[i];
        }
        int suffixProduct = 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            res[i] *= suffixProduct;
            suffixProduct *= nums[i];
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

1. First Loop: In the first loop, the program calculates the product of all elements in the input array. This loop has a time complexity of O(n), where "n" is the length of the input array.

2. Second Loop: In the second loop, the program calculates the product of all elements in the input array except the current element. This loop is also O(n), where "n" is the length of the input array. Inside this loop, there's another loop that multiplies all elements except the current element. So, this inner loop has a time complexity of O(n) as well.

Overall, the time complexity of the program is dominated by the two nested loops, making it O(n^2) in the worst case.

**Space Complexity (Big O Space):**

1. The program uses an integer array `ans` to store the result, which has the same length as the input array. Therefore, the space complexity of the `ans` array is O(n).

2. The program uses integer variables `product`, `p`, and `j`, which consume a constant amount of space regardless of the input size. These variables do not depend on the length of the input array.

Therefore, the overall space complexity of the program is O(n) due to the `ans` array, where "n" is the length of the input array.

In summary, the provided program has a time complexity of O(n^2) and a space complexity of O(n), making it inefficient for larger input arrays. You can optimize the solution to achieve a better time complexity of O(n) and a space complexity of O(1) by calculating the product of all elements in two passes without using division or extra arrays.
