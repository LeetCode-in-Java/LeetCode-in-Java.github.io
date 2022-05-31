## 1544\. Make The String Great

Easy

Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:

*   `0 <= i <= s.length - 2`
*   `s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or **vice-versa**.

To make the string good, you can choose **two adjacent** characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return _the string_ after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.

**Example 1:**

**Input:** s = "leEeetcode"

**Output:** "leetcode"

**Explanation:** In the first step, either you choose i = 1 or i = 2, both will result "leEeetcode" to be reduced to "leetcode".

**Example 2:**

**Input:** s = "abBAcC"

**Output:** ""

**Explanation:** We have many possible scenarios, and all lead to the same answer. For example: "abBAcC" --> "aAcC" --> "cC" --> "" "abBAcC" --> "abBA" --> "aA" --> ""

**Example 3:**

**Input:** s = "s"

**Output:** "s"

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` contains only lower and upper case English letters.

## Solution

```java
import java.util.Stack;

@SuppressWarnings("java:S1149")
public class Solution {
    public String makeGood(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (stack.isEmpty()) {
                stack.add(c);
            } else {
                if (Character.toLowerCase(stack.peek()) == Character.toLowerCase(c)) {
                    if ((Character.isLowerCase(stack.peek()) && Character.isUpperCase(c))) {
                        stack.pop();
                    } else if (Character.isUpperCase(stack.peek()) && Character.isLowerCase(c)) {
                        stack.pop();
                    } else {
                        stack.add(c);
                    }
                } else {
                    stack.add(c);
                }
            }
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        return sb.reverse().toString();
    }
}
```