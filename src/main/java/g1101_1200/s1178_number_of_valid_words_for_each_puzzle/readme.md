## 1178\. Number of Valid Words for Each Puzzle

Hard

With respect to a given `puzzle` string, a `word` is _valid_ if both the following conditions are satisfied:

*   `word` contains the first letter of `puzzle`.
*   For each letter in `word`, that letter is in `puzzle`.
    *   For example, if the puzzle is `"abcdefg"`, then valid words are `"faced"`, `"cabbage"`, and `"baggage"`, while
    *   invalid words are `"beefed"` (does not include `'a'`) and `"based"` (includes `'s'` which is not in the puzzle).

Return _an array_ `answer`_, where_ `answer[i]` _is the number of words in the given word list_ `words` _that is valid with respect to the puzzle_ `puzzles[i]`.

**Example 1:**

**Input:** words = ["aaaa","asas","able","ability","actt","actor","access"], puzzles = ["aboveyz","abrodyz","abslute","absoryz","actresz","gaswxyz"]

**Output:** [1,1,3,2,4,0]

**Explanation:** 

1 valid word for "aboveyz" : "aaaa" 

1 valid word for "abrodyz" : "aaaa" 

3 valid words for "abslute" : "aaaa", "asas", "able" 

2 valid words for "absoryz" : "aaaa", "asas" 

4 valid words for "actresz" : "aaaa", "asas", "actt", "access" 

There are no valid words for "gaswxyz" cause none of the words in the list contains letter 'g'.

**Example 2:**

**Input:** words = ["apple","pleas","please"], puzzles = ["aelwxyz","aelpxyz","aelpsxy","saelpxy","xaelpsy"]

**Output:** [0,1,3,2,0]

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   `4 <= words[i].length <= 50`
*   <code>1 <= puzzles.length <= 10<sup>4</sup></code>
*   `puzzles[i].length == 7`
*   `words[i]` and `puzzles[i]` consist of lowercase English letters.
*   Each `puzzles[i]` does not contain repeated characters.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class Solution {
    public List<Integer> findNumOfValidWords(String[] words, String[] puzzles) {
        List<Integer> ans = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();
        for (String word : words) {
            int wordmask = createMask(word);
            if (map.containsKey(wordmask)) {
                int oldfreq = map.get(wordmask);
                int newfreq = oldfreq + 1;
                map.put(wordmask, newfreq);
            } else {
                map.put(wordmask, 1);
            }
        }
        for (String puzzle : puzzles) {
            int puzzlemask = createMask(puzzle);
            char firstChar = puzzle.charAt(0);
            int first = 1 << (firstChar - 'a');
            int sub = puzzlemask;
            int count = 0;
            while (sub != 0) {
                boolean firstCharPresent = (sub & first) == first;
                boolean wordvalid = map.containsKey(sub);
                if (firstCharPresent && wordvalid) {
                    count += map.get(sub);
                }

                sub = (sub - 1) & puzzlemask;
            }
            ans.add(count);
        }
        return ans;
    }

    private int createMask(String str) {
        int mask = 0;
        for (int i = 0; i < str.length(); i++) {
            int bit = str.charAt(i) - 'a';
            mask = (mask | (1 << bit));
        }
        return mask;
    }
}
```