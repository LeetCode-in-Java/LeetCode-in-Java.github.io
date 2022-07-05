[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 315\. Count of Smaller Numbers After Self

Hard

You are given an integer array `nums` and you have to return a new `counts` array. The `counts` array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example 1:**

**Input:** nums = [5,2,6,1]

**Output:** [2,1,1,0]

**Explanation:** To the right of 5 there are **2** smaller elements (2 and 1). To the right of 2 there is only **1** smaller element (1). To the right of 6 there is **1** smaller element (1). To the right of 1 there is **0** smaller element. 

**Example 2:**

**Input:** nums = [-1]

**Output:** [0] 

**Example 3:**

**Input:** nums = [-1,-1]

**Output:** [0,0] 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int minVal = 10001;
        int maxVal = -10001;
        for (int a : nums) {
            minVal = Math.min(minVal, a);
            maxVal = Math.max(maxVal, a);
        }
        int range = maxVal - (minVal - 1) + 1;
        int offset = -(minVal - 1);
        FenwickTree bit = new FenwickTree(range);
        LinkedList<Integer> ans = new LinkedList<>();
        for (int n = nums.length, i = n - 1; i >= 0; i--) {
            bit.update(offset + nums[i], 1);
            ans.addFirst(bit.ps(offset + nums[i] - 1));
        }
        return ans;
    }

    private static class FenwickTree {
        // binary index tree, index 0 is not used
        int[] bit;
        int n;

        public FenwickTree(int n) {
            this.n = n + 1;
            this.bit = new int[this.n];
        }

        public void update(int i, int v) {
            for (; i < n; i += Integer.lowestOneBit(i)) {
                bit[i] += v;
            }
        }
        // prefix sum query
        private int ps(int j) {
            int ps = 0;
            for (; j != 0; j -= Integer.lowestOneBit(j)) {
                ps += bit[j];
            }
            return ps;
        }
    }
}
```