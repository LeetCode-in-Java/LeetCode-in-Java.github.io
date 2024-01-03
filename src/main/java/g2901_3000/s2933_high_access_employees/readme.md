[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2933\. High-Access Employees

Medium

You are given a 2D **0-indexed** array of strings, `access_times`, with size `n`. For each `i` where `0 <= i <= n - 1`, `access_times[i][0]` represents the name of an employee, and `access_times[i][1]` represents the access time of that employee. All entries in `access_times` are within the same day.

The access time is represented as **four digits** using a **24-hour** time format, for example, `"0800"` or `"2250"`.

An employee is said to be **high-access** if he has accessed the system **three or more** times within a **one-hour period**.

Times with exactly one hour of difference are **not** considered part of the same one-hour period. For example, `"0815"` and `"0915"` are not part of the same one-hour period.

Access times at the start and end of the day are **not** counted within the same one-hour period. For example, `"0005"` and `"2350"` are not part of the same one-hour period.

Return _a list that contains the names of **high-access** employees with any order you want._

**Example 1:**

**Input:** access\_times = \[\["a","0549"],["b","0457"],["a","0532"],["a","0621"],["b","0540"]]

**Output:** ["a"]

**Explanation:** "a" has three access times in the one-hour period of [05:32, 06:31] which are 05:32, 05:49, and 06:21. But "b" does not have more than two access times at all. So the answer is ["a"].

**Example 2:**

**Input:** access\_times = \[\["d","0002"],["c","0808"],["c","0829"],["e","0215"],["d","1508"],["d","1444"],["d","1410"],["c","0809"]]

**Output:** ["c","d"]

**Explanation:** "c" has three access times in the one-hour period of [08:08, 09:07] which are 08:08, 08:09, and 08:29. "d" has also three access times in the one-hour period of [14:10, 15:09] which are 14:10, 14:44, and 15:08. However, "e" has just one access time, so it can not be in the answer and the final answer is ["c","d"].

**Example 3:**

**Input:** access\_times = \[\["cd","1025"],["ab","1025"],["cd","1046"],["cd","1055"],["ab","1124"],["ab","1120"]]

**Output:** ["ab","cd"]

**Explanation:** "ab" has three access times in the one-hour period of [10:25, 11:24] which are 10:25, 11:20, and 11:24. "cd" has also three access times in the one-hour period of [10:25, 11:24] which are 10:25, 10:46, and 10:55. So the answer is ["ab","cd"].

**Constraints:**

*   `1 <= access_times.length <= 100`
*   `access_times[i].length == 2`
*   `1 <= access_times[i][0].length <= 10`
*   `access_times[i][0]` consists only of English small letters.
*   `access_times[i][1].length == 4`
*   `access_times[i][1]` is in 24-hour time format.
*   `access_times[i][1]` consists only of `'0'` to `'9'`.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private boolean isPossible(int a, int b) {
        int hb = b / 100;
        int ha = a / 100;
        int mind = b % 100;
        int mina = a % 100;
        if (hb == 23 && ha == 0) {
            return false;
        }
        if (hb - ha > 1) {
            return false;
        }
        if (hb - ha == 1) {
            mind += 60;
        }
        return mind - mina < 60;
    }

    private boolean isHighAccess(List<Integer> list) {
        if (list.size() < 3) {
            return false;
        }
        int i = 0;
        int j = 1;
        int k = 2;
        while (k < list.size()) {
            int a = list.get(i++);
            int b = list.get(j++);
            int c = list.get(k++);
            if (isPossible(a, c) && isPossible(b, c) && isPossible(a, b)) {
                return true;
            }
        }
        return false;
    }

    private int stringToInt(String str) {
        int i = 1000;
        int val = 0;
        for (char ch : str.toCharArray()) {
            int n = ch - '0';
            val += i * n;
            i = i / 10;
        }
        return val;
    }

    public List<String> findHighAccessEmployees(List<List<String>> accessTimes) {
        HashMap<String, List<Integer>> map = new HashMap<>();
        for (List<String> list : accessTimes) {
            List<Integer> temp = map.getOrDefault(list.get(0), new ArrayList<>());
            int val = stringToInt(list.get(1));
            temp.add(val);
            map.put(list.get(0), temp);
        }
        List<String> ans = new ArrayList<>();
        for (Map.Entry<String, List<Integer>> entry : map.entrySet()) {
            List<Integer> temp = entry.getValue();
            Collections.sort(temp);
            if (isHighAccess(temp)) {
                ans.add(entry.getKey());
            }
        }
        return ans;
    }
}
```