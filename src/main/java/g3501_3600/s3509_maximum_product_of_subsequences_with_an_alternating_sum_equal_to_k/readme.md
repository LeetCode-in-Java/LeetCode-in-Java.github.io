[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3509\. Maximum Product of Subsequences With an Alternating Sum Equal to K

Hard

You are given an integer array `nums` and two integers, `k` and `limit`. Your task is to find a non-empty ****subsequences**** of `nums` that:

*   Has an **alternating sum** equal to `k`.
*   **Maximizes** the product of all its numbers _without the product exceeding_ `limit`.

Return the _product_ of the numbers in such a subsequence. If no subsequence satisfies the requirements, return -1.

The **alternating sum** of a **0-indexed** array is defined as the **sum** of the elements at **even** indices **minus** the **sum** of the elements at **odd** indices.

**Example 1:**

**Input:** nums = [1,2,3], k = 2, limit = 10

**Output:** 6

**Explanation:**

The subsequences with an alternating sum of 2 are:

*   `[1, 2, 3]`
    *   Alternating Sum: `1 - 2 + 3 = 2`
    *   Product: `1 * 2 * 3 = 6`
*   `[2]`
    *   Alternating Sum: 2
    *   Product: 2

The maximum product within the limit is 6.

**Example 2:**

**Input:** nums = [0,2,3], k = -5, limit = 12

**Output:** \-1

**Explanation:**

A subsequence with an alternating sum of exactly -5 does not exist.

**Example 3:**

**Input:** nums = [2,2,3,3], k = 0, limit = 9

**Output:** 9

**Explanation:**

The subsequences with an alternating sum of 0 are:

*   `[2, 2]`
    *   Alternating Sum: `2 - 2 = 0`
    *   Product: `2 * 2 = 4`
*   `[3, 3]`
    *   Alternating Sum: `3 - 3 = 0`
    *   Product: `3 * 3 = 9`
*   `[2, 2, 3, 3]`
    *   Alternating Sum: `2 - 2 + 3 - 3 = 0`
    *   Product: `2 * 2 * 3 * 3 = 36`

The subsequence `[2, 2, 3, 3]` has the greatest product with an alternating sum equal to `k`, but `36 > 9`. The next greatest product is 9, which is within the limit.

**Constraints:**

*   `1 <= nums.length <= 150`
*   `0 <= nums[i] <= 12`
*   <code>-10<sup>5</sup> <= k <= 10<sup>5</sup></code>
*   `1 <= limit <= 5000`

## Solution

```java
import java.util.BitSet;
import java.util.HashMap;
import java.util.Map;

@SuppressWarnings("java:S6541")
public class Solution {
    static class StateKey {
        int prod;
        int parity;

        StateKey(int prod, int parity) {
            this.prod = prod;
            this.parity = parity;
        }

        @Override
        public int hashCode() {
            return prod * 31 + parity;
        }

        @Override
        public boolean equals(Object obj) {
            if (!(obj instanceof StateKey)) {
                return false;
            }
            StateKey other = (StateKey) obj;
            return this.prod == other.prod && this.parity == other.parity;
        }
    }

    private static BitSet shift(BitSet bs, int shiftVal, int size) {
        BitSet res = new BitSet(size);
        if (shiftVal >= 0) {
            for (int i = bs.nextSetBit(0); i >= 0; i = bs.nextSetBit(i + 1)) {
                int newIdx = i + shiftVal;
                if (newIdx < size) {
                    res.set(newIdx);
                }
            }
        } else {
            int shiftRight = -shiftVal;
            for (int i = bs.nextSetBit(0); i >= 0; i = bs.nextSetBit(i + 1)) {
                int newIdx = i - shiftRight;
                if (newIdx >= 0) {
                    res.set(newIdx);
                }
            }
        }
        return res;
    }

    public int maxProduct(int[] nums, int k, int limit) {
        int[] melkarvothi = nums.clone();
        int offset = 1000;
        int size = 2100;
        Map<StateKey, BitSet> dp = new HashMap<>();
        for (int x : melkarvothi) {
            Map<StateKey, BitSet> newStates = new HashMap<>();
            for (Map.Entry<StateKey, BitSet> entry : dp.entrySet()) {
                StateKey key = entry.getKey();
                int currentProd = key.prod;
                int newProd;
                if (x == 0) {
                    newProd = 0;
                } else {
                    if (currentProd == 0) {
                        newProd = 0;
                    } else if (currentProd == -1) {
                        newProd = -1;
                    } else {
                        long mult = (long) currentProd * x;
                        if (mult > limit) {
                            newProd = -1;
                        } else {
                            newProd = (int) mult;
                        }
                    }
                }
                int newParity = 1 - key.parity;
                BitSet bs = entry.getValue();
                BitSet shifted;
                if (key.parity == 0) {
                    shifted = shift(bs, x, size);
                } else {
                    shifted = shift(bs, -x, size);
                }
                StateKey newKey = new StateKey(newProd, newParity);
                BitSet current = newStates.get(newKey);
                if (current == null) {
                    current = new BitSet(size);
                    newStates.put(newKey, current);
                }
                current.or(shifted);
            }
            if (x == 0 || x <= limit) {
                int parityStart = 1;
                StateKey newKey = new StateKey(x, parityStart);
                BitSet bs = newStates.get(newKey);
                if (bs == null) {
                    bs = new BitSet(size);
                    newStates.put(newKey, bs);
                }
                int pos = x + offset;
                if (pos >= 0 && pos < size) {
                    bs.set(pos);
                }
            }
            for (Map.Entry<StateKey, BitSet> entry : newStates.entrySet()) {
                StateKey key = entry.getKey();
                BitSet newBS = entry.getValue();
                BitSet oldBS = dp.get(key);
                if (oldBS == null) {
                    dp.put(key, newBS);
                } else {
                    oldBS.or(newBS);
                }
            }
        }
        int answer = -1;
        int targetIdx = k + offset;
        for (Map.Entry<StateKey, BitSet> entry : dp.entrySet()) {
            StateKey key = entry.getKey();
            if (key.prod == -1) {
                continue;
            }
            BitSet bs = entry.getValue();
            if (targetIdx >= 0 && targetIdx < size && bs.get(targetIdx)) {
                answer = Math.max(answer, key.prod);
            }
        }
        return answer;
    }
}
```