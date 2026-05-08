[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3769\. Sort Integers by Binary Reflection

Easy

You are given an integer array `nums`.

The **binary reflection** of a **positive** integer is defined as the number obtained by reversing the order of its **binary** digits (ignoring any leading zeros) and interpreting the resulting binary number as a decimal.

Sort the array in **ascending** order based on the binary reflection of each element. If two different numbers have the same binary reflection, the **smaller** original number should appear first.

Return the resulting sorted array.

**Example 1:**

**Input:** nums = [4,5,4]

**Output:** [4,4,5]

**Explanation:**

Binary reflections are:

*   4 -> (binary) `100` -> (reversed) `001` -> 1
*   5 -> (binary) `101` -> (reversed) `101` -> 5
*   4 -> (binary) `100` -> (reversed) `001` -> 1

Sorting by the reflected values gives `[4, 4, 5]`.

**Example 2:**

**Input:** nums = [3,6,5,8]

**Output:** [8,3,6,5]

**Explanation:**

Binary reflections are:

*   3 -> (binary) `11` -> (reversed) `11` -> 3
*   6 -> (binary) `110` -> (reversed) `011` -> 3
*   5 -> (binary) `101` -> (reversed) `101` -> 5
*   8 -> (binary) `1000` -> (reversed) `0001` -> 1

Sorting by the reflected values gives `[8, 3, 6, 5]`.   
Note that 3 and 6 have the same reflection, so we arrange them in increasing order of original value.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.PriorityQueue;

public class Solution {
    public int[] sortByReflection(int[] nums) {
        PriorityQueue<int[]> minHeap =
                new PriorityQueue<>(
                        (a, b) -> {
                            if (a[1] == b[1]) {
                                return Integer.compare(a[0], b[0]);
                            }
                            return Integer.compare(a[1], b[1]);
                        });
        int[] sortedByReflection = new int[nums.length];
        for (int num : nums) {
            minHeap.offer(new int[] {num, reverseBinary(num)});
        }
        int idx = 0;
        while (!minHeap.isEmpty()) {
            sortedByReflection[idx++] = minHeap.poll()[0];
        }
        return sortedByReflection;
    }

    private int reverseBinary(int num) {
        int reversed = 0;
        while (num > 0) {
            int lastBit = num & 1;
            reversed = (reversed << 1) | lastBit;
            num = num >> 1;
        }
        return reversed;
    }
}
```