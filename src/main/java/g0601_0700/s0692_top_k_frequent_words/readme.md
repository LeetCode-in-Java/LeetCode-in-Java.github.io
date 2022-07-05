[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 692\. Top K Frequent Words

Medium

Given an array of strings `words` and an integer `k`, return _the_ `k` _most frequent strings_.

Return the answer **sorted** by **the frequency** from highest to lowest. Sort the words with the same frequency by their **lexicographical order**.

**Example 1:**

**Input:** words = ["i","love","leetcode","i","love","coding"], k = 2

**Output:** ["i","love"]

**Explanation:** "i" and "love" are the two most frequent words. Note that "i" comes before "love" due to a lower alphabetical order. 

**Example 2:**

**Input:** words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4

**Output:** ["the","is","sunny","day"]

**Explanation:** "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively. 

**Constraints:**

*   `1 <= words.length <= 500`
*   `1 <= words[i] <= 10`
*   `words[i]` consists of lowercase English letters.
*   `k` is in the range `[1, The number of **unique** words[i]]`

**Follow-up:** Could you solve it in `O(n log(k))` time and `O(n)` extra space?

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.SortedSet;
import java.util.TreeSet;

public class Solution {
    /*
     * O(n) extra space
     * O(nlogk) time
     */
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> map = new HashMap<>();
        for (String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        SortedSet<Map.Entry<String, Integer>> sortedset =
                new TreeSet<>(
                        (e1, e2) -> {
                            if (e1.getValue().intValue() != e2.getValue().intValue()) {
                                return e2.getValue() - e1.getValue();
                            } else {
                                return e1.getKey().compareToIgnoreCase(e2.getKey());
                            }
                        });
        sortedset.addAll(map.entrySet());
        List<String> result = new ArrayList<>();
        Iterator<Map.Entry<String, Integer>> iterator = sortedset.iterator();
        while (iterator.hasNext() && k-- > 0) {
            result.add(iterator.next().getKey());
        }
        return result;
    }
}
```