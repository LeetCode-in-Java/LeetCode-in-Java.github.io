[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 274\. H-Index

Medium

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their <code>i<sup>th</sup></code> paper, return compute the researcher's `h`**\-index**.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): A scientist has an index `h` if `h` of their `n` papers have at least `h` citations each, and the other `n − h` papers have no more than `h` citations each.

If there are several possible values for `h`, the maximum one is taken as the `h`**\-index**.

**Example 1:**

**Input:** citations = [3,0,6,1,5]

**Output:** 3

**Explanation:**

    [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
    Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3. 

**Example 2:**

**Input:** citations = [1,3,1]

**Output:** 1 

**Constraints:**

*   `n == citations.length`
*   `1 <= n <= 5000`
*   `0 <= citations[i] <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int hIndex(int[] citations) {
        // Sort array then traverse from end keep track of counter denoting total elements seen so
        // far
        Arrays.sort(citations);
        int count = 0;
        int hIndex = 0;
        for (int i = citations.length - 1; i >= 0; i--) {
            if (i == citations.length - 1 && count == citations[i]) {
                hIndex = citations[i];
                return hIndex;
                //  Ex:- 7 10--> counter =8
            } else if (citations[i] <= count && count < citations[i + 1]) {
                hIndex = count;
                return hIndex;
                // Ex:- 7 9 --> counter 6 (incuding 7 there willbe 7 elements)
            } else if (citations[i] == count + 1) {
                hIndex = count + 1;
                return hIndex;
            } else {
                count++;
            }
        }
        // case when no element is hindex so far
        if (count < citations[0]) {
            hIndex = count;
        }
        return hIndex;
    }
}
```