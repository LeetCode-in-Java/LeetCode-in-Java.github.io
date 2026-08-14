[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3890\. Integers With Multiple Sum of Two Cubes

Medium

You are given an integer `n`.

An integer `x` is considered **good** if there exist **at least** two **distinct** pairs `(a, b)` such that:

*   `a` and `b` are positive integers.
*   `a <= b`
*   <code>x = a<sup>3</sup> + b<sup>3</sup></code>

Return an array containing all good integers **less than or equal to** `n`, sorted in ascending order.

**Example 1:**

**Input:** n = 4104

**Output:** [1729,4104]

**Explanation:**

Among integers less than or equal to 4104, the good integers are:

*   1729: <code>1<sup>3</sup> + 12<sup>3</sup> = 1729</code> and <code>9<sup>3</sup> + 10<sup>3</sup> = 1729</code>.
*   4104: <code>2<sup>3</sup> + 16<sup>3</sup> = 4104</code> and <code>9<sup>3</sup> + 15<sup>3</sup> = 4104</code>.

Thus, the answer is `[1729, 4104]`.

**Example 2:**

**Input:** n = 578

**Output:** []

**Explanation:**

There are no good integers less than or equal to 578, so the answer is an empty array.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<Integer> findGoodIntegers(int n) {
        Set<Integer> present = new HashSet<>();
        Set<Integer> result = new HashSet<>();
        for (int i = 1; i < 1000; i++) {
            int more = i * i * i;
            for (int j = 1; j <= i; j++) {
                int less = j * j * j;
                int sum = more + less;
                if (sum <= n) {
                    if (present.contains(sum)) {
                        result.add(sum);
                    } else {
                        present.add(sum);
                    }
                } else {
                    break;
                }
            }
        }
        List<Integer> ret = new ArrayList<>(result);
        Collections.sort(ret);
        return ret;
    }
}
```