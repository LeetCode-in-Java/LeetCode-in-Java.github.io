## 1980\. Find Unique Binary String

Medium

Given an array of strings `nums` containing `n` **unique** binary strings each of length `n`, return _a binary string of length_ `n` _that **does not appear** in_ `nums`_. If there are multiple answers, you may return **any** of them_.

**Example 1:**

**Input:** nums = ["01","10"]

**Output:** "11"

**Explanation:** "11" does not appear in nums. "00" would also be correct.

**Example 2:**

**Input:** nums = ["00","01"]

**Output:** "11"

**Explanation:** "11" does not appear in nums. "10" would also be correct.

**Example 3:**

**Input:** nums = ["111","011","001"]

**Output:** "101"

**Explanation:** "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.

**Constraints:**

*   `n == nums.length`
*   `1 <= n <= 16`
*   `nums[i].length == n`
*   `nums[i]` is either `'0'` or `'1'`.
*   All the strings of `nums` are **unique**.

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public String findDifferentBinaryString(String[] nums) {
        Set<String> set = new HashSet<>(Arrays.asList(nums));
        int len = nums[0].length();
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < len) {
            sb.append(1);
            i++;
        }
        int max = Integer.parseInt(sb.toString(), 2);
        for (int num = 0; num <= max; num++) {
            String binary = Integer.toBinaryString(num);
            if (binary.length() < len) {
                sb.setLength(0);
                sb.append(binary);
                while (sb.length() < len) {
                    sb.insert(0, "0");
                }
                binary = sb.toString();
            }
            if (!set.contains(binary)) {
                return binary;
            }
        }
        return null;
    }
}
```