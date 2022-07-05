[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1497\. Check If Array Pairs Are Divisible by k

Medium

Given an array of integers `arr` of even length `n` and an integer `k`.

We want to divide the array into exactly `n / 2` pairs such that the sum of each pair is divisible by `k`.

Return `true` _If you can find a way to do that or_ `false` _otherwise_.

**Example 1:**

**Input:** arr = [1,2,3,4,5,10,6,7,8,9], k = 5

**Output:** true

**Explanation:** Pairs are (1,9),(2,8),(3,7),(4,6) and (5,10).

**Example 2:**

**Input:** arr = [1,2,3,4,5,6], k = 7

**Output:** true

**Explanation:** Pairs are (1,6),(2,5) and(3,4).

**Example 3:**

**Input:** arr = [1,2,3,4,5,6], k = 10

**Output:** false

**Explanation:** You can try all possible pairs to see that there is no way to divide arr into 3 pairs each with sum divisible by 10.

**Constraints:**

*   `arr.length == n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `n` is even.
*   <code>-10<sup>9</sup> <= arr[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public boolean canArrange(int[] arr, int k) {
        int[] freq = new int[k];
        for (int num : arr) {
            freq[Math.abs((num % k) + k) % k]++;
        }
        if (freq[0] % 2 != 0) {
            return false;
        }
        for (int i = 1; i <= k / 2; i++) {
            if (i == k - i && freq[i] % 2 != 0) {
                return false;
            }
            if (freq[i] != freq[k - i]) {
                return false;
            }
        }
        return true;
    }
}
```