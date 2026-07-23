[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3845\. Maximum Subarray XOR with Bounded Range

Hard

You are given a non-negative integer array `nums` and an integer `k`.

You must select a **non-empty subarrays** of `nums` such that the **difference** between its **maximum** and **minimum** elements is at most `k`. The **value** of this subarray is the bitwise XOR of all elements in the subarray.

Return an integer denoting the **maximum** possible **value** of the selected subarray.

**Example 1:**

**Input:** nums = [5,4,5,6], k = 2

**Output:** 7

**Explanation:**

*   Select the subarray <code>[5, <ins>**4, 5, 6**</ins>]</code>.
*   The difference between its maximum and minimum elements is `6 - 4 = 2 <= k`.
*   The value is `4 XOR 5 XOR 6 = 7`.

**Example 2:**

**Input:** nums = [5,4,5,6], k = 1

**Output:** 6

**Explanation:**

*   Select the subarray <code>[5, 4, 5, <ins>**6**</ins>]</code>.
*   The difference between its maximum and minimum elements is `6 - 6 = 0 <= k`.
*   The value is 6.

**Constraints:**

*   <code>1 <= nums.length <= 4 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] < 2<sup>15</sup></code>
*   <code>0 <= k < 2<sup>15</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    private static final int BITS = 15;

    static class TrieNode {
        TrieNode[] next = new TrieNode[2];
        int count = 0;
    }

    private void insert(TrieNode root, int num) {
        TrieNode node = root;
        for (int i = BITS - 1; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.next[bit] == null) {
                node.next[bit] = new TrieNode();
            }
            node = node.next[bit];
            node.count++;
        }
    }

    private void remove(TrieNode root, int num) {
        TrieNode node = root;
        for (int i = BITS - 1; i >= 0; i--) {
            int bit = (num >> i) & 1;
            TrieNode child = node.next[bit];
            child.count--;
            if (child.count == 0) {
                node.next[bit] = null;
            }
            node = child;
        }
    }

    private int query(TrieNode root, int num) {
        TrieNode node = root;
        int res = 0;
        for (int i = BITS - 1; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.next[bit ^ 1] != null) {
                res |= (1 << i);
                node = node.next[bit ^ 1];
            } else {
                node = node.next[bit];
            }
        }
        return res;
    }

    public int maxXor(int[] nums, int k) {
        int n = nums.length;
        int[] prefix = new int[n + 1];
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] ^ nums[i];
        }
        Deque<Integer> maxDeque = new ArrayDeque<>();
        Deque<Integer> minDeque = new ArrayDeque<>();
        TrieNode root = new TrieNode();
        int maxXor = 0;
        int l = 0;
        insert(root, 0);
        for (int r = 0; r < n; r++) {
            // maintain max deque
            while (!maxDeque.isEmpty() && nums[r] > nums[maxDeque.peekLast()]) {
                maxDeque.pollLast();
            }
            maxDeque.offerLast(r);
            // maintain min deque
            while (!minDeque.isEmpty() && nums[r] < nums[minDeque.peekLast()]) {
                minDeque.pollLast();
            }
            minDeque.offerLast(r);
            // shrink window if max-min > k
            while (!maxDeque.isEmpty()
                    && !minDeque.isEmpty()
                    && nums[maxDeque.peekFirst()] - nums[minDeque.peekFirst()] > k) {
                remove(root, prefix[l]);
                l++;
                if (maxDeque.peekFirst() < l) {
                    maxDeque.pollFirst();
                }
                if (minDeque.peekFirst() < l) {
                    minDeque.pollFirst();
                }
            }
            // query and insert prefix[r+1]
            maxXor = Math.max(maxXor, query(root, prefix[r + 1]));
            insert(root, prefix[r + 1]);
        }
        return maxXor;
    }
}
```