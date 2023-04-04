[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 923\. 3Sum With Multiplicity

Medium

Given an integer array `arr`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [1,1,2,2,3,3,4,4,5,5], target = 8

**Output:** 20

**Explanation:**

    Enumerating by the values (arr[i], arr[j], arr[k]): 
    (1, 2, 5) occurs 8 times; 
    (1, 3, 4) occurs 8 times; 
    (2, 2, 4) occurs 2 times; 
    (2, 3, 3) occurs 2 times.

**Example 2:**

**Input:** arr = [1,1,2,2,2,2], target = 5

**Output:** 12

**Explanation:**

arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times: 

We choose one 1 from [1,1] in 2 ways, 

and two 2s from [2,2,2,2] in 6 ways.

**Constraints:**

*   `3 <= arr.length <= 3000`
*   `0 <= arr[i] <= 100`
*   `0 <= target <= 300`

## Solution

```java
public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private static final int MAX = 100;

    public int threeSumMulti(int[] arr, int target) {
        int answer = 0;
        int[] countRight = new int[MAX + 1];
        for (int num : arr) {
            ++countRight[num];
        }
        int[] countLeft = new int[MAX + 1];
        for (int j = 0; j < arr.length - 1; j++) {
            --countRight[arr[j]];
            int remains = target - arr[j];
            if (remains <= 2 * MAX) {
                for (int v = 0; v <= Math.min(remains, MAX); v++) {
                    if (remains - v <= MAX) {
                        int count = countRight[v] * countLeft[remains - v];
                        if (count > 0) {
                            answer = (answer + count) % MOD;
                        }
                    }
                }
            }
            ++countLeft[arr[j]];
        }
        return answer;
    }
}
```