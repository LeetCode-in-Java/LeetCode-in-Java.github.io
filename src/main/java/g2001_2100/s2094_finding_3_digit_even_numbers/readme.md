[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2094\. Finding 3-Digit Even Numbers

Easy

You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

You need to find **all** the **unique** integers that follow the given requirements:

*   The integer consists of the **concatenation** of **three** elements from `digits` in **any** arbitrary order.
*   The integer does not have **leading zeros**.
*   The integer is **even**.

For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

Return _a **sorted** array of the unique integers._

**Example 1:**

**Input:** digits = [2,1,3,0]

**Output:** [102,120,130,132,210,230,302,310,312,320]

**Explanation:** All the possible integers that follow the requirements are in the output array.

Notice that there are no **odd** integers or integers with **leading zeros**. 

**Example 2:**

**Input:** digits = [2,2,8,8,2]

**Output:** [222,228,282,288,822,828,882]

**Explanation:** The same digit can be used as many times as it appears in digits.

In this example, the digit 8 is used twice each time in 288, 828, and 882. 

**Example 3:**

**Input:** digits = [3,7,5]

**Output:** []

**Explanation:** No **even** integers can be formed using the given digits. 

**Constraints:**

*   `3 <= digits.length <= 100`
*   `0 <= digits[i] <= 9`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] findEvenNumbers(int[] digits) {
        int[] idx = new int[1];
        int[] result = new int[9 * 10 * 5];
        int[] digitMap = new int[10];
        for (int digit : digits) {
            digitMap[digit]++;
        }
        dfs(result, digitMap, idx, 0);
        return Arrays.copyOfRange(result, 0, idx[0]);
    }

    private void dfs(int[] result, int[] digitMap, int[] idx, int val) {
        if (val > 99) {
            result[idx[0]++] = val;
            return;
        }
        val *= 10;
        for (int i = 0; i < 10; i++) {
            if (digitMap[i] == 0 || (val == 0 && i == 0) || (val > 99 && (i & 1) == 1)) {
                continue;
            }
            digitMap[i]--;
            val += i;
            dfs(result, digitMap, idx, val);
            val -= i;
            digitMap[i]++;
        }
    }
}
```