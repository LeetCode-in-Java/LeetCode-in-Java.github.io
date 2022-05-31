## 870\. Advantage Shuffle

Medium

You are given two integer arrays `nums1` and `nums2` both of the same length. The **advantage** of `nums1` with respect to `nums2` is the number of indices `i` for which `nums1[i] > nums2[i]`.

Return _any permutation of_ `nums1` _that maximizes its **advantage** with respect to_ `nums2`.

**Example 1:**

**Input:** nums1 = [2,7,11,15], nums2 = [1,10,4,11]

**Output:** [2,11,7,15]

**Example 2:**

**Input:** nums1 = [12,24,8,32], nums2 = [13,25,32,11]

**Output:** [24,32,8,12]

**Constraints:**

*   <code>1 <= nums1.length <= 10<sup>5</sup></code>
*   `nums2.length == nums1.length`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.HashMap;
import java.util.List;
import java.util.PriorityQueue;

@SuppressWarnings("java:S5413")
public class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        PriorityQueue<Integer> pque = new PriorityQueue<>();
        for (int e : nums1) {
            pque.add(e);
        }
        int l = nums1.length;
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        int[] n = new int[l];
        System.arraycopy(nums2, 0, n, 0, l);
        Arrays.sort(n);
        Deque<Integer> sta = new ArrayDeque<>();
        for (int i = 0; i < l && !pque.isEmpty(); i++) {
            List<Integer> p = map.getOrDefault(n[i], new ArrayList<>());
            int x = pque.poll();
            if (x > n[i]) {
                p.add(x);
                map.put(n[i], p);
            } else {
                while (x <= n[i] && !pque.isEmpty()) {
                    sta.push(x);
                    x = pque.poll();
                }
                if (x > n[i]) {
                    p.add(x);
                    map.put(n[i], p);
                } else {
                    sta.push(x);
                }
            }
        }
        for (int i = 0; i < nums2.length; i++) {
            List<Integer> p = map.getOrDefault(nums2[i], new ArrayList<>());
            int t;
            if (!p.isEmpty()) {
                t = p.get(0);
                p.remove(0);
                map.put(nums2[i], p);
            } else {
                t = sta.pop();
            }
            nums1[i] = t;
        }
        return nums1;
    }
}
```