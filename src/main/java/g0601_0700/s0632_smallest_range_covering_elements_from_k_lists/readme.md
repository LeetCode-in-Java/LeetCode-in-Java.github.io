[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 632\. Smallest Range Covering Elements from K Lists

Hard

You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` **or** `a < c` if `b - a == d - c`.

**Example 1:**

**Input:** nums = \[\[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]

**Output:** [20,24]

**Explanation:**

List 1: [4, 10, 15, 24,26], 24 is in range [20,24].

List 2: [0, 9, 12, 20], 20 is in range [20,24].

List 3: [5, 18, 22, 30], 22 is in range [20,24].

**Example 2:**

**Input:** nums = \[\[1,2,3],[1,2,3],[1,2,3]]

**Output:** [1,1]

**Constraints:**

*   `nums.length == k`
*   `1 <= k <= 3500`
*   `1 <= nums[i].length <= 50`
*   <code>-10<sup>5</sup> <= nums[i][j] <= 10<sup>5</sup></code>
*   `nums[i]` is sorted in **non-decreasing** order.

## Solution

```java
import java.util.List;
import java.util.Objects;
import java.util.PriorityQueue;

public class Solution {
    @SuppressWarnings("java:S1210")
    static class Triplet implements Comparable<Triplet> {
        int value;
        int row;
        int idx;

        Triplet(int value, int row, int idx) {
            this.value = value;
            this.row = row;
            this.idx = idx;
        }

        public int compareTo(Triplet obj) {
            return this.value - obj.value;
        }
    }

    public int[] smallestRange(List<List<Integer>> nums) {
        PriorityQueue<Triplet> pq = new PriorityQueue<>();
        int maxInPq = Integer.MIN_VALUE;
        for (int i = 0; i < nums.size(); i++) {
            pq.add(new Triplet(nums.get(i).get(0), i, 0));
            if (maxInPq < nums.get(i).get(0)) {
                maxInPq = nums.get(i).get(0);
            }
        }
        int rangeSize = maxInPq - Objects.requireNonNull(pq.peek()).value + 1;
        int rangeLeft = Objects.requireNonNull(pq.peek()).value;
        int rangeRight = maxInPq;
        while (true) {
            Triplet nextNumber = pq.remove();
            if (nextNumber.idx + 1 < nums.get(nextNumber.row).size()) {
                int val = nums.get(nextNumber.row).get(nextNumber.idx + 1);
                if (val > maxInPq) {
                    maxInPq = val;
                }
                pq.add(new Triplet(val, nextNumber.row, nextNumber.idx + 1));
                if (maxInPq - Objects.requireNonNull(pq.peek()).value + 1 < rangeSize) {
                    rangeSize = maxInPq - pq.peek().value + 1;
                    rangeLeft = maxInPq;
                    rangeRight = pq.peek().value;
                }
            } else {
                break;
            }
        }
        int[] answer = new int[2];
        answer[0] = rangeLeft;
        answer[1] = rangeRight;
        return answer;
    }
}
```