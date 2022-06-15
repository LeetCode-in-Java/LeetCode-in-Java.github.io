## 2286\. Booking Concert Tickets in Groups

Hard

A concert hall has `n` rows numbered from `0` to `n - 1`, each with `m` seats, numbered from `0` to `m - 1`. You need to design a ticketing system that can allocate seats in the following cases:

*   If a group of `k` spectators can sit **together** in a row.
*   If **every** member of a group of `k` spectators can get a seat. They may or **may not** sit together.

Note that the spectators are very picky. Hence:

*   They will book seats only if each member of their group can get a seat with row number **less than or equal** to `maxRow`. `maxRow` can **vary** from group to group.
*   In case there are multiple rows to choose from, the row with the **smallest** number is chosen. If there are multiple seats to choose in the same row, the seat with the **smallest** number is chosen.

Implement the `BookMyShow` class:

*   `BookMyShow(int n, int m)` Initializes the object with `n` as number of rows and `m` as number of seats per row.
*   `int[] gather(int k, int maxRow)` Returns an array of length `2` denoting the row and seat number (respectively) of the **first seat** being allocated to the `k` members of the group, who must sit **together**. In other words, it returns the smallest possible `r` and `c` such that all `[c, c + k - 1]` seats are valid and empty in row `r`, and `r <= maxRow`. Returns `[]` in case it is **not possible** to allocate seats to the group.
*   `boolean scatter(int k, int maxRow)` Returns `true` if all `k` members of the group can be allocated seats in rows `0` to `maxRow`, who may or **may not** sit together. If the seats can be allocated, it allocates `k` seats to the group with the **smallest** row numbers, and the smallest possible seat numbers in each row. Otherwise, returns `false`.

**Example 1:**

**Input**

["BookMyShow", "gather", "gather", "scatter", "scatter"]

[[2, 5], [4, 0], [2, 0], [5, 1], [5, 1]]

**Output:** [null, [0, 0], [], true, false]

**Explanation:**

    BookMyShow bms = new BookMyShow(2, 5); // There are 2 rows with 5 seats each
    bms.gather(4, 0); // return [0, 0]
                      // The group books seats [0, 3] of row 0.
    bms.gather(2, 0); // return []
                      // There is only 1 seat left in row 0,
                      // so it is not possible to book 2 consecutive seats.
    bms.scatter(5, 1); // return True
                       // The group books seat 4 of row 0 and seats [0, 3] of row 1.
    bms.scatter(5, 1); // return False
                       // There is only one seat left in the hall. 

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= m, k <= 10<sup>9</sup></code>
*   `0 <= maxRow <= n - 1`
*   At most <code>5 * 10<sup>4</sup></code> calls **in total** will be made to `gather` and `scatter`.

## Solution

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;

public class BookMyShow {
    private final int n;
    private final int m;
    // max number of seats in a row for some segment of the rows
    private final int[] max;
    // total number of seats for some segment of the rows
    private final long[] total;
    // number of rows with zero free places on the left and on the right
    // using this to quickly skip already zero rows
    // actual nodes are placed in [1,this.n], the first and last element only shows there the first
    // non-zero row
    private final int[] numZerosRight;
    private final int[] numZerosLeft;

    public BookMyShow(int n, int m) {
        // make n to be a power of 2 (for simplicity)
        this.n = nextPow2(n);
        this.m = m;
        // segment tree for max number of seats in a row
        this.max = new int[this.n * 2 - 1];
        // total number of seats for a segment of the rows
        this.total = new long[this.n * 2 - 1];
        this.numZerosRight = new int[this.n + 2];
        this.numZerosLeft = new int[this.n + 2];
        // initialize max and total, for max we firstly set values to m
        // segments of size 1 are placed starting from this.n - 1
        Arrays.fill(max, this.n - 1, this.n + n - 1, m);
        Arrays.fill(total, this.n - 1, this.n + n - 1, m);
        // calculate values of max and total for segments based on values of their children
        for (int i = this.n - 2, i1 = i * 2 + 1, i2 = i * 2 + 2; i >= 0; i--, i1 -= 2, i2 -= 2) {
            max[i] = Math.max(max[i1], max[i2]);
            total[i] = total[i1] + total[i2];
        }
    }

