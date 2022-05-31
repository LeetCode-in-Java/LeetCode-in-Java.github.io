## 1636\. Sort Array by Increasing Frequency

Easy

Given an array of integers `nums`, sort the array in **increasing** order based on the frequency of the values. If multiple values have the same frequency, sort them in **decreasing** order.

Return the _sorted array_.

**Example 1:**

**Input:** nums = [1,1,2,2,2,3]

**Output:** [3,1,1,2,2,2]

**Explanation:** '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.

**Example 2:**

**Input:** nums = [2,3,1,3,2]

**Output:** [1,3,3,2,2]

**Explanation:** '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.

**Example 3:**

**Input:** nums = [-1,1,-6,4,5,-6,1,4,1]

**Output:** [5,-1,4,4,-6,-6,1,1,1]

**Constraints:**

*   `1 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;

public class Solution {
    public int[] frequencySort(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }
        TreeMap<Integer, List<Integer>> map = new TreeMap<>();
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            int freq = entry.getValue();
            map.putIfAbsent(freq, new ArrayList<>());
            List<Integer> list = map.get(freq);
            list.add(entry.getKey());
            map.put(freq, list);
        }
        int[] result = new int[nums.length];
        int i = 0;
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            List<Integer> list = entry.getValue();
            list.sort(Collections.reverseOrder());
            int k = entry.getKey();
            for (int j = 0; j < list.size(); j++, k = entry.getKey()) {
                while (k-- > 0) {
                    result[i++] = list.get(j);
                }
            }
        }
        return result;
    }
}
```