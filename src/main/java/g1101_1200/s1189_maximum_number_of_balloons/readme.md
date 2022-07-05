[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1189\. Maximum Number of Balloons

Easy

Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.

You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG)**

**Input:** text = "nlaebolko"

**Output:** 1

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG)**

**Input:** text = "loonbalxballpoon"

**Output:** 2

**Example 3:**

**Input:** text = "leetcode"

**Output:** 0

**Constraints:**

*   <code>1 <= text.length <= 10<sup>4</sup></code>
*   `text` consists of lower case English letters only.

## Solution

```java
public class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] counts = new int[26];
        for (char c : text.toCharArray()) {
            counts[c - 'a']++;
        }
        return Math.min(
                counts[0],
                Math.min(
                        counts[1], Math.min(counts[11] / 2, Math.min(counts[14] / 2, counts[13]))));
    }
}
```