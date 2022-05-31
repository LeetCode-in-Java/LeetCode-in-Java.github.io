## 1780\. Check if Number is a Sum of Powers of Three

Medium

Given an integer `n`, return `true` _if it is possible to represent_ `n` _as the sum of distinct powers of three._ Otherwise, return `false`.

An integer `y` is a power of three if there exists an integer `x` such that <code>y == 3<sup>x</sup></code>.

**Example 1:**

**Input:** n = 12

**Output:** true

**Explanation:** 12 = 3<sup>1</sup> + 3<sup>2</sup>

**Example 2:**

**Input:** n = 91

**Output:** true

**Explanation:** 91 = 3<sup>0</sup> + 3<sup>2</sup> + 3<sup>4</sup>

**Example 3:**

**Input:** n = 21

**Output:** false

**Constraints:**

*   <code>1 <= n <= 10<sup>7</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public boolean checkPowersOfThree(int n) {
        List<Integer> powers = new ArrayList<>();
        int power = 1;
        for (int i = 1; power <= n; i++) {
            powers.add(power);
            power = (int) Math.pow(3, i);
        }
        int i = powers.size() - 1;
        while (n > 0 && i >= 0) {
            if (n - powers.get(i) > 0) {
                n -= powers.get(i--);
            } else if (n - powers.get(i) == 0) {
                return true;
            } else {
                i--;
            }
        }
        return n == 0;
    }
}
```