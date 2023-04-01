[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 384\. Shuffle an Array

Medium

Given an integer array `nums`, design an algorithm to randomly shuffle the array. All permutations of the array should be **equally likely** as a result of the shuffling.

Implement the `Solution` class:

*   `Solution(int[] nums)` Initializes the object with the integer array `nums`.
*   `int[] reset()` Resets the array to its original configuration and returns it.
*   `int[] shuffle()` Returns a random shuffling of the array.

**Example 1:**

**Input**

    ["Solution", "shuffle", "reset", "shuffle"] 
    [[[1, 2, 3]], [], [], []]

**Output:** 

    [null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

**Explanation:**

    Solution solution = new Solution([1, 2, 3]); 
    solution.shuffle(); // Shuffle the array [1,2,3] and return its result. 
                        // Any permutation of [1,2,3] must be equally likely to be returned. 
                        // Example: return [3, 1, 2] 
    solution.reset();   // Resets the array back to its original configuration [1,2,3]. Return [1, 2, 3] 
    solution.shuffle(); // Returns the random shuffling of array [1,2,3]. Example: return [1, 3, 2]

**Constraints:**

*   `1 <= nums.length <= 200`
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>
*   All the elements of `nums` are **unique**.
*   At most <code>5 * 10<sup>4</sup></code> calls **in total** will be made to `reset` and `shuffle`.

## Solution

```java
import java.util.Random;

@SuppressWarnings("java:S2245")
public class Solution {
    private int[] nums;
    private Random random;

    public Solution(int[] nums) {
        this.nums = nums;
        this.random = new Random();
    }

    // Resets the array to its original configuration and return it.
    public int[] reset() {
        return this.nums;
    }

    // Returns a random shuffling of the array.
    public int[] shuffle() {
        int[] shuffled = this.nums.clone();
        for (int i = nums.length - 1; i > 0; i--) {
            int j = random.nextInt(i + 1);
            swap(shuffled, i, j);
        }
        return shuffled;
    }

    private void swap(int[] shuffled, int i, int j) {
        int tmp = shuffled[i];
        shuffled[i] = shuffled[j];
        shuffled[j] = tmp;
    }
}
```