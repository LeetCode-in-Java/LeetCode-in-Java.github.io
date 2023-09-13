[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 49\. Group Anagrams

Medium

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** strs = ["eat","tea","tan","ate","nat","bat"]

**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]] 

**Example 2:**

**Input:** strs = [""]

**Output:** [[""]] 

**Example 3:**

**Input:** strs = ["a"]

**Output:** [["a"]] 

**Constraints:**

*   <code>1 <= strs.length <= 10<sup>4</sup></code>
*   `0 <= strs[i].length <= 100`
*   `strs[i]` consists of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> hm = new HashMap<>();
        for (String s : strs) {
            char[] ch = s.toCharArray();
            Arrays.sort(ch);
            String temp = new String(ch);
            hm.computeIfAbsent(temp, k -> new ArrayList<>());
            hm.get(temp).add(s);
        }
        return (new ArrayList<>(hm.values()));
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a loop to iterate through each string in the `strs` array. Let's assume there are 'n' strings in the array.

2. For each string, it converts the string into a character array, sorts the character array, and converts it back into a string. This sorting operation takes O(k log k) time, where 'k' is the length of the longest string in the array.

3. After sorting, it checks if the sorted string exists as a key in the `hm` (HashMap). This operation is typically O(1) on average because it involves a hash table lookup.

4. If the key doesn't exist, it creates a new key-value pair in the `hm` HashMap. This is an O(1) operation in the average case.

5. Finally, it adds the original string to the list associated with the sorted string. Adding an element to a list is O(1) on average.

6. Overall, for each of the 'n' strings in the array, the program performs O(k log k) sorting operations and O(1) HashMap operations.

7. Therefore, the overall time complexity of the program is O(n * k log k), where 'n' is the number of strings in the array, and 'k' is the length of the longest string.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the additional data structures used. These include the `hm` HashMap and the lists that store grouped anagrams.

2. The space required for the `hm` HashMap depends on the number of unique sorted strings, which is typically less than or equal to 'n' (the number of input strings).

3. The space required for the lists that store grouped anagrams is also proportional to 'n' because each input string is associated with one of these lists.

4. Therefore, the overall space complexity of the program is O(n), where 'n' is the number of strings in the input array `strs`.

In summary, the time complexity of the provided program is O(n * k log k), where 'n' is the number of strings, and 'k' is the length of the longest string. The space complexity is O(n).
