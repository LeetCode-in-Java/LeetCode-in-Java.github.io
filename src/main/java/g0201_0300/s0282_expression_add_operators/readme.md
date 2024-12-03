[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 282\. Expression Add Operators

Hard

Given a string `num` that contains only digits and an integer `target`, return _**all possibilities** to insert the binary operators_ `'+'`_,_ `'-'`_, and/or_ `'*'` _between the digits of_ `num` _so that the resultant expression evaluates to the_ `target` _value_.

Note that operands in the returned expressions **should not** contain leading zeros.

**Example 1:**

**Input:** num = "123", target = 6

**Output:** ["1\*2\*3","1+2+3"]

**Explanation:** Both "1\*2\*3" and "1+2+3" evaluate to 6. 

**Example 2:**

**Input:** num = "232", target = 8

**Output:** ["2\*3+2","2+3\*2"]

**Explanation:** Both "2\*3+2" and "2+3\*2" evaluate to 8. 

**Example 3:**

**Input:** num = "105", target = 5

**Output:** ["1\*0+5","10-5"]

**Explanation:**

    Both "1*0+5" and "10-5" evaluate to 5.
    Note that "1-05" is not a valid expression because the 5 has a leading zero. 

**Example 4:**

**Input:** num = "00", target = 0

**Output:** ["0\*0","0+0","0-0"]

**Explanation:**

    "0*0", "0+0", and "0-0" all evaluate to 0.
    Note that "00" is not a valid expression because the 0 has a leading zero. 

**Example 5:**

**Input:** num = "3456237490", target = 9191

**Output:** []

**Explanation:** There are no expressions that can be created from "3456237490" to evaluate to 9191. 

**Constraints:**

*   `1 <= num.length <= 10`
*   `num` consists of only digits.
*   <code>-2<sup>31</sup> <= target <= 2<sup>31</sup> - 1</code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S107")
public class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> res = new ArrayList<>();
        if (num.length() == 0 || Long.parseLong(num) > Integer.MAX_VALUE) {
            return res;
        }
        char[] list = num.toCharArray();
        dfs(res, list, 0, 0, target, new char[2 * list.length - 1], 0, 1, '+', 0);
        return res;
    }

    private void dfs(
            List<String> res,
            char[] list,
            int i,
            int j,
            int target,
            char[] arr,
            int val,
            int mul,
            char preOp,
            int join) {
        arr[j++] = list[i];
        int digit = 10 * join + (list[i] - '0');
        int result = val + mul * digit;
        if (preOp == '-') {
            result = val - mul * digit;
        }
        if (i == list.length - 1) {
            if (result == target) {
                StringBuilder sb = new StringBuilder();
                for (int k = 0; k < j; k++) {
                    sb.append(arr[k]);
                }
                res.add(sb.toString());
            }
            return;
        }
        if (preOp == '+') {
            arr[j] = '+';
            dfs(res, list, i + 1, j + 1, target, arr, val + mul * digit, 1, '+', 0);
            arr[j] = '-';
            dfs(res, list, i + 1, j + 1, target, arr, val + mul * digit, 1, '-', 0);
            arr[j] = '*';
            dfs(res, list, i + 1, j + 1, target, arr, val, mul * digit, '+', 0);
            if (digit != 0) {
                dfs(res, list, i + 1, j, target, arr, val, mul, '+', digit);
            }
        } else {
            arr[j] = '+';
            dfs(res, list, i + 1, j + 1, target, arr, val - mul * digit, 1, '+', 0);
            arr[j] = '-';
            dfs(res, list, i + 1, j + 1, target, arr, val - mul * digit, 1, '-', 0);
            arr[j] = '*';
            dfs(res, list, i + 1, j + 1, target, arr, val, mul * digit, '-', 0);
            if (digit != 0) {
                dfs(res, list, i + 1, j, target, arr, val, mul, '-', digit);
            }
        }
    }
}
```