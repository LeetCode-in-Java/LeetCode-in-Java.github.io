[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1974\. Minimum Time to Type Word Using Special Typewriter

Easy

There is a special typewriter with lowercase English letters `'a'` to `'z'` arranged in a **circle** with a **pointer**. A character can **only** be typed if the pointer is pointing to that character. The pointer is **initially** pointing to the character `'a'`.

![](https://assets.leetcode.com/uploads/2021/07/31/chart.jpg)

Each second, you may perform one of the following operations:

*   Move the pointer one character **counterclockwise** or **clockwise**.
*   Type the character the pointer is **currently** on.

Given a string `word`, return the **minimum** number of seconds to type out the characters in `word`.

**Example 1:**

**Input:** word = "abc"

**Output:** 5

**Explanation:** 

The characters are printed as follows: 

- Type the character 'a' in 1 second since the pointer is initially on 'a'. 

- Move the pointer clockwise to 'b' in 1 second. 

- Type the character 'b' in 1 second. 

- Move the pointer clockwise to 'c' in 1 second. 

- Type the character 'c' in 1 second.

**Example 2:**

**Input:** word = "bza"

**Output:** 7

**Explanation:** 

The characters are printed as follows: 

- Move the pointer clockwise to 'b' in 1 second. 

- Type the character 'b' in 1 second. 

- Move the pointer counterclockwise to 'z' in 2 seconds. 

- Type the character 'z' in 1 second. 

- Move the pointer clockwise to 'a' in 1 second. 

- Type the character 'a' in 1 second.

**Example 3:**

**Input:** word = "zjpc"

**Output:** 34

**Explanation:** 

The characters are printed as follows: 

- Move the pointer counterclockwise to 'z' in 1 second. 

- Type the character 'z' in 1 second. 

- Move the pointer clockwise to 'j' in 10 seconds. 

- Type the character 'j' in 1 second. 

- Move the pointer clockwise to 'p' in 6 seconds. 

- Type the character 'p' in 1 second. 

- Move the pointer counterclockwise to 'c' in 13 seconds.

- Type the character 'c' in 1 second.

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public int minTimeToType(String word) {
        int min = 0;
        char curr = 'a';
        for (int i = 0; i < word.length(); i++) {
            int diff = curr - word.charAt(i);
            curr = word.charAt(i);
            min += Math.min(diff + 26, Math.min(Math.abs(diff), 26 - diff));
            min++;
        }
        return min;
    }
}
```