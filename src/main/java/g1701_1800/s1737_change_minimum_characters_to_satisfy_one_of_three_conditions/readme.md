## 1737\. Change Minimum Characters to Satisfy One of Three Conditions

Medium

You are given two strings `a` and `b` that consist of lowercase letters. In one operation, you can change any character in `a` or `b` to **any lowercase letter**.

Your goal is to satisfy **one** of the following three conditions:

*   **Every** letter in `a` is **strictly less** than **every** letter in `b` in the alphabet.
*   **Every** letter in `b` is **strictly less** than **every** letter in `a` in the alphabet.
*   **Both** `a` and `b` consist of **only one** distinct letter.

Return _the **minimum** number of operations needed to achieve your goal._

**Example 1:**

**Input:** a = "aba", b = "caa"

**Output:** 2

**Explanation:** Consider the best way to make each condition true: 

1) Change b to "ccc" in 2 operations, then every letter in a is less than every letter in b. 

2) Change a to "bbb" and b to "aaa" in 3 operations, then every letter in b is less than every letter in a. 

3) Change a to "aaa" and b to "aaa" in 2 operations, then a and b consist of one distinct letter. The best way was done in 2 operations (either condition 1 or condition 3).

**Example 2:**

**Input:** a = "dabadd", b = "cda"

**Output:** 3

**Explanation:** The best way is to make condition 1 true by changing b to "eee".

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>5</sup></code>
*   `a` and `b` consist only of lowercase letters.

## Solution

```java
public class Solution {
    public int minCharacters(String s1, String s2) {
        int[] a = new int[26];
        int[] b = new int[26];
        int l1 = s1.length();
        int l2 = s2.length();
        for (char i : s1.toCharArray()) {
            a[i - 'a']++;
        }
        for (char i : s2.toCharArray()) {
            b[i - 'a']++;
        }
        int min = Integer.MAX_VALUE;
        int t1 = 0;
        int t2 = 0;
        int max = -1;
        for (int i = 0; i < 25; i++) {
            t1 += a[i];
            t2 += b[i];
            min = Math.min(min, Math.min(t1 + l2 - t2, t2 + l1 - t1));
            max = Math.max(max, a[i] + b[i]);
        }
        max = Math.max(max, a[25] + b[25]);
        return Math.min(min, l1 + l2 - max);
    }
}
```