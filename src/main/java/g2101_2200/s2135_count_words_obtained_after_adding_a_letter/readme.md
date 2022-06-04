## 2135\. Count Words Obtained After Adding a Letter

Medium

You are given two **0-indexed** arrays of strings `startWords` and `targetWords`. Each string consists of **lowercase English letters** only.

For each string in `targetWords`, check if it is possible to choose a string from `startWords` and perform a **conversion operation** on it to be equal to that from `targetWords`.

The **conversion operation** is described in the following two steps:

1.  **Append** any lowercase letter that is **not present** in the string to its end.
    *   For example, if the string is `"abc"`, the letters `'d'`, `'e'`, or `'y'` can be added to it, but not `'a'`. If `'d'` is added, the resulting string will be `"abcd"`.
2.  **Rearrange** the letters of the new string in **any** arbitrary order.
    *   For example, `"abcd"` can be rearranged to `"acbd"`, `"bacd"`, `"cbda"`, and so on. Note that it can also be rearranged to `"abcd"` itself.

Return _the **number of strings** in_ `targetWords` _that can be obtained by performing the operations on **any** string of_ `startWords`.

**Note** that you will only be verifying if the string in `targetWords` can be obtained from a string in `startWords` by performing the operations. The strings in `startWords` **do not** actually change during this process.

**Example 1:**

**Input:** startWords = ["ant","act","tack"], targetWords = ["tack","act","acti"]

**Output:** 2

**Explanation:** 

- In order to form targetWords[0] = "tack", we use startWords[1] = "act", append 'k' to it, and rearrange "actk" to "tack". 

- There is no string in startWords that can be used to obtain targetWords[1] = "act". 
  
Note that "act" does exist in startWords, but we **must** append one letter to the string before rearranging it. 

- In order to form targetWords[2] = "acti", we use startWords[1] = "act", append 'i' to it, and rearrange "acti" to "acti" itself.

**Example 2:**

**Input:** startWords = ["ab","a"], targetWords = ["abc","abcd"]

**Output:** 1

**Explanation:** 

- In order to form targetWords[0] = "abc", we use startWords[0] = "ab", add 'c' to it, and rearrange it to "abc". 

- There is no string in startWords that can be used to obtain targetWords[1] = "abcd".

**Constraints:**

*   <code>1 <= startWords.length, targetWords.length <= 5 * 10<sup>4</sup></code>
*   `1 <= startWords[i].length, targetWords[j].length <= 26`
*   Each string of `startWords` and `targetWords` consists of lowercase English letters only.
*   No letter occurs more than once in any string of `startWords` or `targetWords`.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    private Set<Integer> set;

    private void preprocess(String[] words) {
        set = new HashSet<>();
        for (String word : words) {
            int bitMap = getBitMap(word);
            set.add(bitMap);
        }
    }

    private boolean matches(int bitMap) {
        return set.contains(bitMap);
    }

    private int getBitMap(String word) {
        int result = 0;
        for (int i = 0; i < word.length(); i++) {
            int position = word.charAt(i) - 'a';
            result |= (1 << position);
        }
        return result;
    }

    private int addBit(int bitMap, char c) {
        int position = c - 'a';
        bitMap |= (1 << position);
        return bitMap;
    }

    private int removeBit(int bitMap, char c) {
        int position = c - 'a';
        bitMap &= ~(1 << position);
        return bitMap;
    }

    public int wordCount(String[] startWords, String[] targetWords) {
        if (startWords == null || startWords.length == 0) {
            return 0;
        }
        if (targetWords == null || targetWords.length == 0) {
            return 0;
        }
        preprocess(startWords);
        int count = 0;
        for (String word : targetWords) {
            int bitMap = getBitMap(word);
            for (int i = 0; i < word.length(); i++) {
                bitMap = removeBit(bitMap, word.charAt(i));
                if (i > 0) {
                    bitMap = addBit(bitMap, word.charAt(i - 1));
                }
                if (matches(bitMap)) {
                    count++;
                    break;
                }
            }
        }
        return count;
    }
}
```