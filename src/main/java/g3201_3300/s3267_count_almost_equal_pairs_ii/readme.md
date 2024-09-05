[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3267\. Count Almost Equal Pairs II

Hard

**Attention**: In this version, the number of operations that can be performed, has been increased to **twice**.

You are given an array `nums` consisting of positive integers.

We call two integers `x` and `y` **almost equal** if both integers can become equal after performing the following operation **at most <ins>twice</ins>**:

*   Choose **either** `x` or `y` and swap any two digits within the chosen number.

Return the number of indices `i` and `j` in `nums` where `i < j` such that `nums[i]` and `nums[j]` are **almost equal**.

**Note** that it is allowed for an integer to have leading zeros after performing an operation.

**Example 1:**

**Input:** nums = [1023,2310,2130,213]

**Output:** 4

**Explanation:**

The almost equal pairs of elements are:

*   1023 and 2310. By swapping the digits 1 and 2, and then the digits 0 and 3 in 1023, you get 2310.
*   1023 and 213. By swapping the digits 1 and 0, and then the digits 1 and 2 in 1023, you get 0213, which is 213.
*   2310 and 213. By swapping the digits 2 and 0, and then the digits 3 and 2 in 2310, you get 0213, which is 213.
*   2310 and 2130. By swapping the digits 3 and 1 in 2310, you get 2130.

**Example 2:**

**Input:** nums = [1,10,100]

**Output:** 3

**Explanation:**

The almost equal pairs of elements are:

*   1 and 10. By swapping the digits 1 and 0 in 10, you get 01 which is 1.
*   1 and 100. By swapping the second 0 with the digit 1 in 100, you get 001, which is 1.
*   10 and 100. By swapping the first 0 with the digit 1 in 100, you get 010, which is 10.

**Constraints:**

*   `2 <= nums.length <= 5000`
*   <code>1 <= nums[i] < 10<sup>7</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    public int countPairs(int[] nums) {
        int pairs = 0;
        Map<Integer, Integer> counts = new HashMap<>();
        Arrays.sort(nums);
        for (int num : nums) {
            Set<Integer> newNums = new HashSet<>();
            newNums.add(num);
            for (int unit1 = 1, remain1 = num; remain1 > 0; unit1 *= 10, remain1 /= 10) {
                int digit1 = num / unit1 % 10;
                for (int unit2 = unit1 * 10, remain2 = remain1 / 10;
                        remain2 > 0;
                        unit2 *= 10, remain2 /= 10) {
                    int digit2 = num / unit2 % 10;
                    int newNum1 =
                            num - digit1 * unit1 - digit2 * unit2 + digit2 * unit1 + digit1 * unit2;
                    newNums.add(newNum1);
                    for (int unit3 = unit1 * 10, remain3 = remain1 / 10;
                            remain3 > 0;
                            unit3 *= 10, remain3 /= 10) {
                        int digit3 = newNum1 / unit3 % 10;
                        for (int unit4 = unit3 * 10, remain4 = remain3 / 10;
                                remain4 > 0;
                                unit4 *= 10, remain4 /= 10) {
                            int digit4 = newNum1 / unit4 % 10;
                            int newNum2 =
                                    newNum1
                                            - digit3 * unit3
                                            - digit4 * unit4
                                            + digit4 * unit3
                                            + digit3 * unit4;
                            newNums.add(newNum2);
                        }
                    }
                }
            }
            for (int newNum : newNums) {
                pairs += counts.getOrDefault(newNum, 0);
            }
            counts.put(num, counts.getOrDefault(num, 0) + 1);
        }
        return pairs;
    }
}
```