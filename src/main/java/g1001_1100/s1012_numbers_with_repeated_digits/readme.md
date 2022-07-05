[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1012\. Numbers With Repeated Digits

Hard

Given an integer `n`, return _the number of positive integers in the range_ `[1, n]` _that have **at least one** repeated digit_.

**Example 1:**

**Input:** n = 20

**Output:** 1

**Explanation:** The only positive number (<= 20) with at least 1 repeated digit is 11.

**Example 2:**

**Input:** n = 100

**Output:** 10

**Explanation:** The positive numbers (<= 100) with atleast 1 repeated digit are 11, 22, 33, 44, 55, 66, 77, 88, 99, and 100.

**Example 3:**

**Input:** n = 1000

**Output:** 262

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashSet;

@SuppressWarnings("java:S2583")
public class Solution {
    private int noRepeatCount = 0;

    public int numDupDigitsAtMostN(int n) {
        int nStrLength = String.valueOf(n).length();
        int allNineLength;
        if (n < 0 || nStrLength < 2) {
            return 0;
        } else if (Math.pow(10, nStrLength) - 1 == n) {
            allNineLength = nStrLength;
        } else {
            allNineLength = nStrLength - 1;
        }
        for (int numberOfDigits = 1; numberOfDigits <= allNineLength; numberOfDigits++) {
            noRepeatCount += calcNumberOfNoRepeat(numberOfDigits);
        }
        if (Math.pow(10, nStrLength) - 1 > n) {
            int mutations = 10;
            HashSet<Integer> hs = new HashSet<>();
            for (int index1 = 0; index1 < nStrLength; index1++) {
                int noRepeatCountLocal = 0;
                hs.clear();
                for (int index2 = 0; index2 < nStrLength; index2++) {
                    int index2Digit =
                            (int)
                                    (n
                                            / Math.pow(
                                                    10, String.valueOf(n).length() - (index2 + 1.0))
                                            % 10);
                    if (index2 < index1) {
                        if (hs.contains(index2Digit)) {
                            noRepeatCountLocal = 0;
                            break;
                        } else {
                            hs.add(index2Digit);
                        }
                    } else if (index2 == index1) {
                        if (index2 == 0) {
                            noRepeatCountLocal = index2Digit - 1;
                        } else {
                            int inIndex2Range = 0;
                            for (int j : hs) {
                                if ((index2 < nStrLength - 1 && j <= index2Digit - 1)
                                        || (index2 == nStrLength - 1 && j <= index2Digit)) {
                                    inIndex2Range++;
                                }
                            }
                            if (index2 == nStrLength - 1) {
                                noRepeatCountLocal = index2Digit + 1 - inIndex2Range;
                            } else {
                                noRepeatCountLocal = index2Digit - inIndex2Range;
                            }
                        }
                    } else {
                        noRepeatCountLocal *= mutations - index2;
                    }
                }
                if (noRepeatCountLocal > 0) {
                    noRepeatCount += noRepeatCountLocal;
                }
            }
        }
        return n - noRepeatCount;
    }

    private int calcNumberOfNoRepeat(int numberOfDigits) {
        int repeatCount = 0;
        int mutations = 9;
        for (int i = 0; i < numberOfDigits; i++) {
            if (i == 0) {
                repeatCount = mutations;
            } else {
                repeatCount *= mutations--;
            }
        }
        return repeatCount;
    }
}
```