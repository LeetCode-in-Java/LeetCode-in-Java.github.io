[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 131\. Palindrome Partitioning

Medium

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

**Example 1:**

**Input:** s = "aab"

**Output:** [["a","a","b"],["aa","b"]] 

**Example 2:**

**Input:** s = "a"

**Output:** [["a"]] 

**Constraints:**

*   `1 <= s.length <= 16`
*   `s` contains only lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S5413")
public class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        backtracking(res, new ArrayList<>(), s, 0);
        return res;
    }

    private void backtracking(List<List<String>> res, List<String> currArr, String s, int start) {
        if (start == s.length()) {
            res.add(new ArrayList<>(currArr));
        }
        for (int end = start; end < s.length(); end++) {
            if (!isPanlindrome(s, start, end)) {
                continue;
            }
            currArr.add(s.substring(start, end + 1));
            backtracking(res, currArr, s, end + 1);
            currArr.remove(currArr.size() - 1);
        }
    }

    private boolean isPanlindrome(String s, int start, int end) {
        while (start < end && s.charAt(start) == s.charAt(end)) {
            start++;
            end--;
        }
        return start >= end;
    }
}
```

**Time Complexity (Big O Time):**

1. The program starts by initializing an empty result list `res` and calling the `backtracking` function, which is the main recursive function. This initial call is done once.

2. Inside the `backtracking` function, there is a loop that iterates from `start` to `s.length() - 1`. In the worst case, this loop can run from `start = 0` to `start = s.length() - 1`, leading to a linear pass over the characters of the string `s`.

3. Within each iteration of the loop, the program checks if the substring from `start` to `end` is a palindrome using the `isPalindrome` function. This check is done in constant time, as it involves comparing characters.

4. If the substring is a palindrome, the program proceeds to add it to the `currArr` list, a constant-time operation. Then, it makes a recursive call to the `backtracking` function with an updated `start` value.

5. The recursion tree explores all possible combinations of palindrome partitions, and the work done at each level of the recursion tree is proportional to the size of the string `s`.

6. The `backtracking` function can be called multiple times, but the total number of recursive calls is bounded by the number of valid palindrome partitions.

Therefore, the overall time complexity of the program can be analyzed as follows:

- The loop in the `backtracking` function runs in O(N), where N is the length of the input string `s`.
- Within each iteration of the loop, there is a constant amount of work.
- The recursion tree explores all possible palindrome partitions, but the depth of the recursion tree is limited by the length of the string.

As a result, the time complexity of the program is O(N * 2^N), where N is the length of the input string `s`. This is because there can be 2^N possible palindrome partitions, and for each partition, the program performs work linear in the length of `s`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the space required for the result list `res`, the `currArr` list, and the recursion stack.

2. The `res` list stores all valid palindrome partition combinations. In the worst case, there can be 2^N such combinations, each containing a substring of length up to N. Therefore, the space complexity for `res` is O(2^N * N).

3. The `currArr` list stores the current partition being constructed during the recursion. Its size is bounded by the length of the input string `s`, and each recursive call creates a new instance of `currArr`. Therefore, the space complexity for `currArr` is O(N) in the worst case.

4. The recursion stack stores information about the recursive calls. In the worst case, the recursion depth can go up to N, leading to a space complexity of O(N).

5. Overall, the dominant factor in space complexity is the `res` list. Therefore, the total space complexity of the program is O(2^N * N).

In summary, the program has a time complexity of O(N * 2^N) and a space complexity of O(2^N * N), where N is the length of the input string `s`.
