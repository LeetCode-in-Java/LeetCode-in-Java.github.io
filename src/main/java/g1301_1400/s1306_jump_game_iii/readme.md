[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1306\. Jump Game III

Medium

Given an array of non-negative integers `arr`, you are initially positioned at `start` index of the array. When you are at index `i`, you can jump to `i + arr[i]` or `i - arr[i]`, check if you can reach to **any** index with value 0.

Notice that you can not jump outside of the array at any time.

**Example 1:**

**Input:** arr = [4,2,3,0,3,1,2], start = 5

**Output:** true

**Explanation:** 

All possible ways to reach at index 3 with value 0 are: 

index 5 -> index 4 -> index 1 -> index 3 

index 5 -> index 6 -> index 4 -> index 1 -> index 3

**Example 2:**

**Input:** arr = [4,2,3,0,3,1,2], start = 0

**Output:** true

**Explanation:** 

One possible way to reach at index 3 with value 0 is: 

index 0 -> index 4 -> index 1 -> index 3

**Example 3:**

**Input:** arr = [3,0,2,1,2], start = 2

**Output:** false

**Explanation:** There is no way to reach at index 1 with value 0.

**Constraints:**

*   <code>1 <= arr.length <= 5 * 10<sup>4</sup></code>
*   `0 <= arr[i] < arr.length`
*   `0 <= start < arr.length`

## Solution

```java
public class Solution {
    private boolean[] dp;
    private boolean found = false;

    public boolean canReach(int[] arr, int start) {
        if (arr[start] == 0) {
            return true;
        }
        dp = new boolean[arr.length];
        dp[start] = true;
        recurse(arr, start);
        return found;
    }

    private void recurse(int[] arr, int index) {
        if (found) {
            return;
        }
        if (index - arr[index] >= 0 && !dp[index - arr[index]]) {
            if (arr[index - arr[index]] == 0) {
                found = true;
                return;
            }
            dp[index - arr[index]] = true;
            recurse(arr, index - arr[index]);
        }
        if (index + arr[index] < arr.length && !dp[index + arr[index]]) {
            if (arr[index + arr[index]] == 0) {
                found = true;
                return;
            }
            dp[index + arr[index]] = true;
            recurse(arr, index + arr[index]);
        }
    }
}
```