[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 75\. Sort Colors

Medium

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

**Input:** nums = [2,0,2,1,1,0]

**Output:** [0,0,1,1,2,2] 

**Example 2:**

**Input:** nums = [2,0,1]

**Output:** [0,1,2] 

**Example 3:**

**Input:** nums = [0]

**Output:** [0] 

**Example 4:**

**Input:** nums = [1]

**Output:** [1] 

**Constraints:**

*   `n == nums.length`
*   `1 <= n <= 300`
*   `nums[i]` is `0`, `1`, or `2`.

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?

## Solution

```java
public class Solution {
    public void sortColors(int[] nums) {
        int zeroes = 0;
        int ones = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                nums[zeroes++] = 0;
            } else if (nums[i] == 1) {
                ones++;
            }
        }
        for (int j = zeroes; j < zeroes + ones; j++) {
            nums[j] = 1;
        }
        for (int k = zeroes + ones; k < nums.length; k++) {
            nums[k] = 2;
        }
    }
}
```

**Time Complexity (Big O Time):**

1. The program iterates through the input array `nums` once to count the number of zeroes and ones. This loop runs in O(n) time, where `n` is the length of the input array.

2. After counting the zeroes and ones, the program performs two additional loops:
   - The first loop fills the first `zeroes` elements of the array with zeroes. This loop runs in O(zeroes) time.
   - The second loop fills the next `ones` elements of the array with ones. This loop runs in O(ones) time.
   - Finally, the program fills the remaining elements with twos, which can be calculated as `n - zeroes - ones`. This operation also runs in O(n) time.

3. Therefore, the overall time complexity of the program is O(n) because the dominant factor is the single iteration through the array.

**Space Complexity (Big O Space):**

1. The program uses a constant amount of extra space for integer variables (`zeroes`, `ones`) and loop counters. This extra space does not depend on the size of the input array and is considered constant.

2. The program does not use any additional data structures or arrays that scale with the size of the input array.

3. Therefore, the space complexity of the program is O(1), or constant space.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1). The program efficiently sorts an array of 0s, 1s, and 2s in a single pass without using additional memory for data structures.
