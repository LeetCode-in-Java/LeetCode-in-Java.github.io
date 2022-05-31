## 386\. Lexicographical Numbers

Medium

Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.

You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space.

**Example 1:**

**Input:** n = 13

**Output:** [1,10,11,12,13,2,3,4,5,6,7,8,9]

**Example 2:**

**Input:** n = 2

**Output:** [1,2]

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> lst = new ArrayList<>();
        dfs(lst, n, 0);
        return lst;
    }

    private void dfs(List<Integer> lst, int n, int num) {
        for (int i = 0; i <= 9; i++) {
            int cur = 10 * num + i;
            // get rid of 0
            if (cur == 0) {
                continue;
            }
            // when larger than n, return to the previous level
            if (cur > n) {
                return;
            }
            lst.add(cur);
            dfs(lst, n, cur);
        }
    }
}
```