## 899\. Orderly Queue

Hard

You are given a string `s` and an integer `k`. You can choose one of the first `k` letters of `s` and append it at the end of the string..

Return _the lexicographically smallest string you could have after applying the mentioned step any number of moves_.

**Example 1:**

**Input:** s = "cba", k = 1

**Output:** "acb"

**Explanation:**

In the first move, we move the 1<sup>st</sup> character 'c' to the end, obtaining the string "bac".

In the second move, we move the 1<sup>st</sup> character 'b' to the end, obtaining the final result "acb". 

**Example 2:**

**Input:** s = "baaca", k = 3

**Output:** "aaabc"

**Explanation:**

In the first move, we move the 1<sup>st</sup> character 'b' to the end, obtaining the string "aacab".

In the second move, we move the 3<sup>rd</sup> character 'c' to the end, obtaining the final result "aaabc". 

**Constraints:**

*   `1 <= k <= s.length <= 1000`
*   `s` consist of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;

public class Solution {
    public String orderlyQueue(String s, int k) {
        if (k > 1) {
            char[] ans = s.toCharArray();
            Arrays.sort(ans);
            return String.valueOf(ans);
        }
        char min = 'z';
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            char cc = s.charAt(i);
            if (cc < min) {
                min = cc;
            }
        }
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == min) {
                list.add(i);
            }
        }
        String ans = s;
        for (Integer integer : list) {
            String after = s.substring(0, integer);
            String before = s.substring(integer);
            String f = before + after;
            if (f.compareTo(ans) < 0) {
                ans = f;
            }
        }
        return ans;
    }
}
```