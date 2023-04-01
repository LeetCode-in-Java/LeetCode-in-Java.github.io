[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1987\. Number of Unique Good Subsequences

Hard

You are given a binary string `binary`. A **subsequence** of `binary` is considered **good** if it is **not empty** and has **no leading zeros** (with the exception of `"0"`).

Find the number of **unique good subsequences** of `binary`.

*   For example, if `binary = "001"`, then all the **good** subsequences are `["0", "0", "1"]`, so the **unique** good subsequences are `"0"` and `"1"`. Note that subsequences `"00"`, `"01"`, and `"001"` are not good because they have leading zeros.

Return _the number of **unique good subsequences** of_ `binary`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** binary = "001"

**Output:** 2

**Explanation:** The good subsequences of binary are ["0", "0", "1"].

The unique good subsequences are "0" and "1". 

**Example 2:**

**Input:** binary = "11"

**Output:** 2

**Explanation:** The good subsequences of binary are ["1", "1", "11"].

The unique good subsequences are "1" and "11".

**Example 3:**

**Input:** binary = "101"

**Output:** 5

**Explanation:** The good subsequences of binary are ["1", "0", "1", "10", "11", "101"].

The unique good subsequences are "0", "1", "10", "11", and "101". 

**Constraints:**

*   <code>1 <= binary.length <= 10<sup>5</sup></code>
*   `binary` consists of only `'0'`s and `'1'`s.

## Solution

```java
public class Solution {
    private static final int MOD = 1000000007;

    public int numberOfUniqueGoodSubsequences(String binary) {
        boolean addZero = false;
        // in the first round we "concat" to the empty binary
        int count = 1;
        int[] countEndsWith = new int[2];
        for (int i = 0; i < binary.length(); i++) {
            char c = binary.charAt(i);
            int cIndex = c - '0';
            // all valid sub-binaries + c at the end => same count
            int endsWithCCount = count;
            if (c == '0') {
                addZero = true;
                // every time c is '0', we concat it to "" and get "0" - we wish to count it only
                // once (done in the end)
                endsWithCCount--;
            }
            // w/out c at the end minus dups (= already end with c)
            count = (count + endsWithCCount - countEndsWith[cIndex]) % MOD;
            // may be negative due to MOD
            count = count < 0 ? count + MOD : count;
            countEndsWith[cIndex] = endsWithCCount;
        }
        // remove the empty binary
        count--;
        // add "0"
        if (addZero) {
            count++;
        }
        return count;
    }
}
```