[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3766\. Minimum Operations to Make Binary Palindrome

Medium

You are given an integer array `nums`.

For each element `nums[i]`, you may perform the following operations **any** number of times (including zero):

*   Increase `nums[i]` by 1, or
*   Decrease `nums[i]` by 1.

A number is called a **binary palindrome** if its binary representation without leading zeros reads the same forward and backward.

Your task is to return an integer array `ans`, where `ans[i]` represents the **minimum** number of operations required to convert `nums[i]` into a **binary palindrome**.

**Example 1:**

**Input:** nums = [1,2,4]

**Output:** [0,1,1]

**Explanation:**

One optimal set of operations:

| `nums[i]` | Binary(`nums[i]`) | Nearest Palindrome | Binary (Palindrome) | Operations Required | `ans[i]` |
|---|---|---|---|---|---|
| 1 | 1 | 1 | 1 | Already palindrome | 0 |
| 2 | 10 | 3 | 11 | Increase by 1 | 1 |
| 4 | 100 | 3 | 11 | Decrease by 1 | 1 |

Thus, `ans = [0, 1, 1]`.

**Example 2:**

**Input:** nums = [6,7,12]

**Output:** [1,0,3]

**Explanation:**

One optimal set of operations:

| `nums[i]` | Binary(`nums[i]`) | Nearest Palindrome | Binary (Palindrome) | Operations Required | `ans[i]` |
|---|---|---|---|---|---|
| 6 | 110 | 5 | 101 | Decrease by 1 | 1 |
| 7 | 111 | 7 | 111 | Already palindrome | 0 |
| 12 | 1100 | 15 | 1111 | Increase by 3 | 3 |

Thus, `ans = [1, 0, 3]`.

**Constraints:**

*   `1 <= nums.length <= 5000`
*   `1 <= nums[i] <= 5000`

## Solution

```java
public class Solution {
    private int binlen(int n) {
        int c = 0;
        while (n > 0) {
            c++;
            n >>= 1;
        }
        return c;
    }

    private boolean isPal(int n) {
        int l = 0;
        int r = binlen(n) - 1;
        while (l < r) {
            if (((n >> l) & 1) != ((n >> r) & 1)) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }

    public int[] minOperations(int[] nums) {
        boolean[] binary = new boolean[5050];
        int[] ans = new int[nums.length];
        for (int i = 0; i < 5050; i++) {
            binary[i] = isPal(i);
        }
        for (int i = 0; i < nums.length; i++) {
            int a = nums[i];
            int b = nums[i];
            int c1 = 0;
            int c2 = 0;
            while (!binary[a]) {
                a--;
                c1++;
            }
            while (!binary[b]) {
                b++;
                c2++;
            }
            ans[i] = Math.min(c1, c2);
        }
        return ans;
    }
}
```