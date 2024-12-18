[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 434\. Number of Segments in a String

Easy

Given a string `s`, return _the number of segments in the string_.

A **segment** is defined to be a contiguous sequence of **non-space characters**.

**Example 1:**

**Input:** s = "Hello, my name is John"

**Output:** 5

**Explanation:** The five segments are ["Hello,", "my", "name", "is", "John"] 

**Example 2:**

**Input:** s = "Hello"

**Output:** 1 

**Constraints:**

*   `0 <= s.length <= 300`
*   `s` consists of lowercase and uppercase English letters, digits, or one of the following characters `"!@#$%^&*()_+-=',.:"`.
*   The only space character in `s` is `' '`.

## Solution

```java
public class Solution {
    public int countSegments(String s) {
        s = s.trim();
        if (s.isEmpty()) {
            return 0;
        }
        String[] splitted = s.split(" ");
        int result = 0;
        for (String value : splitted) {
            if (!value.isEmpty()) {
                result++;
            }
        }
        return result;
    }
}
```