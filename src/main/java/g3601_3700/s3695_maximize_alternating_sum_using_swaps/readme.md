[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3695\. Maximize Alternating Sum Using Swaps

Hard

You are given an integer array `nums`.

You want to maximize the **alternating sum** of `nums`, which is defined as the value obtained by **adding** elements at even indices and **subtracting** elements at odd indices. That is, `nums[0] - nums[1] + nums[2] - nums[3]...`

You are also given a 2D integer array `swaps` where <code>swaps[i] = [p<sub>i</sub>, q<sub>i</sub>]</code>. For each pair <code>[p<sub>i</sub>, q<sub>i</sub>]</code> in `swaps`, you are allowed to swap the elements at indices <code>p<sub>i</sub></code> and <code>q<sub>i</sub></code>. These swaps can be performed any number of times and in any order.

Return the maximum possible **alternating sum** of `nums`.

**Example 1:**

**Input:** nums = [1,2,3], swaps = \[\[0,2],[1,2]]

**Output:** 4

**Explanation:**

The maximum alternating sum is achieved when `nums` is `[2, 1, 3]` or `[3, 1, 2]`. As an example, you can obtain `nums = [2, 1, 3]` as follows.

*   Swap `nums[0]` and `nums[2]`. `nums` is now `[3, 2, 1]`.
*   Swap `nums[1]` and `nums[2]`. `nums` is now `[3, 1, 2]`.
*   Swap `nums[0]` and `nums[2]`. `nums` is now `[2, 1, 3]`.

**Example 2:**

**Input:** nums = [1,2,3], swaps = \[\[1,2]]

**Output:** 2

**Explanation:**

The maximum alternating sum is achieved by not performing any swaps.

**Example 3:**

**Input:** nums = [1,1000000000,1,1000000000,1,1000000000], swaps = []

**Output:** \-2999999997

**Explanation:**

Since we cannot perform any swaps, the maximum alternating sum is achieved by not performing any swaps.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= swaps.length <= 10<sup>5</sup></code>
*   <code>swaps[i] = [p<sub>i</sub>, q<sub>i</sub>]</code>
*   <code>0 <= p<sub>i</sub> < q<sub>i</sub> <= nums.length - 1</code>
*   <code>[p<sub>i</sub>, q<sub>i</sub>] != [p<sub>j</sub>, q<sub>j</sub>]</code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private int[] root;

    public long maxAlternatingSum(int[] nums, int[][] swaps) {
        int n = nums.length;
        root = new int[n];
        List<Integer>[] list = new ArrayList[n];
        int[] oddCount = new int[n];
        for (int i = 0; i < n; i++) {
            root[i] = i;
            list[i] = new ArrayList<>();
        }
        for (int[] s : swaps) {
            union(s[0], s[1]);
        }
        for (int i = 0; i < n; i++) {
            int r = findRoot(i);
            list[r].add(nums[i]);
            if (i % 2 == 1) {
                oddCount[r]++;
            }
        }
        long result = 0;
        for (int i = 0; i < n; i++) {
            int r = root[i];
            if (r != i) {
                continue;
            }
            list[i].sort((a, b) -> a - b);
            for (int j = 0; j < list[i].size(); j++) {
                result += (long) list[i].get(j) * (j < oddCount[r] ? -1 : 1);
            }
        }
        return result;
    }

    private void union(int a, int b) {
        a = findRoot(a);
        b = findRoot(b);
        if (a == b) {
            return;
        }
        if (a < b) {
            root[b] = a;
        } else {
            root[a] = b;
        }
    }

    private int findRoot(int a) {
        if (a == root[a]) {
            return a;
        }
        root[a] = findRoot(root[a]);
        return root[a];
    }
}
```