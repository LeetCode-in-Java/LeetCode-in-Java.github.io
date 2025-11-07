[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 438\. Find All Anagrams in a String

Medium

Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in **any order**.

**Example 1:**

**Input:** s = "cbaebabacd", p = "abc"

**Output:** [0,6]

**Explanation:**

    The substring with start index = 0 is "cba", which is an anagram of "abc".
    The substring with start index = 6 is "bac", which is an anagram of "abc". 

**Example 2:**

**Input:** s = "abab", p = "ab"

**Output:** [0,1,2]

**Explanation:**

    The substring with start index = 0 is "ab", which is an anagram of "ab".
    The substring with start index = 1 is "ba", which is an anagram of "ab".
    The substring with start index = 2 is "ab", which is an anagram of "ab". 

**Constraints:**

*   <code>1 <= s.length, p.length <= 3 * 10<sup>4</sup></code>
*   `s` and `p` consist of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int[] map = new int[26];
        for (int i = 0; i < p.length(); ++i) {
            map[p.charAt(i) - 'a']++;
        }
        List<Integer> res = new ArrayList<>();
        int i = 0;
        int j = 0;
        while (i < s.length()) {
            int idx = s.charAt(i) - 'a';
            // add the new character
            map[idx]--;
            // if the length is greater than windows length, pop the left charcater in the window
            if (i >= p.length()) {
                map[s.charAt(j++) - 'a']++;
            }
            boolean finish = true;
            for (int k = 0; k < 26; k++) {
                // if it is not an anagram of string p
                if (map[k] != 0) {
                    finish = false;
                    break;
                }
            }
            if (i >= p.length() - 1 && finish) {
                res.add(j);
            }
            i++;
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program is primarily determined by the two loops that iterate through the characters of strings `s` and `p`:

1. The first loop iterates through the characters of string `s`, where `i` goes from 0 to the length of `s`. This loop has a time complexity of O(n), where 'n' is the length of string `s`.

2. The second loop iterates through the characters of string `p`, where `i` goes from 0 to the length of `p`. This loop has a time complexity of O(m), where 'm' is the length of string `p`.

3. Inside the first loop, there's a constant-time operation that updates the `map` array based on characters in string `p`. This operation takes O(1) time.

4. Additionally, there is a loop that iterates through the `map` array, which has a constant length of 26 (since it represents lowercase English letters). This loop takes constant time, O(26) or simply O(1).

Combining these factors, the overall time complexity of the program is O(n + m), where 'n' is the length of string `s`, and 'm' is the length of string `p`. In the worst case, when both strings are of similar lengths, it can be approximated as O(n).

**Space Complexity (Big O Space):**

The space complexity of the program is primarily determined by the following data structures and variables:

1. `map` array of size 26: This array is used to keep track of character frequencies in string `p`. It requires constant space, O(26) or simply O(1).

2. `res` list: The space required for the result list depends on the number of anagrams found, but it's not included in the space complexity analysis.

3. Integer variables `i`, `j`, and `idx`, which require constant space, O(1).

4. Boolean variable `finish`, which requires constant space, O(1).

Therefore, the dominant factor in the space complexity is the `map` array, which takes O(1) space.

In summary, the provided program has a time complexity of O(n + m) and a space complexity of O(1), where 'n' is the length of string `s`, and 'm' is the length of string `p`.
