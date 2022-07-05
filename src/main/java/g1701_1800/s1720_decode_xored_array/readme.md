[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1720\. Decode XORed Array

Easy

There is a **hidden** integer array `arr` that consists of `n` non-negative integers.

It was encoded into another integer array `encoded` of length `n - 1`, such that `encoded[i] = arr[i] XOR arr[i + 1]`. For example, if `arr = [1,0,2,1]`, then `encoded = [1,2,3]`.

You are given the `encoded` array. You are also given an integer `first`, that is the first element of `arr`, i.e. `arr[0]`.

Return _the original array_ `arr`. It can be proved that the answer exists and is unique.

**Example 1:**

**Input:** encoded = [1,2,3], first = 1

**Output:** [1,0,2,1]

**Explanation:** If arr = [1,0,2,1], then first = 1 and encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]

**Example 2:**

**Input:** encoded = [6,2,7,3], first = 4

**Output:** [4,2,0,7,4]

**Constraints:**

*   <code>2 <= n <= 10<sup>4</sup></code>
*   `encoded.length == n - 1`
*   <code>0 <= encoded[i] <= 10<sup>5</sup></code>
*   <code>0 <= first <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] decode(int[] encoded, int first) {
        int[] arr = new int[encoded.length + 1];
        arr[0] = first;
        for (int i = 0; i < encoded.length; i++) {
            arr[i + 1] = encoded[i] ^ arr[i];
        }
        return arr;
    }
}
```