[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1470\. Shuffle the Array

Easy

Given the array `nums` consisting of `2n` elements in the form <code>[x<sub>1</sub>,x<sub>2</sub>,...,x<sub>n</sub>,y<sub>1</sub>,y<sub>2</sub>,...,y<sub>n</sub>]</code>.

_Return the array in the form_ <code>[x<sub>1</sub>,y<sub>1</sub>,x<sub>2</sub>,y<sub>2</sub>,...,x<sub>n</sub>,y<sub>n</sub>]</code>.

**Example 1:**

**Input:** nums = [2,5,1,3,4,7], n = 3

**Output:** [2,3,5,4,1,7]

**Explanation:** Since x<sub>1</sub>\=2, x<sub>2</sub>\=5, x<sub>3</sub>\=1, y<sub>1</sub>\=3, y<sub>2</sub>\=4, y<sub>3</sub>\=7 then the answer is [2,3,5,4,1,7].

**Example 2:**

**Input:** nums = [1,2,3,4,4,3,2,1], n = 4

**Output:** [1,4,2,3,3,2,4,1]

**Example 3:**

**Input:** nums = [1,1,2,2], n = 2

**Output:** [1,2,1,2]

**Constraints:**

*   `1 <= n <= 500`
*   `nums.length == 2n`
*   `1 <= nums[i] <= 10^3`

## Solution

```java
public class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] result = new int[nums.length];
        int i = 0;
        int j = 0;
        while (i < n && j < 2 * n) {
            result[j] = nums[i];
            result[++j] = nums[i + n];
            i++;
            j++;
        }
        return result;
    }
}
```