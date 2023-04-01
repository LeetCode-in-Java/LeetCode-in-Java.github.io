[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1394\. Find Lucky Integer in an Array

Easy

Given an array of integers `arr`, a **lucky integer** is an integer that has a frequency in the array equal to its value.

Return _the largest **lucky integer** in the array_. If there is no **lucky integer** return `-1`.

**Example 1:**

**Input:** arr = [2,2,3,4]

**Output:** 2

**Explanation:** The only lucky number in the array is 2 because frequency[2] == 2.

**Example 2:**

**Input:** arr = [1,2,2,3,3,3]

**Output:** 3

**Explanation:** 1, 2 and 3 are all lucky numbers, return the largest of them.

**Example 3:**

**Input:** arr = [2,2,2,3,3]

**Output:** -1

**Explanation:** There are no lucky numbers in the array.

**Constraints:**

*   `1 <= arr.length <= 500`
*   `1 <= arr[i] <= 500`

## Solution

```java
public class Solution {
    public int findLucky(int[] arr) {
        int[] numbers = new int[501];
        for (int j : arr) {
            numbers[j]++;
        }
        for (int i = 500; i > 0; i--) {
            if (i == numbers[i]) {
                return i;
            }
        }
        return -1;
    }
}
```