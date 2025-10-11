[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3692\. Majority Frequency Characters

Easy

You are given a string `s` consisting of lowercase English letters.

The **frequency group** for a value `k` is the set of characters that appear exactly `k` times in s.

The **majority frequency group** is the frequency group that contains the largest number of **distinct** characters.

Return a string containing all characters in the majority frequency group, in **any** order. If two or more frequency groups tie for that largest size, pick the group whose frequency `k` is **larger**.

**Example 1:**

**Input:** s = "aaabbbccdddde"

**Output:** "ab"

**Explanation:**

| Frequency (k) | Distinct characters in group | Group size | Majority? |
|---------------|------------------------------|------------|-----------|
| 4             | {d}                          | 1          | No        |
| 3             | {a, b}                       | 2          | **Yes**   |
| 2             | {c}                          | 1          | No        |
| 1             | {e}                          | 1          | No        |

Both characters `'a'` and `'b'` share the same frequency 3, they are in the majority frequency group. `"ba"` is also a valid answer.

**Example 2:**

**Input:** s = "abcd"

**Output:** "abcd"

**Explanation:**

| Frequency (k) | Distinct characters in group | Group size | Majority? |
|---------------|------------------------------|------------|-----------|
| 1             | {a, b, c, d}                 | 4          | **Yes**   |

All characters share the same frequency 1, they are all in the majority frequency group.

**Example 3:**

**Input:** s = "pfpfgi"

**Output:** "fp"

**Explanation:**

| Frequency (k) | Distinct characters in group | Group size | Majority?                        |
|---------------|------------------------------|------------|----------------------------------|
| 2             | {p, f}                       | 2          | **Yes**                          |
| 1             | {g, i}                       | 2          | No (tied size, lower frequency)  |

Both characters `'p'` and `'f'` share the same frequency 2, they are in the majority frequency group. There is a tie in group size with frequency 1, but we pick the higher frequency: 2.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public String majorityFrequencyGroup(String s) {
        int[] cntArray = new int[26];
        for (int i = 0; i < s.length(); i++) {
            cntArray[s.charAt(i) - 'a']++;
        }
        int[] freq = new int[s.length() + 1];
        for (int i = 0; i < 26; i++) {
            if (cntArray[i] > 0) {
                freq[cntArray[i]]++;
            }
        }
        int size = 0;
        int bfreq = 0;
        for (int i = 0; i <= s.length(); i++) {
            int si = freq[i];
            if (si > size || (si == size && i > bfreq)) {
                size = si;
                bfreq = i;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            if (cntArray[i] == bfreq) {
                sb.append((char) (i + 'a'));
            }
        }
        return sb.toString();
    }
}
```