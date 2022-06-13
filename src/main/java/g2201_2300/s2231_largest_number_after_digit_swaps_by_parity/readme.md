## 2231\. Largest Number After Digit Swaps by Parity

Easy

You are given a positive integer `num`. You may swap any two digits of `num` that have the same **parity** (i.e. both odd digits or both even digits).

Return _the **largest** possible value of_ `num` _after **any** number of swaps._

**Example 1:**

**Input:** num = 1234

**Output:** 3412

**Explanation:** Swap the digit 3 with the digit 1, this results in the number 3214.

Swap the digit 2 with the digit 4, this results in the number 3412.

Note that there may be other sequences of swaps but it can be shown that 3412 is the largest possible number.

Also note that we may not swap the digit 4 with the digit 1 since they are of different parities. 

**Example 2:**

**Input:** num = 65875

**Output:** 87655

**Explanation:** Swap the digit 8 with the digit 6, this results in the number 85675.

Swap the first digit 5 with the digit 7, this results in the number 87655.

Note that there may be other sequences of swaps but it can be shown that 87655 is the largest possible number. 

**Constraints:**

*   <code>1 <= num <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int largestInteger(int num) {
        char[] str = String.valueOf(num).toCharArray();
        char temp;
        for (int i = 0; i < str.length; i++) {
            temp = str[i];
            int swapIndex = i;
            boolean even = str[i] % 2 == 0;
            int max = Integer.MIN_VALUE;
            if (even) {
                for (int j = i + 1; j < str.length; j++) {
                    if (str[j] % 2 == 0 && str[j] > str[i] && str[j] > max) {
                        max = str[j];
                        temp = str[j];
                        swapIndex = j;
                    }
                }
            } else {
                for (int j = i + 1; j < str.length; j++) {
                    if (str[j] % 2 != 0 && str[j] > str[i] && str[j] > max) {
                        max = str[j];
                        temp = str[j];
                        swapIndex = j;
                    }
                }
            }
            char tempStr = str[i];
            str[i] = temp;
            str[swapIndex] = tempStr;
        }
        return Integer.valueOf(new String(str));
    }
}
```