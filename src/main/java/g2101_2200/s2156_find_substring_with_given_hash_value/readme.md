## 2156\. Find Substring With Given Hash Value

Hard

The hash of a **0-indexed** string `s` of length `k`, given integers `p` and `m`, is computed using the following function:

*   <code>hash(s, p, m) = (val(s[0]) * p<sup>0</sup> + val(s[1]) * p<sup>1</sup> + ... + val(s[k-1]) * p<sup>k-1</sup>) mod m</code>.

Where `val(s[i])` represents the index of `s[i]` in the alphabet from `val('a') = 1` to `val('z') = 26`.

You are given a string `s` and the integers `power`, `modulo`, `k`, and `hashValue.` Return `sub`, _the **first** **substring** of_ `s` _of length_ `k` _such that_ `hash(sub, power, modulo) == hashValue`.

The test cases will be generated such that an answer always **exists**.

A **substring** is a contiguous non-empty sequence of characters within a string.

**Example 1:**

**Input:** s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0

**Output:** "ee"

**Explanation:** The hash of "ee" can be computed to be hash("ee", 7, 20) = (5 \* 1 + 5 \* 7) mod 20 = 40 mod 20 = 0. "ee" is the first substring of length 2 with hashValue 0. Hence, we return "ee".

**Example 2:**

**Input:** s = "fbxzaad", power = 31, modulo = 100, k = 3, hashValue = 32

**Output:** "fbx"

**Explanation:** The hash of "fbx" can be computed to be hash("fbx", 31, 100) = (6 \* 1 + 2 \* 31 + 24 \* 31<sup>2</sup>) mod 100 = 23132 mod 100 = 32. 

The hash of "bxz" can be computed to be hash("bxz", 31, 100) = (2 \* 1 + 24 \* 31 + 26 \* 31<sup>2</sup>) mod 100 = 25732 mod 100 = 32. "fbx" is the first substring of length 3 with hashValue 32. Hence, we return "fbx". 

Note that "bxz" also has a hash of 32 but it appears later than "fbx".

**Constraints:**

*   <code>1 <= k <= s.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= power, modulo <= 10<sup>9</sup></code>
*   `0 <= hashValue < modulo`
*   `s` consists of lowercase English letters only.
*   The test cases are generated such that an answer always **exists**.

## Solution

```java
public class Solution {
    public String subStrHash(String s, int power, int modulo, int k, int hashValue) {
        long mul1 = 1;
        int times = k - 1;
        while (times-- > 0) {
            mul1 = mul1 * power % modulo;
        }
        int index = -1;
        long hash = 0;
        int end = s.length() - 1;
        for (int i = s.length() - 1; i >= 0; i--) {
            int val = s.charAt(i) - 96;
            hash = (hash * power % modulo + val) % modulo;
            if (end - i + 1 == k) {
                if (hash == hashValue) {
                    index = i;
                }
                hash = (hash - (s.charAt(end) - 96) * mul1 % modulo + modulo) % modulo;
                end--;
            }
        }
        return s.substring(index, index + k);
    }
}
```