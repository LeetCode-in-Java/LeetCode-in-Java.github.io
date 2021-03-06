[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 321\. Create Maximum Number

Hard

You are given two integer arrays `nums1` and `nums2` of lengths `m` and `n` respectively. `nums1` and `nums2` represent the digits of two numbers. You are also given an integer `k`.

Create the maximum number of length `k <= m + n` from digits of the two numbers. The relative order of the digits from the same array must be preserved.

Return an array of the `k` digits representing the answer.

**Example 1:**

**Input:** nums1 = [3,4,6,5], nums2 = [9,1,2,5,8,3], k = 5

**Output:** [9,8,6,5,3] 

**Example 2:**

**Input:** nums1 = [6,7], nums2 = [6,0,4], k = 5

**Output:** [6,7,6,0,4] 

**Example 3:**

**Input:** nums1 = [3,9], nums2 = [8,9], k = 3

**Output:** [9,8,9] 

**Constraints:**

*   `m == nums1.length`
*   `n == nums2.length`
*   `1 <= m, n <= 500`
*   `0 <= nums1[i], nums2[i] <= 9`
*   `1 <= k <= m + n`

## Solution

```java
public class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        if (k == 0) {
            return new int[0];
        }
        int[] maxSubNums1 = new int[k];
        int[] maxSubNums2 = new int[k];
        int[] res = new int[k];
        // select l elements from nums1
        for (int l = 0; l <= Math.min(k, nums1.length); l++) {
            if (l + nums2.length < k) {
                continue;
            }
            // create maximum number for each array
            // nums1: l elements; nums2: k - l elements
            maxSubArray(nums1, maxSubNums1, l);
            maxSubArray(nums2, maxSubNums2, k - l);
            // merge the two maximum numbers
            // if get a larger number than res, update res
            res = merge(maxSubNums1, maxSubNums2, l, k - l, res);
        }
        return res;
    }

    private void maxSubArray(int[] nums, int[] maxSub, int size) {
        if (size == 0) {
            return;
        }
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            while (j > 0 && nums.length - i + j > size && nums[i] > maxSub[j - 1]) {
                j--;
            }
            if (j < size) {
                maxSub[j++] = nums[i];
            }
        }
    }

    private int[] merge(int[] maxSub1, int[] maxSub2, int size1, int size2, int[] res) {
        int[] merge = new int[res.length];
        int i = 0;
        int j = 0;
        int idx = 0;
        boolean equal = true;
        while (i < size1 || j < size2) {
            if (j >= size2) {
                merge[idx] = maxSub1[i++];
            } else if (i >= size1) {
                merge[idx] = maxSub2[j++];
            } else {
                int ii = i;
                int jj = j;
                while (ii < size1 && jj < size2 && maxSub1[ii] == maxSub2[jj]) {
                    ii++;
                    jj++;
                }
                if (ii < size1 && jj < size2) {
                    if (maxSub1[ii] > maxSub2[jj]) {
                        merge[idx] = maxSub1[i++];
                    } else {
                        merge[idx] = maxSub2[j++];
                    }
                } else if (jj == size2) {
                    merge[idx] = maxSub1[i++];
                } else {
                    // ii == size1
                    merge[idx] = maxSub2[j++];
                }
            }
            // break if we already know merge must be < res
            if (merge[idx] > res[idx]) {
                equal = false;
            } else if (equal && merge[idx] < res[idx]) {
                break;
            }
            idx++;
        }
        // if get a larger number than res, update res
        int k = res.length;
        if (i == size1 && j == size2 && !equal) {
            return merge;
        }
        if (equal && merge[k - 1] > res[k - 1]) {
            return merge;
        }
        return res;
    }
}
```