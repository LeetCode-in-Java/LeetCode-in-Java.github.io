[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3998\. Transform Binary String Using Subsequence Sort

Medium

You are given a binary string `s`.

You are also given an array of strings `strs`, where each `strs[i]` has the **same** length as `s` and consists of characters `'0'`, `'1'`, and `'?'`. Each `'?'` can be replaced by either `'0'` or `'1'`.

You may perform the following operation any number of times (including zero):

*   Choose any subsequence `sub` of `s`.
*   Sort `sub` in **non-decreasing** order.
*   Replace the chosen **subsequence** in `s` with the sorted `sub`, keeping all other characters unchanged.

Return a boolean array `ans`, where `ans[i]` is `true` if it's possible to replace all `'?'` in `strs[i]` with `'0'` or `'1'` and transform `s` into the resulting string using the allowed operation above, otherwise return `false`.

OriginalJava**Example 1:**

**Input:** s = "101", strs = ["1?1","0?1","0?0"]

**Output:** [true,true,false]

**Explanation:**

| `i` | `strs[i]` | Replacement | Result `strs[i]` | Operation(s) | Result |
| --- | --- | --- | --- | --- | --- |
| 0 | `"1?1"` | `? → 0` | `"101"` | Matches `s`. | `true` |
| 1 | `"0?1"` | `? → 1` | `"011"` | Select the subsequence at indices `[0..2]` of `s` → `"101"`. <br> Sort `"101"` to get `"011" = strs[i]`. | `true` |
| 2 | `"0?0"` | `? → 0` or `1` | `"000"` or `"010"` | Not feasible. | `false` |

Thus, `ans = [true, true, false]`.

**Example 2:**

**Input:** s = "1100", strs = ["0011","11?1","1?1?"]

**Output:** [true,false,true]

**Explanation:**

| `i` | `strs[i]` | Replacement | Result `strs[i]` | Operation(s) | Result |
| --- | --- | --- | --- | --- | --- |
| 0 | `"0011"` | \- | `"0011"` | Select the subsequence at indices `[0..3]` of `s` → `"1100"`. <br> Sort `"1100"` to get `"0011" = strs[i]`. | `true` |
| 1 | `"11?1"` | `? → 0` | `"1101"` | Not feasible. | `false` |
| 2 | `"1?1?"` | First `? → 0` <br> Second `? → 0` | `"1010"` | Select the subsequence at indices `[1, 2]` of `s` → `"10"`. <br> Sort `"10"` to get `"01"`, so <code>s = "1<ins>01</ins>0"</code>. | `true` |

Thus, `ans = [true, false, true]`.

**Example 3:**

**Input:** s = "1010", strs = ["0011"]

**Output:** [true]

**Explanation:**

| `i` | `strs[i]` | Replacement | Result `strs[i]` | Operation(s) | Result |
| --- | --- | --- | --- | --- | --- |
| 0 | `"0011"` | \- | `"0011"` | Select the subsequence at indices `[0, 2, 3]` of `s` → `"110"`. <br> Sort `"110"` to get `"011"`, so <code>s = "0<ins>0</ins>11" = strs[i]</code>. | `true` |

Thus, `ans = [true]`.

**Constraints:**

*   `1 <= n == s.length <= 2000`
*   `s[i]` is either `'0'` or `'1'`.
*   `1 <= strs.length <= 2000`
*   `strs[i].length == n`
*   `strs[i]` is either `'0'`, `'1'`, or `'?'`.

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CompletableFuture;

public class Solution {
    public boolean[] transformStr(String s, String[] strs) {
        int len = strs.length;
        int[] count = new int[2];
        for (char ch : s.toCharArray()) {
            count[ch - '0']++;
        }
        // create lists of all those tasks, which are no blocking main thread
        List<CompletableFuture<Boolean>> listOfFutureTasks = new ArrayList<>();
        for (String target : strs) {
            listOfFutureTasks.add(canConvert(s, target, count));
        }
        // allOf will make sure all things are running concurrently/parallely while .join() can be
        // used to complete em all
        CompletableFuture<Void> allDone =
                CompletableFuture.allOf(listOfFutureTasks.toArray(new CompletableFuture[0]));
        // collect results when allDone is complete
        boolean[] res = new boolean[len];
        allDone.thenRun(
                        () -> {
                            for (int i = 0; i < len; i++) {
                                res[i] = listOfFutureTasks.get(i).join();
                            }
                        })
                .join();
        return res;
    }

    private CompletableFuture<Boolean> canConvert(String src, String target, int[] count) {
        // first check there should be equal or less number of 1's and 0's
        return CompletableFuture.supplyAsync(() -> validateBalance(target, count))
                .thenApply(dst -> dst != null && isPossibleToConvert(src, dst));
    }

    private boolean isPossibleToConvert(String src, String dst) {
        int n = src.length();
        int zero = 0;
        for (int i = 0; i < n; i++) {
            if (src.charAt(i) == '0') {
                zero++;
            }
            if (dst.charAt(i) == '0') {
                zero--;
            }
            if (zero > 0) {
                return false;
            }
        }
        return true;
    }

    private String validateBalance(String s, int[] count) {
        int[] local = new int[2];
        for (char ch : s.toCharArray()) {
            if (ch == '?') {
                continue;
            }
            local[ch - '0']++;
        }
        if (local[0] > count[0] || local[1] > count[1]) {
            return null;
        }
        int diff0 = count[0] - local[0];
        int diff1 = count[1] - local[1];
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == '?') {
                if (diff0-- > 0) {
                    sb.append('0');
                } else if (diff1-- > 0) {
                    sb.append('1');
                }
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```