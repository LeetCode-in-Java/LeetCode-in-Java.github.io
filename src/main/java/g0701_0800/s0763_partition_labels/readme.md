[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 763\. Partition Labels

Medium

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Return _a list of integers representing the size of these parts_.

**Example 1:**

**Input:** s = "ababcbacadefegdehijhklij"

**Output:** [9,7,8]

**Explanation:**

    The partition is "ababcbaca", "defegde", "hijhklij".
    This is a partition so that each letter appears in at most one part.
    A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts. 

**Example 2:**

**Input:** s = "eccbbbbdec"

**Output:** [10] 

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> partitionLabels(String s) {
        char[] letters = s.toCharArray();
        List<Integer> result = new ArrayList<>();
        int[] position = new int[26];
        for (int i = 0; i < letters.length; i++) {
            position[letters[i] - 'a'] = i;
        }
        int i = 0;
        int prev = -1;
        int max = 0;
        while (i < letters.length) {
            if (position[letters[i] - 'a'] > max) {
                max = position[letters[i] - 'a'];
            }
            if (i == max) {
                result.add(i - prev);
                prev = i;
            }
            i++;
        }
        return result;
    }
}
```

**Time Complexity (Big O Time):**

The program iterates through the input string `s` using a single pass. Within this pass, it performs the following operations:

1. Iterating through `s` to populate the `position` array, which records the last occurrence position of each character. This is a linear operation, O(n), where 'n' is the length of the string `s`.

2. Another linear pass through `s` to partition the string. This pass also has a time complexity of O(n).

Overall, the time complexity of the program is dominated by the linear passes through the string. Therefore, the program's time complexity is O(n), where 'n' is the length of the input string `s`.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional data structures used:

1. The `position` array is used to store the last occurrence position of each character. This array has a fixed size of 26 (assuming lowercase English alphabet letters only), so its space complexity is O(26), which is equivalent to O(1) because it's a constant-size array.

2. The `result` list is used to store the partition sizes, which can have a maximum length equal to the number of distinct characters in the input string `s`. In the worst case, if all characters in the alphabet are unique, this list could have a size of 26 (again assuming lowercase English alphabet letters only). Therefore, the space complexity of the `result` list is O(26), which is equivalent to O(1).

Overall, the space complexity of the program is determined by the constant-size arrays and lists, so it is O(1).

In summary, the provided program has a time complexity of O(n) and a space complexity of O(1), where 'n' is the length of the input string `s`.
