[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2940\. Find Building Where Alice and Bob Can Meet

Hard

You are given a **0-indexed** array `heights` of positive integers, where `heights[i]` represents the height of the <code>i<sup>th</sup></code> building.

If a person is in building `i`, they can move to any other building `j` if and only if `i < j` and `heights[i] < heights[j]`.

You are also given another array `queries` where <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>. On the <code>i<sup>th</sup></code> query, Alice is in building <code>a<sub>i</sub></code> while Bob is in building <code>b<sub>i</sub></code>.

Return _an array_ `ans` _where_ `ans[i]` _is **the index of the leftmost building** where Alice and Bob can meet on the_ <code>i<sup>th</sup></code> _query_. _If Alice and Bob cannot move to a common building on query_ `i`, _set_ `ans[i]` _to_ `-1`.

**Example 1:**

**Input:** heights = [6,4,8,5,2,7], queries = \[\[0,1],[0,3],[2,4],[3,4],[2,2]]

**Output:** [2,5,-1,5,2]

**Explanation:** In the first query, Alice and Bob can move to building 2 since heights[0] < heights[2] and heights[1] < heights[2]. 

In the second query, Alice and Bob can move to building 5 since heights[0] < heights[5] and heights[3] < heights[5]. 

In the third query, Alice cannot meet Bob since Alice cannot move to any other building. 

In the fourth query, Alice and Bob can move to building 5 since heights[3] < heights[5] and heights[4] < heights[5]. 

In the fifth query, Alice and Bob are already in the same building. 

For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet. 

For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Example 2:**

**Input:** heights = [5,3,8,2,6,1,4,6], queries = \[\[0,7],[3,5],[5,2],[3,0],[1,6]]

**Output:** [7,6,-1,4,6]

**Explanation:** In the first query, Alice can directly move to Bob's building since heights[0] < heights[7].

In the second query, Alice and Bob can move to building 6 since heights[3] < heights[6] and heights[5] < heights[6]. 

In the third query, Alice cannot meet Bob since Bob cannot move to any other building. 

In the fourth query, Alice and Bob can move to building 4 since heights[3] < heights[4] and heights[0] < heights[4]. 

In the fifth query, Alice can directly move to Bob's building since heights[1] < heights[6]. 

For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet. 

For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Constraints:**

*   <code>1 <= heights.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= heights[i] <= 10<sup>9</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= heights.length - 1</code>

## Solution

```java
import java.util.LinkedList;

public class Solution {
    public int[] leftmostBuildingQueries(int[] heights, int[][] queries) {
        int n = heights.length;
        int[] gr = new int[n];
        LinkedList<Integer> l = new LinkedList<>();
        l.offer(n - 1);
        gr[n - 1] = -1;
        for (int i = n - 2; i >= 0; i--) {
            while (!l.isEmpty() && heights[i] > heights[l.peek()]) {
                l.pop();
            }
            if (!l.isEmpty()) {
                gr[i] = l.peek();
            } else {
                gr[i] = -1;
            }
            l.push(i);
        }
        int[] ans = new int[queries.length];
        int i = 0;
        for (int[] a : queries) {
            int x = gr[a[0]];
            int y = gr[a[1]];
            if (a[0] == a[1]) {
                ans[i++] = a[0];
            } else if (a[0] < a[1] && heights[a[0]] < heights[a[1]]) {
                ans[i++] = a[1];
            } else if (a[1] < a[0] && heights[a[1]] < heights[a[0]]) {
                ans[i++] = a[0];
            } else if (x == -1 || y == -1) {
                ans[i++] = -1;
            } else {
                int m = Math.max(a[0], a[1]);
                while (m < heights.length
                        && m != -1
                        && (heights[m] <= heights[a[0]] || heights[m] <= heights[a[1]])) {
                    m = gr[m];
                }
                if (m >= heights.length || m == -1) {
                    ans[i++] = -1;
                } else {
                    ans[i++] = m;
                }
            }
        }
        return ans;
    }
}
```