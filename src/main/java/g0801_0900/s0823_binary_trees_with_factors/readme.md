## 823\. Binary Trees With Factors

Medium

Given an array of unique integers, `arr`, where each integer `arr[i]` is strictly greater than `1`.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return _the number of binary trees we can make_. The answer may be too large so return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [2,4]

**Output:** 3

**Explanation:** We can make these trees: `[2], [4], [4, 2, 2]`

**Example 2:**

**Input:** arr = [2,4,5,10]

**Output:** 7

**Explanation:** We can make these trees: `[2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2]`.

**Constraints:**

*   `1 <= arr.length <= 1000`
*   <code>2 <= arr[i] <= 10<sup>9</sup></code>
*   All the values of `arr` are **unique**.

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private Map<Integer, Long> dp = new HashMap<>();
    private Map<Integer, Integer> nums = new HashMap<>();
    private static final int MOD = (int) 1e9 + 7;

    public int numFactoredBinaryTrees(int[] arr) {
        Arrays.sort(arr);
        for (int i = 0; i < arr.length; i++) {
            nums.put(arr[i], i);
        }
        long ans = 0;
        for (int i = arr.length - 1; i >= 0; i--) {
            ans = (ans % MOD + recursion(arr, arr[i], i) % MOD) % MOD;
        }
        return (int) ans;
    }

    private long recursion(int[] arr, int v, int idx) {
        if (dp.containsKey(v)) {
            return dp.get(v);
        }
        long ret = 1;
        for (int i = 0; i < idx; i++) {
            int child = arr[i];
            if (v % child == 0 && nums.containsKey(v / child)) {
                ret +=
                        (recursion(arr, child, nums.get(arr[i]))
                                        % MOD
                                        * recursion(arr, v / child, nums.get(v / child))
                                        % MOD)
                                % MOD;
            }
        }
        dp.put(v, ret);
        return ret;
    }
}
```