[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3541\. Find Most Frequent Vowel and Consonant

Easy

You are given a string `s` consisting of lowercase English letters (`'a'` to `'z'`).

Your task is to:

*   Find the vowel (one of `'a'`, `'e'`, `'i'`, `'o'`, or `'u'`) with the **maximum** frequency.
*   Find the consonant (all other letters excluding vowels) with the **maximum** frequency.

Return the sum of the two frequencies.

**Note**: If multiple vowels or consonants have the same maximum frequency, you may choose any one of them. If there are no vowels or no consonants in the string, consider their frequency as 0.

The **frequency** of a letter `x` is the number of times it occurs in the string.

**Example 1:**

**Input:** s = "successes"

**Output:** 6

**Explanation:**

*   The vowels are: `'u'` (frequency 1), `'e'` (frequency 2). The maximum frequency is 2.
*   The consonants are: `'s'` (frequency 4), `'c'` (frequency 2). The maximum frequency is 4.
*   The output is `2 + 4 = 6`.

**Example 2:**

**Input:** s = "aeiaeia"

**Output:** 3

**Explanation:**

*   The vowels are: `'a'` (frequency 3), `'e'` ( frequency 2), `'i'` (frequency 2). The maximum frequency is 3.
*   There are no consonants in `s`. Hence, maximum consonant frequency = 0.
*   The output is `3 + 0 = 3`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters only.

## Solution

```java
public class Solution {
    public int maxFreqSum(String s) {
        int[] freq = new int[26];
        for (char ch : s.toCharArray()) {
            int index = ch - 'a';
            freq[index]++;
        }
        String si = "aeiou";
        int max1 = 0;
        int max2 = 0;
        for (int i = 0; i < 26; i++) {
            char ch = (char) (i + 'a');
            if (si.indexOf(ch) != -1) {
                max1 = Math.max(max1, freq[i]);
            } else {
                max2 = Math.max(max2, freq[i]);
            }
        }
        return max1 + max2;
    }
}
```