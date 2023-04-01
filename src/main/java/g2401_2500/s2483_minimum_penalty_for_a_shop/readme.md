[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2483\. Minimum Penalty for a Shop

Medium

You are given the customer visit log of a shop represented by a **0-indexed** string `customers` consisting only of characters `'N'` and `'Y'`:

*   if the <code>i<sup>th</sup></code> character is `'Y'`, it means that customers come at the <code>i<sup>th</sup></code> hour
*   whereas `'N'` indicates that no customers come at the <code>i<sup>th</sup></code> hour.

If the shop closes at the <code>j<sup>th</sup></code> hour (`0 <= j <= n`), the **penalty** is calculated as follows:

*   For every hour when the shop is open and no customers come, the penalty increases by `1`.
*   For every hour when the shop is closed and customers come, the penalty increases by `1`.

Return _the **earliest** hour at which the shop must be closed to incur a **minimum** penalty._

**Note** that if a shop closes at the <code>j<sup>th</sup></code> hour, it means the shop is closed at the hour `j`.

**Example 1:**

**Input:** customers = "YYNY"

**Output:** 2

**Explanation:** 
- Closing the shop at the 0<sup>th</sup> hour incurs in 1+1+0+1 = 3 penalty. 
- Closing the shop at the 1<sup>st</sup> hour incurs in 0+1+0+1 = 2 penalty. 
- Closing the shop at the 2<sup>nd</sup> hour incurs in 0+0+0+1 = 1 penalty. 
- Closing the shop at the 3<sup>rd</sup> hour incurs in 0+0+1+1 = 2 penalty. 
- Closing the shop at the 4<sup>th</sup> hour incurs in 0+0+1+0 = 1 penalty. 

Closing the shop at 2<sup>nd</sup> or 4<sup>th</sup> hour gives a minimum penalty. Since 2 is earlier, the optimal closing time is 2.

**Example 2:**

**Input:** customers = "NNNNN"

**Output:** 0

**Explanation:** It is best to close the shop at the 0<sup>th</sup> hour as no customers arrive.

**Example 3:**

**Input:** customers = "YYYY"

**Output:** 4

**Explanation:** It is best to close the shop at the 4<sup>th</sup> hour as customers arrive at each hour.

**Constraints:**

*   <code>1 <= customers.length <= 10<sup>5</sup></code>
*   `customers` consists only of characters `'Y'` and `'N'`.

## Solution

```java
public class Solution {
    public int bestClosingTime(String customers) {
        int[] yes = new int[customers.length() + 1];
        int[] no = new int[customers.length() + 1];
        int count = 0;
        for (int i = customers.length() - 1; i >= 0; i--) {
            if (customers.charAt(i) == 'Y') {
                count++;
            }
            yes[i] = count;
        }
        count = 0;
        for (int i = 0; i < customers.length(); i++) {
            if (customers.charAt(i) == 'N') {
                count++;
            }
            no[i + 1] = count;
        }
        int min = Integer.MAX_VALUE;
        int res = 0;
        for (int i = 0; i < yes.length; i++) {
            int sum = yes[i] + no[i];
            if (min > sum) {
                min = sum;
                res = i;
            }
        }
        return res;
    }
}
```