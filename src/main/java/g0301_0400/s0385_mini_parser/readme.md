[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 385\. Mini Parser

Medium

Given a string s represents the serialization of a nested list, implement a parser to deserialize it and return _the deserialized_ `NestedInteger`.

Each element is either an integer or a list whose elements may also be integers or other lists.

**Example 1:**

**Input:** s = "324"

**Output:** 324

**Explanation:** You should return a NestedInteger object which contains a single integer 324.

**Example 2:**

**Input:** s = "[123,[456,[789]]]"

**Output:** [123,[456,[789]]]

**Explanation:** 
Return a NestedInteger object containing a nested list with 2 elements: 
1. An integer containing value 123. 
2. A nested list containing two elements:
   
   i. An integer containing value 456. 
   
    ii. A nested list with one element:
        
    a. An integer containing value 789

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` consists of digits, square brackets `"[]"`, negative sign `'-'`, and commas `','`.
*   `s` is the serialization of valid `NestedInteger`.
*   All the values in the input are in the range <code>[-10<sup>6</sup>, 10<sup>6</sup>]</code>.

## Solution

```java
import com_github_leetcode.NestedInteger;

public class Solution {
    private int i = 0;

    public NestedInteger deserialize(String s) {
        return getAns(s);
    }

    private NestedInteger getAns(String s) {
        if (s.charAt(i) == '[') {
            NestedInteger ni = new NestedInteger();
            i++;
            while (i < s.length() && s.charAt(i) != ']') {
                ni.add(getAns(s));
            }
            i++;
            return ni;
        } else if (s.charAt(i) == ',') {
            i++;
            return getAns(s);
        } else {
            int x = 0;
            int m = 1;
            if (s.charAt(i) == '-') {
                i++;
                m = -1;
            }
            while (i < s.length() && Character.isDigit(s.charAt(i))) {
                x = x * 10 + s.charAt(i++) - '0';
            }
            x *= m;
            return new NestedInteger(x);
        }
    }
}
```