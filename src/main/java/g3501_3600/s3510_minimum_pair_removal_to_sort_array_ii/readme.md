[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3510\. Minimum Pair Removal to Sort Array II

Hard

Given an array `nums`, you can perform the following operation any number of times:

*   Select the **adjacent** pair with the **minimum** sum in `nums`. If multiple such pairs exist, choose the leftmost one.
*   Replace the pair with their sum.

Return the **minimum number of operations** needed to make the array **non-decreasing**.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous element (if it exists).

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** 2

**Explanation:**

*   The pair `(3,1)` has the minimum sum of 4. After replacement, `nums = [5,2,4]`.
*   The pair `(2,4)` has the minimum sum of 6. After replacement, `nums = [5,6]`.

The array `nums` became non-decreasing in two operations.

**Example 2:**

**Input:** nums = [1,2,2]

**Output:** 0

**Explanation:**

The array `nums` is already sorted.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private static class Segment {
        private final int start;
        private final int end;
        private Segment left;
        private Segment right;
        private int lIdx;
        private long lNum;
        private int rIdx;
        private long rNum;
        private boolean ok;
        private long minSum;
        private int li;
        private int ri;

        public static Segment init(int[] arr) {
            return new Segment(arr, 0, arr.length - 1);
        }

        public Segment(int[] arr, int s, int e) {
            start = s;
            end = e;
            if (s >= e) {
                lIdx = rIdx = s;
                lNum = rNum = arr[s];
                minSum = Long.MAX_VALUE;
                ok = true;
                return;
            }
            int mid = s + ((e - s) >> 1);
            left = new Segment(arr, s, mid);
            right = new Segment(arr, mid + 1, e);
            merge();
        }

        private void merge() {
            lIdx = left.lIdx;
            lNum = left.lNum;
            rIdx = right.rIdx;
            rNum = right.rNum;
            ok = left.ok && right.ok && left.rNum <= right.lNum;
            minSum = left.minSum;
            li = left.li;
            ri = left.ri;
            if (left.rNum + right.lNum < minSum) {
                minSum = left.rNum + right.lNum;
                li = left.rIdx;
                ri = right.lIdx;
            }
            if (right.minSum < minSum) {
                minSum = right.minSum;
                li = right.li;
                ri = right.ri;
            }
        }

        public void update(int i, long n) {
            if (start <= i && end >= i) {
                if (start >= end) {
                    lNum = rNum = n;
                } else {
                    left.update(i, n);
                    right.update(i, n);
                    merge();
                }
            }
        }

        public Segment remove(int i) {
            if (start > i || end < i) {
                return this;
            } else if (start >= end) {
                return null;
            }
            left = left.remove(i);
            right = right.remove(i);
            if (null == left) {
                return right;
            } else if (null == right) {
                return left;
            }
            merge();
            return this;
        }
    }

    public int minimumPairRemoval(int[] nums) {
        Segment root = Segment.init(nums);
        int res = 0;
        while (!root.ok) {
            int l = root.li;
            int r = root.ri;
            root.update(l, root.minSum);
            root = root.remove(r);
            res++;
        }
        return res;
    }
}
```