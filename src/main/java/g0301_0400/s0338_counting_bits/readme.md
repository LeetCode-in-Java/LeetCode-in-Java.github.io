[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 338\. Counting Bits

Easy

Given an integer `n`, return _an array_ `ans` _of length_ `n + 1` _such that for each_ `i` (`0 <= i <= n`)_,_ `ans[i]` _is the **number of**_ `1`_**'s** in the binary representation of_ `i`.

**Example 1:**

**Input:** n = 2

**Output:** [0,1,1]

**Explanation:**

    0 --> 0
    1 --> 1
    2 --> 10 

**Example 2:**

**Input:** n = 5

**Output:** [0,1,1,2,1,2]

**Explanation:**

    0 --> 0
    1 --> 1
    2 --> 10
    3 --> 11
    4 --> 100
    5 --> 101 

**Constraints:**

*   <code>0 <= n <= 10<sup>5</sup></code>

**Follow up:**

*   It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
*   Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

## Solution

```java
public class Solution {
    public int[] countBits(int num) {
        int[] result = new int[num + 1];
        int borderPos = 1;
        int incrPos = 1;
        for (int i = 1; i < result.length; i++) {
            // when we reach pow of 2 ,  reset borderPos and incrPos
            if (incrPos == borderPos) {
                result[i] = 1;
                incrPos = 1;
                borderPos = i;
            } else {
                result[i] = 1 + result[incrPos++];
            }
        }
        return result;
    }
}
```

**Time Complexity (Big O Time):**

The program iterates through numbers from 1 to `num` and calculates the number of 1s in each number's binary representation. The key part is that it efficiently reuses previously calculated results for smaller numbers.

1. The outer loop runs `num` times, where `num` is the input integer.
2. The inner loop is based on the value of `borderPos`, which is reset every time `incrPos` reaches a power of 2 (e.g., 1, 2, 4, 8, ...).
3. Inside the inner loop, there is a constant-time operation to update the `result` array.

Therefore, the overall time complexity of the program is O(num), where 'num' is the input integer.

**Space Complexity (Big O Space):**

1. The program uses an integer array `result` of size `num + 1` to store the count of 1s for each number from 0 to `num`. Therefore, the space complexity is O(num).

In summary, the provided program has a time complexity of O(num) and a space complexity of O(num), where 'num' is the input integer.
