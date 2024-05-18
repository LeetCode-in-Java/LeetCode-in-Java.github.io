[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 93\. Restore IP Addresses

Medium

A **valid IP address** consists of exactly four integers separated by single dots. Each integer is between `0` and `255` (**inclusive**) and cannot have leading zeros.

*   For example, `"0.1.2.201"` and `"192.168.1.1"` are **valid** IP addresses, but `"0.011.255.245"`, `"192.168.1.312"` and `"192.168@1.1"` are **invalid** IP addresses.

Given a string `s` containing only digits, return _all possible valid IP addresses that can be formed by inserting dots into_ `s`. You are **not** allowed to reorder or remove any digits in `s`. You may return the valid IP addresses in **any** order.

**Example 1:**

**Input:** s = "25525511135"

**Output:** ["255.255.11.135","255.255.111.35"] 

**Example 2:**

**Input:** s = "0000"

**Output:** ["0.0.0.0"] 

**Example 3:**

**Input:** s = "1111"

**Output:** ["1.1.1.1"] 

**Example 4:**

**Input:** s = "010010"

**Output:** ["0.10.0.10","0.100.1.0"] 

**Example 5:**

**Input:** s = "101023"

**Output:** ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"] 

**Constraints:**

*   `0 <= s.length <= 20`
*   `s` consists of digits only.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static final int SEG_COUNT = 4;
    private List<String> result = new ArrayList<>();
    private int[] segments = new int[SEG_COUNT];

    public List<String> restoreIpAddresses(String s) {
        dfs(s, 0, 0);
        return result;
    }

    public void dfs(String s, int segId, int segStart) {
        // find 4 segments and get to last index
        if (segId == SEG_COUNT) {
            if (segStart == s.length()) {
                StringBuilder addr = new StringBuilder();
                for (int i = 0; i < SEG_COUNT; i++) {
                    addr.append(segments[i]);
                    if (i != SEG_COUNT - 1) {
                        addr.append('.');
                    }
                }
                result.add(addr.toString());
            }
            return;
        }
        // last index and no 4 segments
        if (segStart == s.length()) {
            return;
        }
        // start with a zero
        if (s.charAt(segStart) == '0') {
            segments[segId] = 0;
            dfs(s, segId + 1, segStart + 1);
            return;
        }
        int addr = 0;
        for (int index = segStart; index < s.length(); index++) {
            addr = addr * 10 + s.charAt(index) - '0';
            if (addr >= 0 && addr <= 255) {
                segments[segId] = addr;
                dfs(s, segId + 1, index + 1);
            } else {
                break;
            }
        }
    }
}
```