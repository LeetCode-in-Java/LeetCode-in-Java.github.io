[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 869\. Reordered Power of 2

Medium

You are given an integer `n`. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return `true` _if and only if we can do this so that the resulting number is a power of two_.

**Example 1:**

**Input:** n = 1

**Output:** true

**Example 2:**

**Input:** n = 10

**Output:** false

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

public class Solution {
    public boolean reorderedPowerOf2(int n) {
        int i = 0;
        while (Math.pow(2, i) < (long) n * 10) {
            if (isValid(String.valueOf((int) (Math.pow(2, i++))), String.valueOf(n))) {
                return true;
            }
        }
        return false;
    }

    private boolean isValid(String a, String b) {
        Map<Character, Integer> m = new HashMap<>();
        Map<Character, Integer> mTwo = new HashMap<>();
        for (char c : a.toCharArray()) {
            m.put(c, m.containsKey(c) ? m.get(c) + 1 : 1);
        }
        for (char c : b.toCharArray()) {
            mTwo.put(c, mTwo.containsKey(c) ? mTwo.get(c) + 1 : 1);
        }
        for (Map.Entry<Character, Integer> entry : mTwo.entrySet()) {
            if (!m.containsKey(entry.getKey())
                    || !Objects.equals(entry.getValue(), m.get(entry.getKey()))) {
                return false;
            }
        }
        return a.charAt(0) != '0' && m.size() == mTwo.size();
    }
}
```