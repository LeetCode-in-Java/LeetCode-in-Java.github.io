## 201\. Bitwise AND of Numbers Range

Medium

Given two integers `left` and `right` that represent the range `[left, right]`, return _the bitwise AND of all numbers in this range, inclusive_.

**Example 1:**

**Input:** left = 5, right = 7

**Output:** 4 

**Example 2:**

**Input:** left = 0, right = 0

**Output:** 0 

**Example 3:**

**Input:** left = 1, right = 2147483647

**Output:** 0 

**Constraints:**

*   <code>0 <= left <= right <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    private static final int[] MASKS =
            new int[] {
                0,
                0x80000000,
                0xC0000000,
                0xE0000000,
                0xF0000000,
                0xF8000000,
                0xFC000000,
                0xFE000000,
                0xFF000000,
                0xFF800000,
                0xFFC00000,
                0xFFE00000,
                0xFFF00000,
                0xFFF80000,
                0xFFFC0000,
                0xFFFE0000,
                0xFFFF0000,
                0xFFFF8000,
                0xFFFFC000,
                0xFFFFE000,
                0xFFFFF000,
                0xFFFFF800,
                0xFFFFFC00,
                0xFFFFFE00,
                0xFFFFFF00,
                0xFFFFFF80,
                0xFFFFFFC0,
                0xFFFFFFE0,
                0xFFFFFFF0,
                0xFFFFFFF8,
                0xFFFFFFFC,
                0xFFFFFFFE
            };

    public int rangeBitwiseAnd(int left, int right) {
        return left == right ? left : right & MASKS[Integer.numberOfLeadingZeros(left ^ right)];
    }
}
```