    public int[] gather(int k, int maxRow) {
        // find most left row with enough free places
        int mostLeft = mostLeft(0, 0, n, k, maxRow + 1);
        if (mostLeft == -1) {
            return new int[0];
        }
        // get corresponding segment tree node
        int v = n - 1 + mostLeft;
        int[] ans = {mostLeft, m - max[v]};
        // update max and total for this node
        max[v] -= k;
        total[v] -= k;
        // until this is a root of segment tree we update its parent
        while (v != 0) {
            v = (v - 1) / 2;
            max[v] = Math.max(max[v * 2 + 1], max[v * 2 + 2]);
            total[v] = total[v * 2 + 1] + total[v * 2 + 2];
        }
        return ans;
    }

    private int mostLeft(int v, int l, int r, int k, int qr) {
        if (l >= qr || max[v] < k) {
            return -1;
        }
        if (l == r - 1) {
            return l;
        }
        int mid = (l + r) / 2;
        int left = mostLeft(v * 2 + 1, l, mid, k, qr);
        if (left != -1) {
            return left;
        }
        return mostLeft(v * 2 + 2, mid, r, k, qr);
    }

    public boolean scatter(int k, int maxRow) {
        // find total number of free places in the rows [0; maxRow+1)
        long sum = total(0, 0, n, maxRow + 1);
        if (sum < k) {
            return false;
        }
        int i = 0;
        // to don't update parent for both of its children we use a queue
        Deque<Integer> deque = new ArrayDeque<>();
        while (k != 0) {
            i = i + numZerosRight[i] + 1;
            int v = n - 1 + i - 1;
            int spent = Math.min(k, max[v]);
            k -= spent;
            max[v] -= spent;
            total[v] -= spent;
            if (max[v] == 0) {
                // update numZerosRight and numZerosLeft
                numZerosRight[i - numZerosLeft[i] - 1] += numZerosRight[i] + 1;
                numZerosLeft[i + numZerosRight[i] + 1] += numZerosLeft[i] + 1;
            }
            if (v != 0) {
                v = (v - 1) / 2;
                // if we already have the parent node in the queue we don't need to update it
                if (deque.isEmpty() || deque.peekLast() != v) {
                    deque.addLast(v);
                }
            }
        }
        // update max and total
        while (!deque.isEmpty()) {
            int v = deque.pollFirst();
            max[v] = Math.max(max[v * 2 + 1], max[v * 2 + 2]);
            total[v] = total[v * 2 + 1] + total[v * 2 + 2];
            if (v != 0) {
                v = (v - 1) / 2;
                // if we already have the parent node in the queue we don't need to update it
                if (deque.isEmpty() || deque.peekLast() != v) {
                    deque.addLast(v);
                }
            }
        }
        return true;
    }

    // find sum of [ql, qr)
    private long total(int v, int l, int r, int qr) {
        if (l >= qr) {
            return 0;
        }
        if (r <= qr) {
            return total[v];
        }
        int mid = (l + r) / 2;
        return total(v * 2 + 1, l, mid, qr) + total(v * 2 + 2, mid, r, qr);
    }

    private static int nextPow2(int n) {
        if ((n & (n - 1)) == 0) {
            return n;
        }
        return Integer.highestOneBit(n) << 1;
    }
}

/*
 * Your BookMyShow object will be instantiated and called as such:
 * BookMyShow obj = new BookMyShow(n, m);
 * int[] param_1 = obj.gather(k,maxRow);
 * boolean param_2 = obj.scatter(k,maxRow);
 */
```