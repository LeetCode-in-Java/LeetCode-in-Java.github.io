[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1239\. Maximum Length of a Concatenated String with Unique Characters

Medium

You are given an array of strings `arr`. A string `s` is formed by the **concatenation** of a **subsequence** of `arr` that has **unique characters**.

Return _the **maximum** possible length_ of `s`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** arr = ["un","iq","ue"]

**Output:** 4

**Explanation:** All the valid concatenations are: 
- "" 
- "un" 
- "iq" 
- "ue" 
- "uniq" ("un" + "iq") 
- "ique" ("iq" + "ue") 
  
Maximum length is 4.

**Example 2:**

**Input:** arr = ["cha","r","act","ers"]

**Output:** 6

**Explanation:** Possible longest valid concatenations are "chaers" ("cha" + "ers") and "acters" ("act" + "ers").

**Example 3:**

**Input:** arr = ["abcdefghijklmnopqrstuvwxyz"]

**Output:** 26

**Explanation:** The only string in arr has all 26 characters.

**Constraints:**

*   `1 <= arr.length <= 16`
*   `1 <= arr[i].length <= 26`
*   `arr[i]` contains only lowercase English letters.

## Solution

```java
import java.util.List;

public class Solution {
    public int maxLength(List<String> arr) {
        return find(0, 0, arr);
    }

    private int find(int index, int visChar, List<String> arr) {
        if (index == arr.size()) {
            return 0;
        }
        int ans = 0;
        ans = Math.max(ans, find(index + 1, visChar, arr));
        if (checkCurrStringValidOrNot(visChar, arr.get(index))) {
            visChar = updateState(visChar, arr.get(index));
            ans = Math.max(ans, arr.get(index).length() + find(index + 1, visChar, arr));
        }
        return ans;
    }

    private boolean checkCurrStringValidOrNot(int vis, String s) {
        for (char c : s.toCharArray()) {
            if ((vis & (1 << (c - 'a'))) != 0) {
                return false;
            }
            vis |= (1 << (c - 'a'));
        }
        return true;
    }

    private int updateState(int vis, String s) {
        for (char c : s.toCharArray()) {
            vis |= (1 << (c - 'a'));
        }
        return vis;
    }
}
```