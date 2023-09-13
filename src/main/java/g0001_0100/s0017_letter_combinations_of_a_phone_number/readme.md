[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 17\. Letter Combinations of a Phone Number

Medium

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

**Input:** digits = "23"

**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"] 

**Example 2:**

**Input:** digits = ""

**Output:** [] 

**Example 3:**

**Input:** digits = "2"

**Output:** ["a","b","c"] 

**Constraints:**

*   `0 <= digits.length <= 4`
*   `digits[i]` is a digit in the range `['2', '9']`.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits.isEmpty()) {
            return Collections.emptyList();
        }
        String[] letters = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        List<String> ans = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        findCombinations(0, digits, letters, sb, ans);
        return ans;
    }

    private void findCombinations(
            int start, String nums, String[] letters, StringBuilder curr, List<String> ans) {
        if (curr.length() == nums.length()) {
            ans.add(curr.toString());
            return;
        }
        for (int i = start; i < nums.length(); i++) {
            int n = Character.getNumericValue(nums.charAt(i));
            for (int j = 0; j < letters[n].length(); j++) {
                char ch = letters[n].charAt(j);
                curr.append(ch);
                findCombinations(i + 1, nums, letters, curr, ans);
                curr.deleteCharAt(curr.length() - 1);
            }
        }
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(4^n), where "n" is the number of digits in the input string `digits`. Here's the breakdown:

1. The program uses a recursive approach to generate all possible letter combinations for the input digits.

2. The `findCombinations` method is called recursively for each digit in the input string `digits`. For each digit, it generates multiple combinations by iterating through the corresponding letters (e.g., "abc" for digit 2).

3. The number of recursive calls made by the program is exponential in nature. For each digit, there are typically 3 or 4 letters associated with it (except for "0" and "1" with no associated letters). Therefore, in the worst case, the number of recursive calls per digit can be considered constant.

4. Since there are "n" digits in the input string, and for each digit, there are a constant number of recursive calls (3 or 4), the overall time complexity is exponential, specifically O(4^n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(n), where "n" is the number of digits in the input string `digits`. Here's why:

1. The program uses a `StringBuilder` (`curr`) to build the current combination of letters. The maximum length of this `StringBuilder` is equal to the number of digits in the input string `digits`.

2. The program uses a list (`ans`) to store the final letter combinations, but the space used by this list is proportional to the number of valid combinations, which is less than or equal to 4^n.

3. Other than the `curr` and `ans` variables, the program uses a few integer variables and arrays (e.g., `start`, `nums`, `letters`) with constant space requirements.

Therefore, the overall space complexity is O(n), primarily due to the `curr` `StringBuilder` and the list `ans`.
