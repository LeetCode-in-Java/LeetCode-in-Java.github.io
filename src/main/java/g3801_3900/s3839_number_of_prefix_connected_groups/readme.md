[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3839\. Number of Prefix Connected Groups

Medium

You are given an array of strings `words` and an integer `k`.

Two words `a` and `b` at **distinct indices** are **prefix\-connected** if `a[0..k-1] == b[0..k-1]`.

A **connected group** is a set of words such that each pair of words is prefix-connected.

Return the **number of connected groups** that contain **at least** two words, formed from the given words.

**Note:**

*   Words with length less than `k` cannot join any group and are ignored.
*   Duplicate strings are treated as separate words.

**Example 1:**

**Input:** words = ["apple","apply","banana","bandit"], k = 2

**Output:** 2

**Explanation:**

Words sharing the same first `k = 2` letters are grouped together:

*   `words[0] = "apple"` and `words[1] = "apply"` share prefix `"ap"`.
*   `words[2] = "banana"` and `words[3] = "bandit"` share prefix `"ba"`.

Thus, there are 2 connected groups, each containing at least two words.

**Example 2:**

**Input:** words = ["car","cat","cartoon"], k = 3

**Output:** 1

**Explanation:**

Words are evaluated for a prefix of length `k = 3`:

*   `words[0] = "car"` and `words[2] = "cartoon"` share prefix `"car"`.
*   `words[1] = "cat"` does not share a 3-length prefix with any other word.

Thus, there is 1 connected group.

**Example 3:**

**Input:** words = ["bat","dog","dog","doggy","bat"], k = 3

**Output:** 2

**Explanation:**

Words are evaluated for a prefix of length `k = 3`:

*   `words[0] = "bat"` and `words[4] = "bat"` form a group.
*   `words[1] = "dog"`, `words[2] = "dog"` and `words[3] = "doggy"` share prefix `"dog"`.

Thus, there are 2 connected groups, each containing at least two words.

**Constraints:**

*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 100`
*   `1 <= k <= 100`
*   All strings in `words` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public int prefixConnected(String[] words, int k) {
        java.util.HashMap<String, Integer> mpp = new java.util.HashMap<>();
        for (String word : words) {
            if (word.length() >= k) {
                String pref = word.substring(0, k);
                mpp.put(pref, mpp.getOrDefault(pref, 0) + 1);
            }
        }
        int cnt = 0;
        for (int y : mpp.values()) {
            if (y >= 2) {
                cnt++;
            }
        }
        return cnt;
    }
}
```