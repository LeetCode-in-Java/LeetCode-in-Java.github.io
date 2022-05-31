## 2097\. Valid Arrangement of Pairs

Hard

You are given a **0-indexed** 2D integer array `pairs` where <code>pairs[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. An arrangement of `pairs` is **valid** if for every index `i` where `1 <= i < pairs.length`, we have <code>end<sub>i-1</sub> == start<sub>i</sub></code>.

Return _**any** valid arrangement of_ `pairs`.

**Note:** The inputs will be generated such that there exists a valid arrangement of `pairs`.

**Example 1:**

**Input:** pairs = [[5,1],[4,5],[11,9],[9,4]]

**Output:** [[11,9],[9,4],[4,5],[5,1]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 9 == 9 = start<sub>1</sub>

end<sub>1</sub> = 4 == 4 = start<sub>2</sub>

end<sub>2</sub> = 5 == 5 = start<sub>3</sub>

**Example 2:**

**Input:** pairs = [[1,3],[3,2],[2,1]]

**Output:** [[1,3],[3,2],[2,1]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 3 == 3 = start<sub>1</sub>

end<sub>1</sub> = 2 == 2 = start<sub>2</sub>

The arrangements [[2,1],[1,3],[3,2]] and [[3,2],[2,1],[1,3]] are also valid.

**Example 3:**

**Input:** pairs = [[1,2],[1,3],[2,1]]

**Output:** [[1,2],[2,1],[1,3]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 2 == 2 = start<sub>1</sub>

end<sub>1</sub> = 1 == 1 = start<sub>2</sub>

**Constraints:**

*   <code>1 <= pairs.length <= 10<sup>5</sup></code>
*   `pairs[i].length == 2`
*   <code>0 <= start<sub>i</sub>, end<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>start<sub>i</sub> != end<sub>i</sub></code>
*   No two pairs are exactly the same.
*   There **exists** a valid arrangement of `pairs`.

## Solution

```java
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Map;
import java.util.Queue;

public class Solution {
    public int[][] validArrangement(int[][] pairs) {
        HashMap<Integer, int[]> inOutedge = new HashMap<>();
        HashMap<Integer, Queue<Integer>> adList = getAdList(pairs, inOutedge);
        int start = getStart(inOutedge);
        int[][] res = new int[pairs.length][2];
        getRes(start, adList, res, pairs.length - 1);
        return res;
    }

    private HashMap<Integer, Queue<Integer>> getAdList(
            int[][] pairs, HashMap<Integer, int[]> inOutEdge) {
        HashMap<Integer, Queue<Integer>> adList = new HashMap<>();
        for (int[] pair : pairs) {
            int s = pair[0];
            int d = pair[1];
            Queue<Integer> set = adList.computeIfAbsent(s, k -> new LinkedList<>());
            set.add(d);
            int[] sEdgeCnt = inOutEdge.computeIfAbsent(s, k -> new int[2]);
            int[] dEdgeCnt = inOutEdge.computeIfAbsent(d, k -> new int[2]);
            sEdgeCnt[1]++;
            dEdgeCnt[0]++;
        }
        return adList;
    }

    private int getRes(int k, HashMap<Integer, Queue<Integer>> adList, int[][] res, int idx) {
        Queue<Integer> edges = adList.get(k);
        if (edges == null) {
            return idx;
        }
        while (!edges.isEmpty()) {
            int edge = edges.poll();
            idx = getRes(edge, adList, res, idx);
            res[idx--] = new int[] {k, edge};
        }
        return idx;
    }

    private int getStart(HashMap<Integer, int[]> map) {
        int start = -1;
        for (Map.Entry<Integer, int[]> entry : map.entrySet()) {
            int k = entry.getKey();
            int inEdge = entry.getValue()[0];
            int outEdge = entry.getValue()[1];
            start = k;
            if (outEdge - inEdge == 1) {
                return k;
            }
        }
        return start;
    }
}
```