[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2191\. Sort the Jumbled Numbers

Medium

You are given a **0-indexed** integer array `mapping` which represents the mapping rule of a shuffled decimal system. `mapping[i] = j` means digit `i` should be mapped to digit `j` in this system.

The **mapped value** of an integer is the new integer obtained by replacing each occurrence of digit `i` in the integer with `mapping[i]` for all `0 <= i <= 9`.

You are also given another integer array `nums`. Return _the array_ `nums` _sorted in **non-decreasing** order based on the **mapped values** of its elements._

**Notes:**

*   Elements with the same mapped values should appear in the **same relative order** as in the input.
*   The elements of `nums` should only be sorted based on their mapped values and **not be replaced** by them.

**Example 1:**

**Input:** mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]

**Output:** [338,38,991]

**Explanation:**

Map the number 991 as follows:

1. mapping[9] = 6, so all occurrences of the digit 9 will become 6.

2. mapping[1] = 9, so all occurrences of the digit 1 will become 9.

Therefore, the mapped value of 991 is 669.

338 maps to 007, or 7 after removing the leading zeros.

38 maps to 07, which is also 7 after removing leading zeros.

Since 338 and 38 share the same mapped value, they should remain in the same relative order, so 338 comes before 38.

Thus, the sorted array is [338,38,991]. 

**Example 2:**

**Input:** mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]

**Output:** [123,456,789]

**Explanation:** 789 maps to 789, 456 maps to 456, and 123 maps to 123. Thus, the sorted array is [123,456,789]. 

**Constraints:**

*   `mapping.length == 10`
*   `0 <= mapping[i] <= 9`
*   All the values of `mapping[i]` are **unique**.
*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] < 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static class RealNum {
        int index;
        int orig;
        int real = 0;

        public RealNum(int[] mapping, int orig, int index) {
            this.orig = orig;
            this.index = index;
            int mult = 1;
            if (orig == 0) {
                real = mapping[0];
            } else {
                while (orig > 0) {
                    int mod = orig % 10;
                    orig = orig / 10;
                    real += mapping[mod] * mult;
                    mult *= 10;
                }
            }
        }
    }

    public int[] sortJumbled(int[] mapping, int[] nums) {
        List<RealNum> realNums = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            RealNum realNum = new RealNum(mapping, num, i);
            realNums.add(realNum);
        }
        realNums.sort(
                (a, b) -> {
                    int retval = a.real - b.real;
                    if (retval != 0) {
                        return retval;
                    }
                    return a.index - b.index;
                });
        int[] retval = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            retval[i] = realNums.get(i).orig;
        }
        return retval;
    }
}
```