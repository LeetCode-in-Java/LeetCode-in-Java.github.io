## 2076\. Process Restricted Friend Requests

Hard

You are given an integer `n` indicating the number of people in a network. Each person is labeled from `0` to `n - 1`.

You are also given a **0-indexed** 2D integer array `restrictions`, where <code>restrictions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> means that person <code>x<sub>i</sub></code> and person <code>y<sub>i</sub></code> **cannot** become **friends**, either **directly** or **indirectly** through other people.

Initially, no one is friends with each other. You are given a list of friend requests as a **0-indexed** 2D integer array `requests`, where <code>requests[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> is a friend request between person <code>u<sub>j</sub></code> and person <code>v<sub>j</sub></code>.

A friend request is **successful** if <code>u<sub>j</sub></code> and <code>v<sub>j</sub></code> can be **friends**. Each friend request is processed in the given order (i.e., `requests[j]` occurs before `requests[j + 1]`), and upon a successful request, <code>u<sub>j</sub></code> and <code>v<sub>j</sub></code> **become direct friends** for all future friend requests.

Return _a **boolean array**_ `result`, _where each_ `result[j]` _is_ `true` _if the_ <code>j<sup>th</sup></code> _friend request is **successful** or_ `false` _if it is not_.

**Note:** If <code>u<sub>j</sub></code> and <code>v<sub>j</sub></code> are already direct friends, the request is still **successful**.

**Example 1:**

**Input:** n = 3, restrictions = [[0,1]], requests = [[0,2],[2,1]]

**Output:** [true,false]

**Explanation:**

Request 0: Person 0 and person 2 can be friends, so they become direct friends.

Request 1: Person 2 and person 1 cannot be friends since person 0 and person 1 would be indirect friends (1--2--0). 

**Example 2:**

**Input:** n = 3, restrictions = [[0,1]], requests = [[1,2],[0,2]]

**Output:** [true,false]

**Explanation:**

Request 0: Person 1 and person 2 can be friends, so they become direct friends.

Request 1: Person 0 and person 2 cannot be friends since person 0 and person 1 would be indirect friends (0--2--1). 

**Example 3:**

**Input:** n = 5, restrictions = [[0,1],[1,2],[2,3]], requests = [[0,4],[1,2],[3,1],[3,4]]

**Output:** [true,false,true,false]

**Explanation:**

Request 0: Person 0 and person 4 can be friends, so they become direct friends.

Request 1: Person 1 and person 2 cannot be friends since they are directly restricted.

Request 2: Person 3 and person 1 can be friends, so they become direct friends.

Request 3: Person 3 and person 4 cannot be friends since person 0 and person 1 would be indirect friends (0--4--3--1). 

**Constraints:**

*   `2 <= n <= 1000`
*   `0 <= restrictions.length <= 1000`
*   `restrictions[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= n - 1</code>
*   <code>x<sub>i</sub> != y<sub>i</sub></code>
*   `1 <= requests.length <= 1000`
*   `requests[j].length == 2`
*   <code>0 <= u<sub>j</sub>, v<sub>j</sub> <= n - 1</code>
*   <code>u<sub>j</sub> != v<sub>j</sub></code>

## Solution

```java
public class Solution {
    public boolean[] friendRequests(int n, int[][] restrictions, int[][] requests) {
        // Check for each request whether it can cause conflict or not
        UnionFind uf = new UnionFind(n);
        boolean[] res = new boolean[requests.length];
        for (int i = 0; i < requests.length; i++) {
            int p1 = uf.findParent(requests[i][0]);
            int p2 = uf.findParent(requests[i][1]);
            if (p1 == p2) {
                res[i] = true;
                continue;
            }
            // Check whether the current request will violate any restriction or not
            boolean flag = true;
            for (int[] restrict : restrictions) {
                int r1 = uf.findParent(restrict[0]);
                int r2 = uf.findParent(restrict[1]);
                if ((r1 == p1 && r2 == p2) || (r1 == p2 && r2 == p1)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                res[i] = true;
                // Union
                uf.parent[p1] = p2;
            }
        }
        return res;
    }

    private static class UnionFind {
        int n;
        int[] parent;

        public UnionFind(int n) {
            this.n = n;
            this.parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int findParent(int user) {
            while (parent[user] != user) {
                parent[user] = parent[parent[user]];
                user = parent[user];
            }
            return user;
        }
    }
}
```