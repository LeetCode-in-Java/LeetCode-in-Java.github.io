[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 970\. Powerful Integers

Medium

Given three integers `x`, `y`, and `bound`, return _a list of all the **powerful integers** that have a value less than or equal to_ `bound`.

An integer is **powerful** if it can be represented as <code>x<sup>i</sup> + y<sup>j</sup></code> for some integers `i >= 0` and `j >= 0`.

You may return the answer in **any order**. In your answer, each value should occur **at most once**.

**Example 1:**

**Input:** x = 2, y = 3, bound = 10

**Output:** [2,3,4,5,7,9,10]

**Explanation:**

2 = 2<sup>0</sup> + 3<sup>0</sup>

3 = 2<sup>1</sup> + 3<sup>0</sup>

4 = 2<sup>0</sup> + 3<sup>1</sup>

5 = 2<sup>1</sup> + 3<sup>1</sup>

7 = 2<sup>2</sup> + 3<sup>1</sup>

9 = 2<sup>3</sup> + 3<sup>0</sup>

10 = 2<sup>0</sup> + 3<sup>2</sup>

**Example 2:**

**Input:** x = 3, y = 5, bound = 15

**Output:** [2,4,6,8,10,14]

**Constraints:**

*   `1 <= x, y <= 100`
*   <code>0 <= bound <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

public class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        int iBound = (x == 1 ? 1 : (int) (Math.log10(bound) / Math.log10(x)));
        int jBound = (y == 1 ? 1 : (int) (Math.log10(bound) / Math.log10(y)));
        HashSet<Integer> set = new HashSet<>();
        for (int i = 0; i <= iBound; i++) {
            for (int j = 0; j <= jBound; j++) {
                int number = (int) (Math.pow(x, i) + Math.pow(y, j));
                if (number <= bound) {
                    set.add(number);
                }
            }
        }
        return new ArrayList<>(set);
    }
}
```