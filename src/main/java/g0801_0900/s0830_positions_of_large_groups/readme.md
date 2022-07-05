[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 830\. Positions of Large Groups

Easy

In a string `s` of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like `s = "abbxxxxzyy"` has the groups `"a"`, `"bb"`, `"xxxx"`, `"z"`, and `"yy"`.

A group is identified by an interval `[start, end]`, where `start` and `end` denote the start and end indices (inclusive) of the group. In the above example, `"xxxx"` has the interval `[3,6]`.

A group is considered **large** if it has 3 or more characters.

Return _the intervals of every **large** group sorted in **increasing order by start index**_.

**Example 1:**

**Input:** s = "abbxxxxzzy"

**Output:** [[3,6]]

**Explanation:** `"xxxx" is the only` large group with start index 3 and end index 6.

**Example 2:**

**Input:** s = "abc"

**Output:** []

**Explanation:** We have groups "a", "b", and "c", none of which are large groups.

**Example 3:**

**Input:** s = "abcdddeeeeaabbbcd"

**Output:** [[3,5],[6,9],[12,14]]

**Explanation:** The large groups are "ddd", "eeee", and "bbb".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` contains lowercase English letters only.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {
        List<List<Integer>> map = new ArrayList<>();
        int i = 0;
        while (i < s.length()) {
            int j = i;
            while (j < s.length() && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            if ((j - 1) - i + 1 >= 3) {
                map.add(Arrays.asList(i, j - 1));
            }
            i = j;
        }
        return map;
    }
}
```