[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2502\. Design Memory Allocator

Medium

You are given an integer `n` representing the size of a **0-indexed** memory array. All memory units are initially free.

You have a memory allocator with the following functionalities:

1.  **Allocate** a block of `size` consecutive free memory units and assign it the id `mID`.
2.  **Free** all memory units with the given id `mID`.

**Note** that:

*   Multiple blocks can be allocated to the same `mID`.
*   You should free all the memory units with `mID`, even if they were allocated in different blocks.

Implement the `Allocator` class:

*   `Allocator(int n)` Initializes an `Allocator` object with a memory array of size `n`.
*   `int allocate(int size, int mID)` Find the **leftmost** block of `size` **consecutive** free memory units and allocate it with the id `mID`. Return the block's first index. If such a block does not exist, return `-1`.
*   `int free(int mID)` Free all memory units with the id `mID`. Return the number of memory units you have freed.

**Example 1:**

**Input** ["Allocator", "allocate", "allocate", "allocate", "free", "allocate", "allocate", "allocate", "free", "allocate", "free"] [[10], [1, 1], [1, 2], [1, 3], [2], [3, 4], [1, 1], [1, 1], [1], [10, 2], [7]]

**Output:** [null, 0, 1, 2, 1, 3, 1, 6, 3, -1, 0]

**Explanation:**

Allocator loc = new Allocator(10); // Initialize a memory array of size 10. All memory units are initially free.

loc.allocate(1, 1); // The leftmost block's first index is 0. The memory array becomes [**1**,\_,\_,\_,\_,\_,\_,\_,\_,\_]. We return 0.

loc.allocate(1, 2); // The leftmost block's first index is 1. The memory array becomes [1,**2**,\_,\_,\_,\_,\_,\_,\_,\_]. We return 1.

loc.allocate(1, 3); // The leftmost block's first index is 2. The memory array becomes [1,2,**3**,\_,\_,\_,\_,\_,\_,\_]. We return 2.

loc.free(2); // Free all memory units with mID 2. The memory array becomes [1,\_, 3,\_,\_,\_,\_,\_,\_,\_]. We return 1 since there is only 1 unit with mID 2.

loc.allocate(3, 4); // The leftmost block's first index is 3. The memory array becomes [1,\_,3,**4**,**4**,**4**,\_,\_,\_,\_]. We return 3.

loc.allocate(1, 1); // The leftmost block's first index is 1. The memory array becomes [1,**1**,3,4,4,4,\_,\_,\_,\_]. We return 1.

loc.allocate(1, 1); // The leftmost block's first index is 6. The memory array becomes [1,1,3,4,4,4,**1**,\_,\_,\_]. We return 6.

loc.free(1); // Free all memory units with mID 1. The memory array becomes [\_,\_,3,4,4,4,\_,\_,\_,\_]. We return 3 since there are 3 units with mID 1.

loc.allocate(10, 2); // We can not find any free block with 10 consecutive free memory units, so we return -1.

loc.free(7); // Free all memory units with mID 7. The memory array remains the same since there is no memory unit with mID 7. We return 0. 

**Constraints:**

*   `1 <= n, size, mID <= 1000`
*   At most `1000` calls will be made to `allocate` and `free`.

## Solution

```java
public class Allocator {
    private final Node root;

    public Allocator(int n) {
        root = new Node(0, n, -1);
    }

    public int allocate(int size, int mID) {
        Node cur = root;
        while (cur != null && (cur.size < size || cur.id != -1)) {
            cur = cur.next;
        }
        // unable to allocate
        if (cur == null) {
            return -1;
        }
        if (cur.size == size) {
            cur.id = mID;
            return cur.ind;
        } else {
            Node n = new Node(cur.ind + size, cur.size - size, -1);
            n.next = cur.next;
            cur.next = n;
            cur.size = size;
            cur.id = mID;
            return cur.ind;
        }
    }

    public int free(int mID) {
        return collapse(root, mID);
    }

    private int collapse(Node cur, int id) {
        // base case
        if (cur == null) {
            return 0;
        }
        // include size if matching id
        int res = cur.id == id ? cur.size : 0;
        // recurse on child
        res += collapse(cur.next, id);
        // unallocate
        if (cur.id == id) {
            cur.id = -1;
        }
        // collapse unallocated adjacent nodes
        while (cur.next != null && cur.next.id == -1 && cur.id == -1) {
            cur.size += cur.next.size;
            cur.next = cur.next.next;
        }
        return res;
    }

    private static class Node {
        int ind;
        Node next;
        int size;
        int id;

        public Node(int i, int sz, int mID) {
            ind = i;
            size = sz;
            id = mID;
        }
    }
}

/*
 * Your Allocator object will be instantiated and called as such:
 * Allocator obj = new Allocator(n);
 * int param_1 = obj.allocate(size,mID);
 * int param_2 = obj.free(mID);
 */
```