[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 717\. 1-bit and 2-bit Characters

Easy

We have two special characters:

*   The first character can be represented by one bit `0`.
*   The second character can be represented by two bits (`10` or `11`).

Given a binary array `bits` that ends with `0`, return `true` if the last character must be a one-bit character.

**Example 1:**

**Input:** bits = [1,0,0]

**Output:** true

**Explanation:** The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.

**Example 2:**

**Input:** bits = [1,1,1,0]

**Output:** false

**Explanation:** The only way to decode it is two-bit character and two-bit character. So the last character is not one-bit character.

**Constraints:**

*   `1 <= bits.length <= 1000`
*   `bits[i]` is either `0` or `1`.

## Solution

```java
public class Solution {
    public boolean isOneBitCharacter(int[] arr) {
        int i = 0;
        while (i < arr.length - 1) {
            i = (arr[i] == 1) ? i + 2 : i + 1;
        }
        return (i == arr.length - 1);
    }
}
```