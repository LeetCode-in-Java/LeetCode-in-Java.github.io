[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2306\. Naming a Company

Hard

You are given an array of strings `ideas` that represents a list of names to be used in the process of naming a company. The process of naming a company is as follows:

1.  Choose 2 **distinct** names from `ideas`, call them <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code>.
2.  Swap the first letters of <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code> with each other.
3.  If **both** of the new names are not found in the original `ideas`, then the name <code>idea<sub>A</sub> idea<sub>B</sub></code> (the **concatenation** of <code>idea<sub>A</sub></code> and <code>idea<sub>B</sub></code>, separated by a space) is a valid company name.
4.  Otherwise, it is not a valid name.

Return _the number of **distinct** valid names for the company_.

**Example 1:**

**Input:** ideas = ["coffee","donuts","time","toffee"]

**Output:** 6

**Explanation:** The following selections are valid:

- ("coffee", "donuts"): The company name created is "doffee conuts".

- ("donuts", "coffee"): The company name created is "conuts doffee".

- ("donuts", "time"): The company name created is "tonuts dime".

- ("donuts", "toffee"): The company name created is "tonuts doffee".

- ("time", "donuts"): The company name created is "dime tonuts".

- ("toffee", "donuts"): The company name created is "doffee tonuts".

Therefore, there are a total of 6 distinct company names.


The following are some examples of invalid selections:

- ("coffee", "time"): The name "toffee" formed after swapping already exists in the original array.

- ("time", "toffee"): Both names are still the same after swapping and exist in the original array.

- ("coffee", "toffee"): Both names formed after swapping already exist in the original array. 

**Example 2:**

**Input:** ideas = ["lack","back"]

**Output:** 0

**Explanation:** There are no valid selections. Therefore, 0 is returned. 

**Constraints:**

*   <code>2 <= ideas.length <= 5 * 10<sup>4</sup></code>
*   `1 <= ideas[i].length <= 10`
*   `ideas[i]` consists of lowercase English letters.
*   All the strings in `ideas` are **unique**.

## Solution

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    private long count(Map<Character, Set<String>> map, char a, char b) {
        if (!map.containsKey(a) || !map.containsKey(b)) {
            return 0;
        }
        long common = 0;
        Set<String> first = map.get(a);
        Set<String> second = map.get(b);
        for (String c : first) {
            if (second.contains(c)) {
                common++;
            }
        }
        long uniqueA = first.size() - common;
        long uniqueB = second.size() - common;
        return uniqueA * uniqueB * 2L;
    }

    public long distinctNames(String[] ideas) {
        long ans = 0;
        Map<Character, Set<String>> map = new HashMap<>();
        for (String idea : ideas) {
            char startChar = idea.charAt(0);
            Set<String> values = map.getOrDefault(startChar, new HashSet<>());
            values.add(idea.substring(1));
            map.put(startChar, values);
        }
        for (int i = 0; i <= 26; i++) {
            for (int j = i + 1; j <= 26; j++) {
                long unique = count(map, (char) (i + 'a'), (char) (j + 'a'));
                ans += unique;
            }
        }
        return ans;
    }
}
```