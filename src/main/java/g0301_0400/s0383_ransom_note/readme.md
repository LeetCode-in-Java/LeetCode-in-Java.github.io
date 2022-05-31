## 383\. Ransom Note

Easy

Given two stings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

**Input:** ransomNote = "a", magazine = "b"

**Output:** false

**Example 2:**

**Input:** ransomNote = "aa", magazine = "ab"

**Output:** false

**Example 3:**

**Input:** ransomNote = "aa", magazine = "aab"

**Output:** true

**Constraints:**

*   <code>1 <= ransomNote.length, magazine.length <= 10<sup>5</sup></code>
*   `ransomNote` and `magazine` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] a = new int[26];
        int n = ransomNote.length();
        for (int i = 0; i < n; i++) {
            a[ransomNote.charAt(i) - 97]++;
        }
        for (int i = 0; i < magazine.length() && n != 0; i++) {
            if (a[magazine.charAt(i) - 97] > 0) {
                n--;
                a[magazine.charAt(i) - 97]--;
            }
        }
        return n == 0;
    }
}
```