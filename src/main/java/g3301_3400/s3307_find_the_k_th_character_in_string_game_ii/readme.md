[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3307\. Find the K-th Character in String Game II

Hard

Alice and Bob are playing a game. Initially, Alice has a string `word = "a"`.

You are given a **positive** integer `k`. You are also given an integer array `operations`, where `operations[i]` represents the **type** of the <code>i<sup>th</sup></code> operation.

Now Bob will ask Alice to perform **all** operations in sequence:

*   If `operations[i] == 0`, **append** a copy of `word` to itself.
*   If `operations[i] == 1`, generate a new string by **changing** each character in `word` to its **next** character in the English alphabet, and **append** it to the _original_ `word`. For example, performing the operation on `"c"` generates `"cd"` and performing the operation on `"zb"` generates `"zbac"`.

Return the value of the <code>k<sup>th</sup></code> character in `word` after performing all the operations.

**Note** that the character `'z'` can be changed to `'a'` in the second type of operation.

**Example 1:**

**Input:** k = 5, operations = [0,0,0]

**Output:** "a"

**Explanation:**

Initially, `word == "a"`. Alice performs the three operations as follows:

*   Appends `"a"` to `"a"`, `word` becomes `"aa"`.
*   Appends `"aa"` to `"aa"`, `word` becomes `"aaaa"`.
*   Appends `"aaaa"` to `"aaaa"`, `word` becomes `"aaaaaaaa"`.

**Example 2:**

**Input:** k = 10, operations = [0,1,0,1]

**Output:** "b"

**Explanation:**

Initially, `word == "a"`. Alice performs the four operations as follows:

*   Appends `"a"` to `"a"`, `word` becomes `"aa"`.
*   Appends `"bb"` to `"aa"`, `word` becomes `"aabb"`.
*   Appends `"aabb"` to `"aabb"`, `word` becomes `"aabbaabb"`.
*   Appends `"bbccbbcc"` to `"aabbaabb"`, `word` becomes `"aabbaabbbbccbbcc"`.

**Constraints:**

*   <code>1 <= k <= 10<sup>14</sup></code>
*   `1 <= operations.length <= 100`
*   `operations[i]` is either 0 or 1.
*   The input is generated such that `word` has **at least** `k` characters after all operations.

## Solution

```java
public class Solution {
    public char kthCharacter(long k, int[] operations) {
        if (k == 1) {
            return 'a';
        }
        long len = 1;
        long newK = -1;
        int operation = -1;
        for (int ope : operations) {
            len *= 2;
            if (len >= k) {
                operation = ope;
                newK = k - len / 2;
                break;
            }
        }
        char ch = kthCharacter(newK, operations);
        if (operation == 0) {
            return ch;
        }
        return ch == 'z' ? 'a' : (char) (ch + 1);
    }
}
```