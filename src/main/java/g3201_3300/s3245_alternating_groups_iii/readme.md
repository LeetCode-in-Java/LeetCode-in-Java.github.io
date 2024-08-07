[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3245\. Alternating Groups III

Hard

There are some red and blue tiles arranged circularly. You are given an array of integers `colors` and a 2D integers array `queries`.

The color of tile `i` is represented by `colors[i]`:

*   `colors[i] == 0` means that tile `i` is **red**.
*   `colors[i] == 1` means that tile `i` is **blue**.

An **alternating** group is a contiguous subset of tiles in the circle with **alternating** colors (each tile in the group except the first and last one has a different color from its **adjacent** tiles in the group).

You have to process queries of two types:

*   <code>queries[i] = [1, size<sub>i</sub>]</code>, determine the count of **alternating** groups with size <code>size<sub>i</sub></code>.
*   <code>queries[i] = [2, index<sub>i</sub>, color<sub>i</sub>]</code>, change <code>colors[index<sub>i</sub>]</code> to <code>color<sub>i</sub></code>.

Return an array `answer` containing the results of the queries of the first type _in order_.

**Note** that since `colors` represents a **circle**, the **first** and the **last** tiles are considered to be next to each other.

**Example 1:**

**Input:** colors = [0,1,1,0,1], queries = \[\[2,1,0],[1,4]]

**Output:** [2]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-14-44.png)**

First query:

Change `colors[1]` to 0.

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-20-25.png)

Second query:

Count of the alternating groups with size 4:

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-25-02-2.png)![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-24-12.png)

**Example 2:**

**Input:** colors = [0,0,1,0,1,1], queries = \[\[1,3],[2,3,0],[1,5]]

**Output:** [2,0]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-35-50.png)

First query:

Count of the alternating groups with size 3:

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-37-13.png)![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-36-40.png)

Second query: `colors` will not change.

Third query: There is no alternating group with size 5.

**Constraints:**

