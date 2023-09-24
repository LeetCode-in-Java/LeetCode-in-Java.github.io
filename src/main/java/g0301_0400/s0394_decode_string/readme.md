[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 394\. Decode String

Medium

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there won't be input like `3a` or `2[4]`.

**Example 1:**

**Input:** s = "3[a]2[bc]"

**Output:** "aaabcbc" 

**Example 2:**

**Input:** s = "3[a2[c]]"

**Output:** "accaccacc" 

**Example 3:**

**Input:** s = "2[abc]3[cd]ef"

**Output:** "abcabccdcdcdef" 

**Example 4:**

**Input:** s = "abc3[cd]xyz"

**Output:** "abccdcdcdxyz" 

**Constraints:**

*   `1 <= s.length <= 30`
*   `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
*   `s` is guaranteed to be **a valid** input.
*   All the integers in `s` are in the range `[1, 300]`.

## Solution

```java
public class Solution {
    private int i = 0;

    public String decodeString(String s) {
        int count = 0;
        StringBuilder sb = new StringBuilder();
        while (i < s.length()) {
            char c = s.charAt(i);
            i++;
            if (Character.isLetter(c)) {
                sb.append(c);
            } else if (Character.isDigit(c)) {
                count = count * 10 + Character.getNumericValue(c);
            } else if (c == ']') {
                break;
            } else if (c == '[') {
                // sub problem
                String repeat = decodeString(s);
                while (count > 0) {
                    sb.append(repeat);
                    count--;
                }
            }
        }
        return sb.toString();
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program depends on the length of the input string 's' and the number of times it encounters square brackets ('[' and ']'). Here's a breakdown of the operations:

1. The program iterates through the input string 's' character by character, visiting each character exactly once. This operation takes O(n) time, where 'n' is the length of the input string.

2. When the program encounters a digit character, it starts building a count value by parsing the numeric part of a potential repeat count. This process also takes O(n) time since each digit character is processed once.

3. When the program encounters a square bracket ('['), it recursively calls the `decodeString` function to process the substring inside the brackets. The number of recursive calls depends on the nesting level of brackets and the length of substrings inside the brackets.

4. In each recursive call, the program processes the substring inside the brackets and appends it to the result StringBuilder. The maximum number of iterations inside the recursion is proportional to the count value, which can be at most the length of the input string 's'. Therefore, the recursive part takes O(n) time in total.

Combining all these factors, the overall time complexity of the program is O(n), where 'n' is the length of the input string 's'.

**Space Complexity (Big O Space):**

1. The program uses a StringBuilder `sb` to store the decoded string. The space complexity of the StringBuilder is O(n) because it can grow to accommodate the entire input string 's' in the worst case.

2. The program uses an integer variable `count` to store the repeat count for a substring inside square brackets. This variable requires constant space, O(1).

3. The program uses an integer variable `i` for tracking the current position in the input string. This variable also requires constant space, O(1).

4. The program uses recursion to process substrings inside square brackets. The maximum depth of recursion depends on the nesting level of brackets in the input string. In the worst case, if there are 'k' levels of nesting, the space complexity due to recursion is O(k).

Overall, the dominant factor in the space complexity is the StringBuilder `sb`, which can grow up to O(n) in size. Therefore, the space complexity of the program is O(n).

In summary, the provided program has a time complexity of O(n) and a space complexity of O(n), where 'n' is the length of the input string 's'.
