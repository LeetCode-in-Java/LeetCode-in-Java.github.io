[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 20\. Valid Parentheses

Easy

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.

**Example 1:**

**Input:** s = "()"

**Output:** true 

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true 

**Example 3:**

**Input:** s = "(]"

**Output:** false 

**Example 4:**

**Input:** s = "([)]"

**Output:** false 

**Example 5:**

**Input:** s = "{[]}"

**Output:** true 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of parentheses only `'()[]{}'`.

## Solution

```java
import java.util.Stack;

@SuppressWarnings("java:S1149")
public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '(' || c == '[' || c == '{') {
                stack.push(c);
            } else if (c == ')' && !stack.isEmpty() && stack.peek() == '(') {
                stack.pop();
            } else if (c == '}' && !stack.isEmpty() && stack.peek() == '{') {
                stack.pop();
            } else if (c == ']' && !stack.isEmpty() && stack.peek() == '[') {
                stack.pop();
            } else {
                return false;
            }
        }
        return stack.isEmpty();
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where "n" represents the length of the input string `s`. Here's the breakdown:

1. The program iterates through each character of the input string `s` using a `for` loop. This loop runs for "n" iterations, where "n" is the length of the string.

2. Inside the loop, the program performs constant-time operations for each character:
   - It checks if the character is one of the opening parentheses (`(`, `[`, or `{`) and pushes it onto the stack (constant time).
   - It checks if the character is one of the closing parentheses (`)`, `]`, or `}`) and verifies if it matches the corresponding opening parenthesis on the top of the stack. If they match, it pops the opening parenthesis from the stack (constant time).

3. If any character other than parentheses is encountered, the program returns `false` immediately, so the worst-case scenario is iterating through the entire string.

Since the loop runs for "n" iterations, and all operations inside the loop are constant time, the overall time complexity is O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(n), where "n" represents the length of the input string `s`. Here's why:

1. The program uses a stack (`stack`) to keep track of the opening parentheses encountered while iterating through the string. In the worst case, when the input string contains only opening parentheses, the stack can grow to have a maximum of "n" elements, where "n" is the length of the input string.

2. Other than the stack, the program uses a few integer variables and character variables (`c`) that consume constant space relative to the input size.

Therefore, the dominant factor in terms of space complexity is the stack, which has a space complexity of O(n).
