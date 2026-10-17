[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3937\. Minimum Operations to Make Array Modulo Alternating I

Medium

You are given an integer array `nums` and an integer `k`.

In one operation, you can **increase** or **decrease** any element of `nums` by 1.

An array is called **modulo alternating** if there exist two **distinct** integers `x` and `y` (`0 <= x, y < k`) such that:

*   For every **even** index `i`, `nums[i] % k == x`
*   For every **odd** index `i`, `nums[i] % k == y`

Return the **minimum** number of operations required to make `nums` **modulo alternating**.

**Example 1:**

**Input:** nums = [1,4,2,8], k = 3

**Output:** 2

**Explanation:**

*   Let's choose `x = 1` for even indices and `y = 2` for odd indices.
*   Perform the following operations:
    *   Increment `nums[1] = 4` by 1, giving `nums = [1, 5, 2, 8]`.
    *   Decrement `nums[2] = 2` by 1, giving `nums = [1, 5, 1, 8]`.
*   Now, for even indices, `nums[i] % k = 1`, and for odd indices, `nums[i] % k = 2`.
*   Thus, the total number of operations required is 2.

**Example 2:**

**Input:** nums = [1,1,1], k = 3

**Output:** 1

**Explanation:**

*   Incrementing `nums[1]` by 1 gives `nums = [1, 2, 1]`, which satisfies the condition with `x = 1` and `y = 2`.
*   Thus, the total number of operations required is 1.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `2 <= k <= 100`

## Solution

```java
public class Solution {
    public int minOperations(int[] nums, int k) {
        int[] even = new int[k];
        int[] odd = new int[k];
        for (int i = 0; i < nums.length; i++) {
            int rem = nums[i] % k;
            for (int j = 0; j < k; j++) {
                int diff = Math.abs(rem - j);
                int cost = Math.min(diff, k - diff);
                if (i % 2 == 0) {
                    even[j] += cost;
                } else {
                    odd[j] += cost;
                }
            }
        }
        int ans = Integer.MAX_VALUE;
        for (int x = 0; x < k; x++) {
            for (int y = 0; y < k; y++) {
                if (x != y) {
                    ans = Math.min(ans, odd[x] + even[y]);
                }
            }
        }
        return ans;
    }
}
```