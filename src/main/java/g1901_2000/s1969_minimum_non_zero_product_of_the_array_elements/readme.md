[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1969\. Minimum Non-Zero Product of the Array Elements

Medium

You are given a positive integer `p`. Consider an array `nums` (**1-indexed**) that consists of the integers in the **inclusive** range <code>[1, 2<sup>p</sup> - 1]</code> in their binary representations. You are allowed to do the following operation **any** number of times:

*   Choose two elements `x` and `y` from `nums`.
*   Choose a bit in `x` and swap it with its corresponding bit in `y`. Corresponding bit refers to the bit that is in the **same position** in the other integer.

For example, if `x = 1101` and `y = 0011`, after swapping the <code>2<sup>nd</sup></code> bit from the right, we have `x = 1111` and `y = 0001`.

Find the **minimum non-zero** product of `nums` after performing the above operation **any** number of times. Return _this product_ _**modulo**_ <code>10<sup>9</sup> + 7</code>.

**Note:** The answer should be the minimum product **before** the modulo operation is done.

**Example 1:**

**Input:** p = 1

**Output:** 1

**Explanation:** nums = [1]. There is only one element, so the product equals that element.

**Example 2:**

**Input:** p = 2

**Output:** 6

**Explanation:** nums = [01, 10, 11].

Any swap would either make the product 0 or stay the same. 

Thus, the array product of 1 \* 2 \* 3 = 6 is already minimized.

**Example 3:**

**Input:** p = 3

**Output:** 1512

**Explanation:** nums = [001, 010, 011, 100, 101, 110, 111] 

- In the first operation we can swap the leftmost bit of the second and fifth elements. 

    - The resulting array is [001, 110, 011, 100, 001, 110, 111]. 
  
- In the second operation we can swap the middle bit of the third and fourth elements. 

    - The resulting array is [001, 110, 001, 110, 001, 110, 111]. 
      
The array product is 1 \* 6 \* 1 \* 6 \* 1 \* 6 \* 7 = 1512, which is the minimum possible product.

**Constraints:**

*   `1 <= p <= 60`

## Solution

```java
public class Solution {
    public int minNonZeroProduct(int p) {
        int m = (int) (1e9 + 7);
        long n = ((1L << p) - 2) % m;
        long ans = n + 1;
        long cnt = (1L << (p - 1)) - 1;
        while (cnt > 0) {
            if ((cnt & 1) == 1) {
                ans = ans * n % m;
            }
            cnt >>= 1;
            n = n * n % m;
        }
        return (int) ans;
    }
}
```