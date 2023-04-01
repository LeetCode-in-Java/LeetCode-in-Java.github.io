[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1093\. Statistics from a Large Sample

Medium

You are given a large sample of integers in the range `[0, 255]`. Since the sample is so large, it is represented by an array `count` where `count[k]` is the **number of times** that `k` appears in the sample.

Calculate the following statistics:

*   `minimum`: The minimum element in the sample.
*   `maximum`: The maximum element in the sample.
*   `mean`: The average of the sample, calculated as the total sum of all elements divided by the total number of elements.
*   `median`:
    *   If the sample has an odd number of elements, then the `median` is the middle element once the sample is sorted.
    *   If the sample has an even number of elements, then the `median` is the average of the two middle elements once the sample is sorted.
*   `mode`: The number that appears the most in the sample. It is guaranteed to be **unique**.

Return _the statistics of the sample as an array of floating-point numbers_ `[minimum, maximum, mean, median, mode]`_. Answers within_ <code>10<sup>-5</sup></code> _of the actual answer will be accepted._

**Example 1:**

**Input:** count = [0,1,3,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

**Output:** [1.00000,3.00000,2.37500,2.50000,3.00000]

**Explanation:** The sample represented by count is [1,2,2,2,3,3,3,3].

The minimum and maximum are 1 and 3 respectively.

The mean is (1+2+2+2+3+3+3+3) / 8 = 19 / 8 = 2.375.

Since the size of the sample is even, the median is the average of the two middle elements 2 and 3, which is 2.5.

The mode is 3 as it appears the most in the sample.

**Example 2:**

**Input:** count = [0,4,3,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

**Output:** [1.00000,4.00000,2.18182,2.00000,1.00000]

**Explanation:** The sample represented by count is [1,1,1,1,2,2,2,3,3,4,4].

The minimum and maximum are 1 and 4 respectively.

The mean is (1+1+1+1+2+2+2+3+3+4+4) / 11 = 24 / 11 = 2.18181818... (for display purposes, the output shows the rounded number 2.18182).

Since the size of the sample is odd, the median is the middle element 2.

The mode is 1 as it appears the most in the sample.

**Constraints:**

*   `count.length == 256`
*   <code>0 <= count[i] <= 10<sup>9</sup></code>
*   <code>1 <= sum(count) <= 10<sup>9</sup></code>
*   The mode of the sample that `count` represents is **unique**.

## Solution

```java
public class Solution {
    public double[] sampleStats(int[] count) {
        int l = 0;
        int r = 255;
        int nl = 0;
        int nr = 0;
        int mn = 256;
        int mx = -1;
        int mid1 = 0;
        int mid2 = 0;
        int mode = 0;
        double avg = 0;
        double mid;
        while (l <= r) {
            while (count[l] == 0) {
                l++;
            }
            while (count[r] == 0) {
                r--;
            }
            if (nl < nr) {
                avg += (double) count[l] * l;
                nl += count[l];
                if (count[l] > count[mode]) {
                    mode = l;
                }
                mx = Math.max(mx, l);
                mn = Math.min(mn, l);
                mid1 = l;
                l++;
            } else {
                avg += (double) count[r] * r;
                nr += count[r];
                if (count[r] > count[mode]) {
                    mode = r;
                }
                mx = Math.max(mx, r);
                mn = Math.min(mn, r);
                mid2 = r;
                r--;
            }
        }
        avg /= nl + nr;
        // Find median
        if (nl < nr) {
            mid = mid2;
        } else if (nl > nr) {
            mid = mid1;
        } else {
            mid = (double) (mid1 + mid2) / 2;
        }
        return new double[] {mn, mx, avg, mid, mode};
    }
}
```