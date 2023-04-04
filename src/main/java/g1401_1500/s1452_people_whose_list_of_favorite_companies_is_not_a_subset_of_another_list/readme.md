[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1452\. People Whose List of Favorite Companies Is Not a Subset of Another List

Medium

Given the array `favoriteCompanies` where `favoriteCompanies[i]` is the list of favorites companies for the `ith` person (**indexed from 0**).

_Return the indices of people whose list of favorite companies is not a **subset** of any other list of favorites companies_. You must return the indices in increasing order.

**Example 1:**

**Input:** favoriteCompanies = \[\["leetcode","google","facebook"],["google","microsoft"],["google","facebook"],["google"],["amazon"]]

**Output:** [0,1,4]

**Explanation:** Person with index=2 has favoriteCompanies[2]=["google","facebook"] which is a subset of favoriteCompanies[0]=["leetcode","google","facebook"] corresponding to the person with index 0. Person with index=3 has favoriteCompanies[3]=["google"] which is a subset of favoriteCompanies[0]=["leetcode","google","facebook"] and favoriteCompanies[1]=["google","microsoft"]. Other lists of favorite companies are not a subset of another list, therefore, the answer is [0,1,4].

**Example 2:**

**Input:** favoriteCompanies = \[\["leetcode","google","facebook"],["leetcode","amazon"],["facebook","google"]]

**Output:** [0,1]

**Explanation:** In this case favoriteCompanies[2]=["facebook","google"] is a subset of favoriteCompanies[0]=["leetcode","google","facebook"], therefore, the answer is [0,1].

**Example 3:**

**Input:** favoriteCompanies = \[\["leetcode"],["google"],["facebook"],["amazon"]]

**Output:** [0,1,2,3]

**Constraints:**

*   `1 <= favoriteCompanies.length <= 100`
*   `1 <= favoriteCompanies[i].length <= 500`
*   `1 <= favoriteCompanies[i][j].length <= 20`
*   All strings in `favoriteCompanies[i]` are **distinct**.
*   All lists of favorite companies are **distinct**, that is, If we sort alphabetically each list then `favoriteCompanies[i] != favoriteCompanies[j].`
*   All strings consist of lowercase English letters only.

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

@SuppressWarnings("java:S1119")
public class Solution {
    public List<Integer> peopleIndexes(List<List<String>> favoriteCompanies) {
        int n = favoriteCompanies.size();
        List<Integer> res = new ArrayList<>();
        List<Set<String>> in = new ArrayList<>();
        for (List<String> list : favoriteCompanies) {
            in.add(new HashSet<>(list));
        }
        outer:
        for (int i = 0; i < n; i++) {
            for (int j : res) {
                if (isSubset(in.get(i), in.get(j))) {
                    continue outer;
                }
            }
            for (int j = i + 1; j < n; j++) {
                if (isSubset(in.get(i), in.get(j))) {
                    continue outer;
                }
            }
            res.add(i);
        }
        return res;
    }

    private boolean isSubset(Set<String> subset, Set<String> set) {
        if (subset.size() >= set.size()) {
            return false;
        }
        return set.containsAll(subset);
    }
}
```