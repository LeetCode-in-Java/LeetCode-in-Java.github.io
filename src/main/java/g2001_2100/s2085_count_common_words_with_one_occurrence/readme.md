## 2085\. Count Common Words With One Occurrence

Easy

Given two string arrays `words1` and `words2`, return _the number of strings that appear **exactly once** in **each** of the two arrays._

**Example 1:**

**Input:** words1 = ["leetcode","is","amazing","as","is"], words2 = ["amazing","leetcode","is"]

**Output:** 2

**Explanation:**

- "leetcode" appears exactly once in each of the two arrays. We count this string.

- "amazing" appears exactly once in each of the two arrays. We count this string.

- "is" appears in each of the two arrays, but there are 2 occurrences of it in words1. We do not count this string.

- "as" appears once in words1, but does not appear in words2. We do not count this string.

Thus, there are 2 strings that appear exactly once in each of the two arrays. 

**Example 2:**

**Input:** words1 = ["b","bb","bbb"], words2 = ["a","aa","aaa"]

**Output:** 0

**Explanation:** There are no strings that appear in each of the two arrays. 

**Example 3:**

**Input:** words1 = ["a","ab"], words2 = ["a","a","a","ab"]

**Output:** 1

**Explanation:** The only string that appears exactly once in each of the two arrays is "ab". 

**Constraints:**

*   `1 <= words1.length, words2.length <= 1000`
*   `1 <= words1[i].length, words2[j].length <= 30`
*   `words1[i]` and `words2[j]` consists only of lowercase English letters.

## Solution

```java
import java.util.HashMap;
import java.util.Objects;

public class Solution {
    public int countWords(String[] words1, String[] words2) {
        int count = 0;
        HashMap<String, Integer> map = new HashMap<>();
        HashMap<String, Integer> map1 = new HashMap<>();
        // Putting the "words1" array in the map
        for (String s : words1) {
            if (!map.containsKey(s)) {
                map.put(s, 1);
            } else {
                map.put(s, map.get(s) + 1);
            }
        }
        // Putting "words2" array in another map
        for (String s : words2) {
            if (!map1.containsKey(s)) {
                map1.put(s, 1);
            } else {
                map1.put(s, map1.get(s) + 1);
            }
        }
        // traversing through the "words1" array
        for (String s : words1) {
            // Checking if the key is present and is matching in both maps
            // and if the key has appeared just one time in "map1" map
            if (Objects.equals(map.get(s), map1.get(s)) && map1.get(s) == 1) {
                count++;
            }
        }
        return count;
    }
}
```