[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1993\. Operations on Tree

Medium

You are given a tree with `n` nodes numbered from `0` to `n - 1` in the form of a parent array `parent` where `parent[i]` is the parent of the <code>i<sup>th</sup></code> node. The root of the tree is node `0`, so `parent[0] = -1` since it has no parent. You want to design a data structure that allows users to lock, unlock, and upgrade nodes in the tree.

The data structure should support the following functions:

*   **Lock: Locks** the given node for the given user and prevents other users from locking the same node. You may only lock a node using this function if the node is unlocked.
*   **Unlock: Unlocks** the given node for the given user. You may only unlock a node using this function if it is currently locked by the same user.
*   **Upgrade: Locks** the given node for the given user and **unlocks** all of its descendants **regardless** of who locked it. You may only upgrade a node if **all** 3 conditions are true:
    *   The node is unlocked,
    *   It has at least one locked descendant (by **any** user), and
    *   It does not have any locked ancestors.

Implement the `LockingTree` class:

*   `LockingTree(int[] parent)` initializes the data structure with the parent array.
*   `lock(int num, int user)` returns `true` if it is possible for the user with id `user` to lock the node `num`, or `false` otherwise. If it is possible, the node `num` will become **locked** by the user with id `user`.
*   `unlock(int num, int user)` returns `true` if it is possible for the user with id `user` to unlock the node `num`, or `false` otherwise. If it is possible, the node `num` will become **unlocked**.
*   `upgrade(int num, int user)` returns `true` if it is possible for the user with id `user` to upgrade the node `num`, or `false` otherwise. If it is possible, the node `num` will be **upgraded**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/29/untitled.png)

**Input**

["LockingTree", "lock", "unlock", "unlock", "lock", "upgrade", "lock"]

[[[-1, 0, 0, 1, 1, 2, 2]], [2, 2], [2, 3], [2, 2], [4, 5], [0, 1], [0, 1]]

**Output:** [null, true, false, true, true, true, false]

**Explanation:**

    LockingTree lockingTree = new LockingTree([-1, 0, 0, 1, 1, 2, 2]);
    lockingTree.lock(2, 2);    // return true because node 2 is unlocked.
                               // Node 2 will now be locked by user 2.
    lockingTree.unlock(2, 3);  // return false because user 3 cannot unlock a node locked by user 2.
    lockingTree.unlock(2, 2);  // return true because node 2 was previously locked by user 2.
                               // Node 2 will now be unlocked.
    lockingTree.lock(4, 5);    // return true because node 4 is unlocked.
                               // Node 4 will now be locked by user 5.
    lockingTree.upgrade(0, 1); // return true because node 0 is unlocked and has at least one locked descendant (node 4).
                               // Node 0 will now be locked by user 1 and node 4 will now be unlocked.
    lockingTree.lock(0, 1);    // return false because node 0 is already locked. 

**Constraints:**

*   `n == parent.length`
*   `2 <= n <= 2000`
*   `0 <= parent[i] <= n - 1` for `i != 0`
*   `parent[0] == -1`
*   `0 <= num <= n - 1`
*   <code>1 <= user <= 10<sup>4</sup></code>
*   `parent` represents a valid tree.
*   At most `2000` calls **in total** will be made to `lock`, `unlock`, and `upgrade`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;

public class LockingTree {
    private int[][] a;
    private HashMap<Integer, List<Integer>> map = new HashMap<>();

    public LockingTree(int[] parent) {
        int l = parent.length;
        a = new int[l][2];
        for (int i = 0; i < l; i++) {
            a[i][0] = parent[i];
            a[i][1] = -1;
            map.putIfAbsent(parent[i], new ArrayList<>());
            List<Integer> p = map.get(parent[i]);
            p.add(i);
            map.put(parent[i], p);
        }
    }

    public boolean lock(int num, int user) {
        int userId = a[num][1];
        if (userId == -1) {
            a[num][1] = user;
            return true;
        }
        return false;
    }

    public boolean unlock(int num, int user) {
        int y = a[num][1];
        if (y == user) {
            a[num][1] = -1;
            return true;
        }
        return false;
    }

    public boolean upgrade(int num, int user) {
        int par = num;
        while (par >= 0) {
            int lop = a[par][1];
            if (lop != -1) {
                return false;
            }
            par = a[par][0];
        }
        int f = 0;
        LinkedList<Integer> que = new LinkedList<>();
        int[] v = new int[a.length];
        que.add(num);
        v[num] = 1;
        while (!que.isEmpty()) {
            int t = que.get(0);
            que.remove(0);
            List<Integer> p = map.getOrDefault(t, new ArrayList<>());
            for (int e : p) {
                if (a[e][1] != -1) {
                    f = 1;
                    a[e][1] = -1;
                }
                if (v[e] == 0) {
                    que.add(e);
                    v[e] = 1;
                }
            }
        }
        if (f == 1) {
            a[num][1] = user;
            return true;
        }
        return false;
    }
}

/*
 * Your LockingTree object will be instantiated and called as such:
 * LockingTree obj = new LockingTree(parent);
 * boolean param_1 = obj.lock(num,user);
 * boolean param_2 = obj.unlock(num,user);
 * boolean param_3 = obj.upgrade(num,user);
 */
```