[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 70\. Climbing Stairs

Easy

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2

**Output:** 2

**Explanation:** There are two ways to climb to the top. 1. 1 step + 1 step 2. 2 steps 

**Example 2:**

**Input:** n = 3

**Output:** 3

**Explanation:** There are three ways to climb to the top. 1. 1 step + 1 step + 1 step 2. 1 step + 2 steps 3. 2 steps + 1 step 

**Constraints:**

*   `1 <= n <= 45`

## Solution

```java
public class Solution {
    public int climbStairs(int n) {
        if (n < 2) {
            return n;
        }
        int[] cache = new int[n];
        // creating a cache or DP to store the result
        // so that we dont have to iterate multiple times
        // for the same values;

        // for 0 and 1 the result array i.e cache values would be 1 and 2
        // in loop we are just getting ith values i.e 5th step values from
        // i-1 and i-2 which are 4th step and 3rd step values.
        cache[0] = 1;
        cache[1] = 2;
        for (int i = 2; i < n; i++) {
            cache[i] = cache[i - 1] + cache[i - 2];
        }
        return cache[n - 1];
    }
}
```

﻿**Time Complexity (Big O Time):**

1. The program uses dynamic programming with a loop that runs from 2 to `n-1` to fill in the `cache` array.

2. The loop iterates through all steps from 2 to `n-1`, and for each step `i`, it calculates the number of distinct ways by summing the values from the previous two steps (`cache[i-1]` and `cache[i-2]`).

3. Therefore, the loop runs in O(n) time because it iterates through all `n` steps once.

4. The other operations in the program, such as array assignments and comparisons, are constant time operations and do not depend on the input value `n`.

5. Overall, the time complexity of the program is O(n), where 'n' is the number of steps in the staircase.

**Space Complexity (Big O Space):**

1. The program uses an integer array `cache` of size `n` to store the intermediate results of the number of distinct ways to climb each step.

2. Therefore, the space complexity is O(n) because the space usage is directly proportional to the input value `n`.

3. The space complexity is dominated by the `cache` array, and it scales linearly with the size of the input staircase.

In summary, the time complexity of the provided program is O(n), and the space complexity is also O(n), where 'n' is the number of steps in the staircase. The program efficiently calculates the number of distinct ways to climb the staircase using dynamic programming with memoization.
