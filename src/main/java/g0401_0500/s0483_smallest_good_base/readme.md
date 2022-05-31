## 483\. Smallest Good Base

Hard

Given an integer `n` represented as a string, return _the smallest **good base** of_ `n`.

We call `k >= 2` a **good base** of `n`, if all digits of `n` base `k` are `1`'s.

**Example 1:**

**Input:** n = "13"

**Output:** "3"

**Explanation:** 13 base 3 is 111.

**Example 2:**

**Input:** n = "4681"

**Output:** "8"

**Explanation:** 4681 base 8 is 11111.

**Example 3:**

**Input:** n = "1000000000000000000"

**Output:** "999999999999999999"

**Explanation:** 1000000000000000000 base 999999999999999999 is 11.

**Constraints:**

*   `n` is an integer in the range <code>[3, 10<sup>18</sup>]</code>.
*   `n` does not contain any leading zeros.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public String smallestGoodBase(String n) {
        return sol1(n);
    }

    private String sol1(String n) {
        long x = Long.parseLong(n);
        List<Long> ans = new ArrayList<>();
        ans.add(x - 1);
        long y = x - 1;
        for (int i = 2; i < 63; i++) {
            double dm = Math.pow(y, 1.0 / i);
            long dml = (long) dm;
            for (int j = 0; j > -3 && dml + j > 1; j--) {
                long d = dml + j;
                if (y % d == 0) {
                    long poly = poly(d, i);
                    if (poly == x) {
                        ans.add(d);
                    }
                }
            }
        }
        long end = ans.get(ans.size() - 1);
        return end + "";
    }

    private long poly(long b, int n) {
        long ans = 1;
        long m = 1;
        for (int i = 0; i < n; i++) {
            m *= b;
            ans += m;
        }
        return ans;
    }
}
```