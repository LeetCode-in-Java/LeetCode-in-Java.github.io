## 1363\. Largest Multiple of Three

Hard

Given an array of digits `digits`, return _the largest multiple of **three** that can be formed by concatenating some of the given digits in **any order**_. If there is no answer return an empty string.

Since the answer may not fit in an integer data type, return the answer as a string. Note that the returning answer must not contain unnecessary leading zeros.

**Example 1:**

**Input:** digits = [8,1,9]

**Output:** "981"

**Example 2:**

**Input:** digits = [8,6,7,1,0]

**Output:** "8760"

**Example 3:**

**Input:** digits = [1]

**Output:** ""

**Constraints:**

*   <code>1 <= digits.length <= 10<sup>4</sup></code>
*   `0 <= digits[i] <= 9`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public String largestMultipleOfThree(int[] digits) {
        int sum = 0;
        int[] count = new int[10];
        // Here we are using the property that any no is divisible by 3 when its sum of digits is
        // divisible by 3
        // get sum of digits and count of each digit
        for (int x : digits) {
            sum += x;
            count[x]++;
        }
        StringBuilder sb = new StringBuilder();
        int[] copied = Arrays.copyOf(count, count.length);
        // if sum % 3 != 0 then processing required
        if (sum % 3 != 0) {
            int rem = sum % 3;
            int oldRem = rem;
            while (oldRem != 0) {
                while (rem != 0) {
                    // if the remainder that we are trying to delete and its required digits is not
                    // present
                    // then the value will become -ve at that digit
                    copied[rem % 10]--;
                    // increase the remainder by 3 each time a -ve value is found
                    // and reset the rem and copied from orig count array and break
                    if (copied[rem % 10] < 0) {
                        oldRem += 3;
                        rem = oldRem;
                        copied = Arrays.copyOf(count, count.length);
                        break;
                    }
                    rem /= 10;
                    if (rem == 0) {
                        oldRem = 0;
                    }
                }
            }
        }
        // generate the largest number by considering from the last digit ie 9,8,7,6...
        for (int i = 9; i >= 0; i--) {
            int val = copied[i];
            while (val > 0) {
                sb.append(i);
                val--;
            }
        }
        // check for any leading zeroes and remove
        while (sb.length() > 1) {
            if (sb.charAt(0) != '0') {
                break;
            } else {
                sb.deleteCharAt(0);
            }
        }
        return sb.toString();
    }
}
```