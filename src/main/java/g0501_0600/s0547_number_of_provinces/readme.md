## 547\. Number of Provinces

Medium

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the <code>i<sup>th</sup></code> city and the <code>j<sup>th</sup></code> city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return _the total number of **provinces**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

**Input:** isConnected = [[1,1,0],[1,1,0],[0,0,1]]

**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

**Input:** isConnected = [[1,0,0],[0,1,0],[0,0,1]]

**Output:** 3

**Constraints:**

*   `1 <= n <= 200`
*   `n == isConnected.length`
*   `n == isConnected[i].length`
*   `isConnected[i][j]` is `1` or `0`.
*   `isConnected[i][i] == 1`
*   `isConnected[i][j] == isConnected[j][i]`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findCircleNum(int[][] arr) {
        int[] parent = new int[arr.length];
        Arrays.fill(parent, -1);
        int ans = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = i + 1; j < arr[i].length; j++) {
                if (arr[i][j] == 1) {
                    ans += union(i, j, parent);
                }
            }
        }
        return arr.length - ans;
    }

    private int union(int a, int b, int[] arr) {
        int ga = find(a, arr);
        int gb = find(b, arr);
        if (ga != gb) {
            arr[gb] = ga;
            return 1;
        }
        return 0;
    }

    private int find(int a, int[] arr) {
        if (arr[a] == -1) {
            return a;
        }
        arr[a] = find(arr[a], arr);
        return arr[a];
    }
}
```