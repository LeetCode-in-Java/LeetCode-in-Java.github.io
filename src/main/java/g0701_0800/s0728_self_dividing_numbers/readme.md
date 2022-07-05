[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 728\. Self Dividing Numbers

Easy

A **self-dividing number** is a number that is divisible by every digit it contains.

*   For example, `128` is **a self-dividing number** because `128 % 1 == 0`, `128 % 2 == 0`, and `128 % 8 == 0`.

A **self-dividing number** is not allowed to contain the digit zero.

Given two integers `left` and `right`, return _a list of all the **self-dividing numbers** in the range_ `[left, right]`.

**Example 1:**

**Input:** left = 1, right = 22

**Output:** [1,2,3,4,5,6,7,8,9,11,12,15,22]

**Example 2:**

**Input:** left = 47, right = 85

**Output:** [48,55,66,77]

**Constraints:**

*   <code>1 <= left <= right <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> selfDividingNumbers(final int left, final int right) {
        final List<Integer> list = new ArrayList<>();
        for (int i = left; i <= right; i++) {
            if (isSelfDividing(i)) {
                list.add(i);
            }
        }
        return list;
    }

    private boolean isSelfDividing(int value) {
        final int origin = value;
        while (value != 0) {
            final int digit = value % 10;
            value /= 10;
            if (digit == 0 || origin % digit != 0) {
                return false;
            }
        }
        return true;
    }
}
```