[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2178\. Maximum Split of Positive Even Integers

Medium

You are given an integer `finalSum`. Split it into a sum of a **maximum** number of **unique** positive even integers.

*   For example, given `finalSum = 12`, the following splits are **valid** (unique positive even integers summing up to `finalSum`): `(12)`, `(2 + 10)`, `(2 + 4 + 6)`, and `(4 + 8)`. Among them, `(2 + 4 + 6)` contains the maximum number of integers. Note that `finalSum` cannot be split into `(2 + 2 + 4 + 4)` as all the numbers should be unique.

Return _a list of integers that represent a valid split containing a **maximum** number of integers_. If no valid split exists for `finalSum`, return _an **empty** list_. You may return the integers in **any** order.

**Example 1:**

**Input:** finalSum = 12

**Output:** [2,4,6]

**Explanation:** The following are valid splits: `(12)`, `(2 + 10)`, `(2 + 4 + 6)`, and `(4 + 8)`.

(2 + 4 + 6) has the maximum number of integers, which is 3. Thus, we return [2,4,6].

Note that [2,6,4], [6,2,4], etc. are also accepted. 

**Example 2:**

**Input:** finalSum = 7

**Output:** []

**Explanation:** There are no valid splits for the given finalSum. Thus, we return an empty array. 

**Example 3:**

**Input:** finalSum = 28

**Output:** [6,8,2,12]

**Explanation:** The following are valid splits: `(2 + 26)`, `(6 + 8 + 2 + 12)`, and `(4 + 24)`.

`(6 + 8 + 2 + 12)` has the maximum number of integers, which is 4. Thus, we return [6,8,2,12].

Note that [10,2,4,12], [6,2,4,16], etc. are also accepted. 

**Constraints:**

*   <code>1 <= finalSum <= 10<sup>10</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Long> maximumEvenSplit(long finalSum) {
        long curr = 2;
        long remainingSum = finalSum;
        List<Long> result = new ArrayList<>();
        if (finalSum % 2 != 0) {
            return result;
        }
        while (remainingSum >= curr) {
            result.add(curr);
            remainingSum = remainingSum - curr;
            curr += 2;
        }
        /*
        go greedily by starting from smallest even number
        for target = 16 after the while loop
        remainingSum = 4
        curr = 8 (if we add 8 it exceeds the target 16)
        result = [2,4,6]
        so remove 6 from list and add it to remainigSum and insert to list
        result = [2,4,10]
        */
        long lastSum = result.get(result.size() - 1);
        result.remove(result.size() - 1);
        result.add(lastSum + remainingSum);
        return result;
    }
}
```