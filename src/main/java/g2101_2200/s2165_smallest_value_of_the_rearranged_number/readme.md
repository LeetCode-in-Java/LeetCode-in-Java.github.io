## 2165\. Smallest Value of the Rearranged Number

Medium

You are given an integer `num.` **Rearrange** the digits of `num` such that its value is **minimized** and it does not contain **any** leading zeros.

Return _the rearranged number with minimal value_.

Note that the sign of the number does not change after rearranging the digits.

**Example 1:**

**Input:** num = 310

**Output:** 103

**Explanation:** The possible arrangements for the digits of 310 are 013, 031, 103, 130, 301, 310. 

The arrangement with the smallest value that does not contain any leading zeros is 103. 

**Example 2:**

**Input:** num = -7605

**Output:** -7650

**Explanation:** Some possible arrangements for the digits of -7605 are -7650, -6705, -5076, -0567. 

The arrangement with the smallest value that does not contain any leading zeros is -7650. 

**Constraints:**

*   <code>-10<sup>15</sup> <= num <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long smallestNumber(long num) {
        int[] count = new int[10];
        long tempNum;
        if (num > 0) {
            tempNum = num;
        } else {
            tempNum = num * -1;
        }
        int min = 10;
        while (tempNum > 0) {
            int rem = (int) (tempNum % 10);
            if (rem != 0) {
                min = Math.min(min, rem);
            }
            count[rem]++;
            tempNum = tempNum / 10;
        }
        long output = 0;
        if (num > 0) {
            output = output * 10 + min;
            count[min]--;
            for (int i = 0; i < 10; i++) {
                for (int j = 0; j < count[i]; j++) {
                    output = output * 10 + i;
                }
            }
        } else {
            for (int i = 9; i >= 0; i--) {
                for (int j = 0; j < count[i]; j++) {
                    output = output * 10 + i;
                }
            }
            output = output * -1;
        }
        return output;
    }
}
```