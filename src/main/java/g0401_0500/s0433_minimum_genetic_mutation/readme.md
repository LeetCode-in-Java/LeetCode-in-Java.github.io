## 433\. Minimum Genetic Mutation

Medium

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `start` to a gene string `end` where one mutation is defined as one single character changed in the gene string.

*   For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `start` and `end` and the gene bank `bank`, return _the minimum number of mutations needed to mutate from_ `start` _to_ `end`. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

**Example 1:**

**Input:** start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]

**Output:** 1 

**Example 2:**

**Input:** start = "AACCGGTT", end = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]

**Output:** 2 

**Example 3:**

**Input:** start = "AAAAACCC", end = "AACCCCCC", bank = ["AAAACCCC","AAACCCCC","AACCCCCC"]

**Output:** 3 

**Constraints:**

*   `start.length == 8`
*   `end.length == 8`
*   `0 <= bank.length <= 10`
*   `bank[i].length == 8`
*   `start`, `end`, and `bank[i]` consist of only the characters `['A', 'C', 'G', 'T']`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Set;

@SuppressWarnings("java:S3012")
public class Solution {
    private List<String> isInBank(Set<String> set, String cur) {
        List<String> res = new ArrayList<>();
        for (String each : set) {
            int diff = 0;
            for (int i = 0; i < each.length(); i++) {
                if (each.charAt(i) != cur.charAt(i)) {
                    diff++;
                    if (diff > 1) {
                        break;
                    }
                }
            }
            if (diff == 1) {
                res.add(each);
            }
        }
        return res;
    }

    public int minMutation(String start, String end, String[] bank) {
        Set<String> set = new HashSet<>();
        for (String s : bank) {
            set.add(s);
        }
        Queue<String> queue = new LinkedList<>();
        queue.offer(start);
        int step = 0;
        while (!queue.isEmpty()) {
            int curSize = queue.size();
            while (curSize-- > 0) {
                String cur = queue.poll();
                if (cur.equals(end)) {
                    return step;
                }
                for (String next : isInBank(set, cur)) {
                    queue.offer(next);
                    set.remove(next);
                }
            }
            step++;
        }
        return -1;
    }
}
```