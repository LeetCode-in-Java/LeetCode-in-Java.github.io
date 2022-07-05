[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1803\. Count Pairs With XOR in a Range

Hard

Given a **(0-indexed)** integer array `nums` and two integers `low` and `high`, return _the number of **nice pairs**_.

A **nice pair** is a pair `(i, j)` where `0 <= i < j < nums.length` and `low <= (nums[i] XOR nums[j]) <= high`.

**Example 1:**

**Input:** nums = [1,4,2,7], low = 2, high = 6

**Output:** 6

**Explanation:** All nice pairs (i, j) are as follows:

- (0, 1): nums[0] XOR nums[1] = 5 

- (0, 2): nums[0] XOR nums[2] = 3 

- (0, 3): nums[0] XOR nums[3] = 6 

- (1, 2): nums[1] XOR nums[2] = 6 

- (1, 3): nums[1] XOR nums[3] = 3 

- (2, 3): nums[2] XOR nums[3] = 5

**Example 2:**

**Input:** nums = [9,8,4,2,1], low = 5, high = 14

**Output:** 8

**Explanation:** All nice pairs (i, j) are as follows: 

- (0, 2): nums[0] XOR nums[2] = 13 

- (0, 3): nums[0] XOR nums[3] = 11 

- (0, 4): nums[0] XOR nums[4] = 8 

- (1, 2): nums[1] XOR nums[2] = 12 

- (1, 3): nums[1] XOR nums[3] = 10 

- (1, 4): nums[1] XOR nums[4] = 9 

- (2, 3): nums[2] XOR nums[3] = 6 

- (2, 4): nums[2] XOR nums[4] = 5

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   <code>1 <= low <= high <= 2 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int countPairs(int[] nums, int low, int high) {
        Trie root = new Trie();
        int pairsCount = 0;
        for (int num : nums) {
            int pairsCountHigh = countPairsWhoseXorLessThanX(num, root, high + 1);
            int pairsCountLow = countPairsWhoseXorLessThanX(num, root, low);
            pairsCount += (pairsCountHigh - pairsCountLow);
            root.insertNumber(num);
        }
        return pairsCount;
    }

    private int countPairsWhoseXorLessThanX(int num, Trie root, int x) {
        int pairs = 0;
        Trie curr = root;
        for (int i = 14; i >= 0 && curr != null; i--) {
            int numIthBit = (num >> i) & 1;
            int xIthBit = (x >> i) & 1;
            if (xIthBit == 1) {
                if (curr.child[numIthBit] != null) {
                    pairs += curr.child[numIthBit].count;
                }
                curr = curr.child[1 - numIthBit];
            } else {
                curr = curr.child[numIthBit];
            }
        }
        return pairs;
    }

    private static class Trie {
        Trie[] child;
        int count;

        public Trie() {
            child = new Trie[2];
            count = 0;
        }

        public void insertNumber(int num) {
            Trie curr = this;
            for (int i = 14; i >= 0; i--) {
                int ithBit = (num >> i) & 1;
                if (curr.child[ithBit] == null) {
                    curr.child[ithBit] = new Trie();
                }
                curr.child[ithBit].count++;
                curr = curr.child[ithBit];
            }
        }
    }
}
```