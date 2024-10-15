[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3309\. Maximum Possible Number by Binary Concatenation

Medium

You are given an array of integers `nums` of size 3.

Return the **maximum** possible number whose _binary representation_ can be formed by **concatenating** the _binary representation_ of **all** elements in `nums` in some order.

**Note** that the binary representation of any number _does not_ contain leading zeros.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 30

**Explanation:**

Concatenate the numbers in the order `[3, 1, 2]` to get the result `"11110"`, which is the binary representation of 30.

**Example 2:**

**Input:** nums = [2,8,16]

**Output:** 1296

**Explanation:**

Concatenate the numbers in the order `[2, 8, 16]` to get the result `"10100010000"`, which is the binary representation of 1296.

**Constraints:**

*   `nums.length == 3`
*   `1 <= nums[i] <= 127`

## Solution

```java
public class Solution {
    private String result = "0";

    public int maxGoodNumber(int[] nums) {
        boolean[] visited = new boolean[nums.length];
        StringBuilder sb = new StringBuilder();
        solve(nums, visited, 0, sb);
        int score = 0;
        int val;
        for (char c : result.toCharArray()) {
            val = c - '0';
            score *= 2;
            score += val;
        }
        return score;
    }

    private void solve(int[] nums, boolean[] visited, int pos, StringBuilder sb) {
        if (pos == nums.length) {
            String val = sb.toString();
            if ((result.length() == val.length() && result.compareTo(val) < 0)
                    || val.length() > result.length()) {
                result = val;
            }
            return;
        }
        String cur;
        for (int i = 0; i < nums.length; ++i) {
            if (visited[i]) {
                continue;
            }
            visited[i] = true;
            cur = Integer.toBinaryString(nums[i]);
            sb.append(cur);
            solve(nums, visited, pos + 1, sb);
            sb.setLength(sb.length() - cur.length());
            visited[i] = false;
        }
    }
}
```