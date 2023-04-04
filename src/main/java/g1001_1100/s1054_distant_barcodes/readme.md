[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1054\. Distant Barcodes

Medium

In a warehouse, there is a row of barcodes, where the <code>i<sup>th</sup></code> barcode is `barcodes[i]`.

Rearrange the barcodes so that no two adjacent barcodes are equal. You may return any answer, and it is guaranteed an answer exists.

**Example 1:**

**Input:** barcodes = [1,1,1,2,2,2]

**Output:** [2,1,2,1,2,1]

**Example 2:**

**Input:** barcodes = [1,1,1,1,2,2,3,3]

**Output:** [1,3,1,3,1,2,1,2]

**Constraints:**

*   `1 <= barcodes.length <= 10000`
*   `1 <= barcodes[i] <= 10000`

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i : barcodes) {
            cnt.put(i, cnt.getOrDefault(i, 0) + 1);
        }
        List<Map.Entry<Integer, Integer>> list = new ArrayList<>(cnt.entrySet());
        list.sort(Map.Entry.<Integer, Integer>comparingByValue().reversed());
        int l = barcodes.length;
        int i = 0;
        int[] res = new int[l];
        for (Map.Entry<Integer, Integer> e : list) {
            int time = e.getValue();
            while (time-- > 0) {
                res[i] = e.getKey();
                i += 2;
                if (i >= barcodes.length) {
                    i = 1;
                }
            }
        }
        return res;
    }
}
```