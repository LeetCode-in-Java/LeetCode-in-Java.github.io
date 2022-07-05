[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 744\. Find Smallest Letter Greater Than Target

Easy

Given a characters array `letters` that is sorted in **non-decreasing** order and a character `target`, return _the smallest character in the array that is larger than_ `target`.

**Note** that the letters wrap around.

*   For example, if `target == 'z'` and `letters == ['a', 'b']`, the answer is `'a'`.

**Example 1:**

**Input:** letters = ["c","f","j"], target = "a"

**Output:** "c"

**Example 2:**

**Input:** letters = ["c","f","j"], target = "c"

**Output:** "f"

**Example 3:**

**Input:** letters = ["c","f","j"], target = "d"

**Output:** "f"

**Constraints:**

*   <code>2 <= letters.length <= 10<sup>4</sup></code>
*   `letters[i]` is a lowercase English letter.
*   `letters` is sorted in **non-decreasing** order.
*   `letters` contains at least two different characters.
*   `target` is a lowercase English letter.

## Solution

```java
public class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int start = 0;
        int end = letters.length - 1;
        // If target is greater than last element return first element of the array.
        if (letters[letters.length - 1] <= target) {
            return letters[start];
        }
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (letters[mid] <= target) {
                start = mid + 1;
            } else {
                end = mid;
            }
        }
        return letters[start];
    }
}
```