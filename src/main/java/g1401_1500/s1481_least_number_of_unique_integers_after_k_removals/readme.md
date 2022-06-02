## 1481\. Least Number of Unique Integers after K Removals

Medium

Given an array of integers `arr` and an integer `k`. Find the _least number of unique integers_ after removing **exactly** `k` elements**.**

**Example 1:**

**Input:** arr = [5,5,4], k = 1

**Output:** 1

**Explanation:** Remove the single 4, only 5 is left.

**Example 2:**

**Input:** arr = [4,3,1,1,3,3,2], k = 3

**Output:** 2

**Explanation:** Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.

**Constraints:**

*   `1 <= arr.length <= 10^5`
*   `1 <= arr[i] <= 10^9`
*   `0 <= k <= arr.length`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int a : arr) {
            count.put(a, 1 + count.getOrDefault(a, 0));
        }
        int remaining = count.size();
        int occur = 1;
        int[] occurrenceCount = new int[arr.length + 1];

        for (int v : count.values()) {
            ++occurrenceCount[v];
        }
        while (k > 0) {
            if (k - occur * occurrenceCount[occur] >= 0) {
                k -= occur * occurrenceCount[occur];
                remaining -= occurrenceCount[occur++];
            } else {
                return remaining - k / occur;
            }
        }
        return remaining;
    }
}
```