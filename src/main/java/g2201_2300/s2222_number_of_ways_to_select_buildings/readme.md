[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2222\. Number of Ways to Select Buildings

Medium

You are given a **0-indexed** binary string `s` which represents the types of buildings along a street where:

*   `s[i] = '0'` denotes that the <code>i<sup>th</sup></code> building is an office and
*   `s[i] = '1'` denotes that the <code>i<sup>th</sup></code> building is a restaurant.

As a city official, you would like to **select** 3 buildings for random inspection. However, to ensure variety, **no two consecutive** buildings out of the **selected** buildings can be of the same type.

*   For example, given `s = "0**0**1**1**0**1**"`, we cannot select the <code>1<sup>st</sup></code>, <code>3<sup>rd</sup></code>, and <code>5<sup>th</sup></code> buildings as that would form `"0**11**"` which is **not** allowed due to having two consecutive buildings of the same type.

Return _the **number of valid ways** to select 3 buildings._

**Example 1:**

**Input:** s = "001101"

**Output:** 6

**Explanation:** The following sets of indices selected are valid: 

- \[0,2,4] from "**0**0**1**1**0**1" forms "010" 

- \[0,3,4] from "**0**01**10**1" forms "010" 

- \[1,2,4] from "0**01**1**0**1" forms "010" 

- \[1,3,4] from "0**0**1**10**1" forms "010" 

- \[2,4,5] from "00**1**1**01**" forms "101" 

- \[3,4,5] from "001**101**" forms "101" 
  
No other selection is valid. Thus, there are 6 total ways.

**Example 2:**

**Input:** s = "11100"

**Output:** 0

**Explanation:** It can be shown that there are no valid selections.

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public long numberOfWays(String s) {
        long z = 0;
        long o = 0;
        long zo = 0;
        long oz = 0;
        long zoz = 0;
        long ozo = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') {
                zoz += zo;
                oz += o;
                z++;
            } else {
                ozo += oz;
                zo += z;
                o++;
            }
        }
        return zoz + ozo;
    }
}
```