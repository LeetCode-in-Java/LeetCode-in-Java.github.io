[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 771\. Jewels and Stones

Easy

You're given strings `jewels` representing the types of stones that are jewels, and `stones` representing the stones you have. Each character in `stones` is a type of stone you have. You want to know how many of the stones you have are also jewels.

Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

**Example 1:**

**Input:** jewels = "aA", stones = "aAAbbbb"

**Output:** 3 

**Example 2:**

**Input:** jewels = "z", stones = "ZZ"

**Output:** 0 

**Constraints:**

*   `1 <= jewels.length, stones.length <= 50`
*   `jewels` and `stones` consist of only English letters.
*   All the characters of `jewels` are **unique**.

## Solution

```java
public class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        int[] x = new int[60];
        int count = 0;
        int len = jewels.length();
        int len2 = stones.length();
        for (int i = 0; i < len; i++) {
            x[jewels.charAt(i) - 65]++;
        }
        for (int i = 0; i < len2; i++) {
            if (x[stones.charAt(i) - 65] == 1) {
                count++;
            }
        }
        return count;
    }
}
```