*   <code>4 <= colors.length <= 5 * 10<sup>4</sup></code>
*   `0 <= colors[i] <= 1`
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i][0] == 1` or `queries[i][0] == 2`
*   For all `i` that:
    *   `queries[i][0] == 1`: `queries[i].length == 2`, `3 <= queries[i][1] <= colors.length - 1`
    *   `queries[i][0] == 2`: `queries[i].length == 3`, `0 <= queries[i][1] <= colors.length - 1`, `0 <= queries[i][2] <= 1`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private void go(int ind, LST lst, int[] fs, int n, LST ff, int[] c) {
        if (ind > 0) {
            int pre = lst.prev(ind - 1);
            int nex = lst.next(pre + 1);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre != -1 && pre < n && --fs[nex - pre] == 0) {
                ff.unsetPos(nex - pre);
            }
        }
        if (lst.get(ind)) {
            int pre = ind;
            int nex = lst.next(ind + 1);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre != -1 && pre < n && --fs[nex - pre] == 0) {
                ff.unsetPos(nex - pre);
            }
        }
        if (lst.get(ind + 1)) {
            int pre = ind + 1;
            int nex = lst.next(ind + 2);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre != -1 && pre < n && --fs[nex - pre] == 0) {
                ff.unsetPos(nex - pre);
            }
        }
        lst.unsetPos(ind);
        lst.unsetPos(ind + 1);
        c[ind] ^= 1;
        if (ind > 0 && c[ind] != c[ind - 1]) {
            lst.setPos(ind);
        }
        if (ind + 1 < c.length && c[ind + 1] != c[ind]) {
            lst.setPos(ind + 1);
        }
        if (ind > 0) {
            int pre = lst.prev(ind - 1);
            int nex = lst.next(pre + 1);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre != -1 && pre < n && ++fs[nex - pre] == 1) {
                ff.setPos(nex - pre);
            }
        }
        if (lst.get(ind)) {
            int pre = ind;
            int nex = lst.next(ind + 1);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre < n && ++fs[nex - pre] == 1) {
                ff.setPos(nex - pre);
            }
        }
        if (lst.get(ind + 1)) {
            int pre = ind + 1;
            int nex = lst.next(ind + 2);
            if (nex == -1) {
                nex = 2 * n;
            }
            if (pre < n && ++fs[nex - pre] == 1) {
                ff.setPos(nex - pre);
            }
        }
    }

    public List<Integer> numberOfAlternatingGroups(int[] colors, int[][] queries) {
        int n = colors.length;
        int[] c = new int[2 * n];
        for (int i = 0; i < 2 * n; i++) {
            c[i] = colors[i % n] ^ (i % 2 == 0 ? 0 : 1);
        }
        LST lst = new LST(2 * n + 3);
        for (int i = 1; i < 2 * n; i++) {
            if (c[i] != c[i - 1]) {
                lst.setPos(i);
            }
        }
        int[] fs = new int[2 * n + 1];
        LST ff = new LST(2 * n + 1);
        for (int i = 0; i < n; i++) {
            if (lst.get(i)) {
                int ne = lst.next(i + 1);
                if (ne == -1) {
                    ne = 2 * n;
                }
                fs[ne - i]++;
                ff.setPos(ne - i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int[] q : queries) {
            if (q[0] == 1) {
                if (lst.next(0) == -1) {
                    ans.add(n);
                } else {
                    int lans = 0;
                    for (int i = ff.next(q[1]); i != -1; i = ff.next(i + 1)) {
                        lans += (i - q[1] + 1) * fs[i];
                    }
                    if (c[2 * n - 1] != c[0]) {
                        int f = lst.next(0);
                        if (f >= q[1]) {
                            lans += (f - q[1] + 1);
                        }
                    }
                    ans.add(lans);
                }
            } else {
                int ind = q[1];
                int val = q[2];
                if (colors[ind] == val) {
                    continue;
                }
                colors[ind] ^= 1;
                go(ind, lst, fs, n, ff, c);
                go(ind + n, lst, fs, n, ff, c);
            }
        }
        return ans;
    }

    private static class LST {
        private long[][] set;
        private int n;

        public LST(int n) {
            this.n = n;
            int d = 1;
            d = getD(n, d);
            set = new long[d][];
            for (int i = 0, m = n >>> 6; i < d; i++, m >>>= 6) {
                set[i] = new long[m + 1];
            }
        }

        private int getD(int n, int d) {
            int m = n;
            while (m > 1) {
                m >>>= 6;
                d++;
            }
            return d;
        }

        public LST setPos(int pos) {
            if (pos >= 0 && pos < n) {
                for (int i = 0; i < set.length; i++, pos >>>= 6) {
                    set[i][pos >>> 6] |= 1L << pos;
                }
            }
            return this;
        }

        public LST unsetPos(int pos) {
            if (pos >= 0 && pos < n) {
                for (int i = 0;
                        i < set.length && (i == 0 || set[i - 1][pos] == 0L);
                        i++, pos >>>= 6) {
                    set[i][pos >>> 6] &= ~(1L << pos);
                }
            }
            return this;
        }

        public boolean get(int pos) {
            return pos >= 0 && pos < n && set[0][pos >>> 6] << ~pos < 0;
        }

        public int prev(int pos) {
            int i = 0;
            while (i < set.length && pos >= 0) {
                int pre = prev(set[i][pos >>> 6], pos & 63);
                if (pre != -1) {
                    pos = pos >>> 6 << 6 | pre;
                    while (i > 0) {
                        pos = pos << 6 | 63 - Long.numberOfLeadingZeros(set[--i][pos]);
                    }
                    return pos;
                }
                i++;
                pos >>>= 6;
                pos--;
            }
            return -1;
        }

        private int prev(long set, int n) {
            long h = set << ~n;
            if (h == 0L) {
                return -1;
            }
            return -Long.numberOfLeadingZeros(h) + n;
        }

        public int next(int pos) {
            int i = 0;
            while (i < set.length && pos >>> 6 < set[i].length) {
                int nex = next(set[i][pos >>> 6], pos & 63);
                if (nex != -1) {
                    pos = pos >>> 6 << 6 | nex;
                    while (i > 0) {
                        pos = pos << 6 | Long.numberOfTrailingZeros(set[--i][pos]);
                    }
                    return pos;
                }
                i++;
                pos >>>= 6;
                pos++;
            }
            return -1;
        }

        private static int next(long set, int n) {
            long h = set >>> n;
            if (h == 0L) {
                return -1;
            }
            return Long.numberOfTrailingZeros(h) + n;
        }

        @Override
        public String toString() {
            List<Integer> list = new ArrayList<>();
            for (int pos = next(0); pos != -1; pos = next(pos + 1)) {
                list.add(pos);
            }
            return list.toString();
        }
    }
}
```