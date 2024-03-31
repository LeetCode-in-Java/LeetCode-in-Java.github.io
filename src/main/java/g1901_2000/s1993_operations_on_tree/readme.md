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
import java.util.List;

@SuppressWarnings("unchecked")
public class LockingTree {
    private List<Integer>[] graph;
    private boolean[] locked;
    private int[] parent;
    private int[] users;
    private int[] control;

    public LockingTree(int[] parent) {
        int n = parent.length;
        this.parent = parent;
        graph = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int i = 1; i < n; i++) {
            graph[parent[i]].add(i);
        }
        locked = new boolean[n];
        users = new int[n];
        control = new int[n];
    }

    private void setLock(int id, int user) {
        locked[id] = true;
        users[id] = user;
    }

    private void subNodeUnlock(int id) {
        for (int child : graph[id]) {
            locked[child] = false;
            if (control[child] <= 0) {
                continue;
            }
            control[child] = 0;
            subNodeUnlock(child);
        }
    }

    public boolean lock(int id, int user) {
        if (locked[id]) {
            return false;
        }
        setLock(id, user);
        if (control[id] == 0) {
            int node = parent[id];
            while (node != -1) {
                control[node]++;
                if (locked[node] || control[node] > 1) {
                    break;
                }
                node = parent[node];
            }
        }
        return true;
    }

    public boolean unlock(int id, int user) {
        if (!locked[id] || users[id] != user) {
            return false;
        }
        locked[id] = false;
        if (control[id] == 0) {
            int node = parent[id];
            while (node != -1) {
                control[node]--;
                if (locked[node] || control[node] >= 1) {
                    break;
                }
                node = parent[node];
            }
        }
        return true;
    }

    public boolean upgrade(int id, int user) {
        if (locked[id] || control[id] == 0) {
            return false;
        }
        int cur = parent[id];
        while (cur != -1) {
            if (locked[cur]) {
                return false;
            }
            cur = parent[cur];
        }
        setLock(id, user);
        if (control[id] > 0) {
            control[id] = 0;
            subNodeUnlock(id);
        }
        return true;
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