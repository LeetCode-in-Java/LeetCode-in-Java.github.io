[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3080\. Mark Elements on Array by Performing Queries

Medium

You are given a **0-indexed** array `nums` of size `n` consisting of positive integers.

You are also given a 2D array `queries` of size `m` where <code>queries[i] = [index<sub>i</sub>, k<sub>i</sub>]</code>.

Initially all elements of the array are **unmarked**.

You need to apply `m` queries on the array in order, where on the <code>i<sup>th</sup></code> query you do the following:

*   Mark the element at index <code>index<sub>i</sub></code> if it is not already marked.
*   Then mark <code>k<sub>i</sub></code> unmarked elements in the array with the **smallest** values. If multiple such elements exist, mark the ones with the smallest indices. And if less than <code>k<sub>i</sub></code> unmarked elements exist, then mark all of them.

Return _an array answer of size_ `m` _where_ `answer[i]` _is the **sum** of unmarked elements in the array after the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

**Input:** nums = [1,2,2,1,2,3,1], queries = \[\[1,2],[3,3],[4,2]]

**Output:** [8,3,0]

**Explanation:**

We do the following queries on the array:

*   Mark the element at index `1`, and `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,2,<ins>**1**</ins>,2,3,1]</code>. The sum of unmarked elements is `2 + 2 + 3 + 1 = 8`.
*   Mark the element at index `3`, since it is already marked we skip it. Then we mark `3` of the smallest unmarked elements with the smallest indices, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,<ins>**2**</ins>,<ins>**1**</ins>,<ins>**2**</ins>,3,**<ins>1</ins>**]</code>. The sum of unmarked elements is `3`.
*   Mark the element at index `4`, since it is already marked we skip it. Then we mark `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,<ins>**2**</ins>,<ins>**1**</ins>,<ins>**2**</ins>,**<ins>3</ins>**,<ins>**1**</ins>]</code>. The sum of unmarked elements is `0`.

**Example 2:**

**Input:** nums = [1,4,2,3], queries = \[\[0,1]]

**Output:** [7]

**Explanation:** We do one query which is mark the element at index `0` and mark the smallest element among unmarked elements. The marked elements will be <code>nums = [**<ins>1</ins>**,4,<ins>**2**</ins>,3]</code>, and the sum of unmarked elements is `4 + 3 = 7`.

**Constraints:**

*   `n == nums.length`
*   `m == queries.length`
*   <code>1 <= m <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= index<sub>i</sub>, k<sub>i</sub> <= n - 1</code>

## Solution

```java
@SuppressWarnings({"java:S1871", "java:S6541"})
public class Solution {
    public long[] unmarkedSumArray(int[] nums, int[][] queries) {
        int l = nums.length;
        int[] orig = new int[l];
        for (int i = 0; i < l; i++) {
            orig[i] = i;
        }
        int x = 1;
        while (x < l) {
            int[] temp = new int[l];
            int[] teor = new int[l];
            int y = 0;
            while (y < l) {
                int s1 = 0;
                int s2 = 0;
                while (s1 + s2 < 2 * x && y + s1 + s2 < l) {
                    if (s2 >= x || y + x + s2 >= l) {
                        temp[y + s1 + s2] = nums[y + s1];
                        teor[y + s1 + s2] = orig[y + s1];
                        s1++;
                    } else if (s1 >= x) {
                        temp[y + s1 + s2] = nums[y + x + s2];
                        teor[y + s1 + s2] = orig[y + x + s2];
                        s2++;
                    } else if (nums[y + s1] <= nums[y + x + s2]) {
                        temp[y + s1 + s2] = nums[y + s1];
                        teor[y + s1 + s2] = orig[y + s1];
                        s1++;
                    } else {
                        temp[y + s1 + s2] = nums[y + x + s2];
                        teor[y + s1 + s2] = orig[y + x + s2];
                        s2++;
                    }
                }
                y += 2 * x;
            }
            for (int i = 0; i < l; i++) {
                nums[i] = temp[i];
                orig[i] = teor[i];
            }
            x *= 2;
        }
        int[] change = new int[l];
        for (int i = 0; i < l; i++) {
            change[orig[i]] = i;
        }
        boolean[] mark = new boolean[l];
        int m = queries.length;
        int st = 0;
        long sum = 0;
        for (int num : nums) {
            sum += num;
        }
        long[] out = new long[m];
        for (int i = 0; i < m; i++) {
            int a = queries[i][0];
            if (!mark[change[a]]) {
                mark[change[a]] = true;
                sum -= nums[change[a]];
            }
            int b = queries[i][1];
            int many = 0;
            while (many < b) {
                if (st == l) {
                    out[i] = sum;
                    break;
                }
                if (!mark[st]) {
                    mark[st] = true;
                    sum -= nums[st];
                    many++;
                }
                st++;
            }
            out[i] = sum;
        }
        return out;
    }
}
```