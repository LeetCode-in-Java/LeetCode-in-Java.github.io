[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3527\. Find the Most Common Response

Medium

You are given a 2D string array `responses` where each `responses[i]` is an array of strings representing survey responses from the <code>i<sup>th</sup></code> day.

Return the **most common** response across all days after removing **duplicate** responses within each `responses[i]`. If there is a tie, return the _lexicographically smallest_ response.

**Example 1:**

**Input:** responses = \[\["good","ok","good","ok"],["ok","bad","good","ok","ok"],["good"],["bad"]]

**Output:** "good"

**Explanation:**

*   After removing duplicates within each list, `responses = \[\["good", "ok"], ["ok", "bad", "good"], ["good"], ["bad"]]`.
*   `"good"` appears 3 times, `"ok"` appears 2 times, and `"bad"` appears 2 times.
*   Return `"good"` because it has the highest frequency.

**Example 2:**

**Input:** responses = \[\["good","ok","good"],["ok","bad"],["bad","notsure"],["great","good"]]

**Output:** "bad"

**Explanation:**

*   After removing duplicates within each list we have `responses = \[\["good", "ok"], ["ok", "bad"], ["bad", "notsure"], ["great", "good"]]`.
*   `"bad"`, `"good"`, and `"ok"` each occur 2 times.
*   The output is `"bad"` because it is the lexicographically smallest amongst the words with the highest frequency.

**Constraints:**

*   `1 <= responses.length <= 1000`
*   `1 <= responses[i].length <= 1000`
*   `1 <= responses[i][j].length <= 10`
*   `responses[i][j]` consists of only lowercase English letters

## Solution

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private boolean compareStrings(String str1, String str2) {
        int n = str1.length();
        int m = str2.length();
        int i = 0;
        int j = 0;
        while (i < n && j < m) {
            if (str1.charAt(i) < str2.charAt(j)) {
                return true;
            } else if (str1.charAt(i) > str2.charAt(j)) {
                return false;
            }
            i++;
            j++;
        }
        return n < m;
    }

    public String findCommonResponse(List<List<String>> responses) {
        int n = responses.size();
        Map<String, int[]> mp = new HashMap<>();
        String ans = responses.get(0).get(0);
        int maxFreq = 0;
        for (int row = 0; row < n; row++) {
            int m = responses.get(row).size();
            for (int col = 0; col < m; col++) {
                String resp = responses.get(row).get(col);
                int[] arr = mp.getOrDefault(resp, new int[] {0, -1});
                if (arr[1] != row) {
                    arr[0]++;
                    arr[1] = row;
                    mp.put(resp, arr);
                }
                if (arr[0] > maxFreq
                        || !ans.equals(resp) && arr[0] == maxFreq && compareStrings(resp, ans)) {
                    ans = resp;
                    maxFreq = arr[0];
                }
            }
        }
        return ans;
    }
}
```