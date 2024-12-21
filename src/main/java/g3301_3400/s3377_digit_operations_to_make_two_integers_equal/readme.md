[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3377\. Digit Operations to Make Two Integers Equal

Medium

You are given two integers `n` and `m` that consist of the **same** number of digits.

You can perform the following operations **any** number of times:

*   Choose **any** digit from `n` that is not 9 and **increase** it by 1.
*   Choose **any** digit from `n` that is not 0 and **decrease** it by 1.

The integer `n` must not be a **prime** number at any point, including its original value and after each operation.

The cost of a transformation is the sum of **all** values that `n` takes throughout the operations performed.

Return the **minimum** cost to transform `n` into `m`. If it is impossible, return -1.

A prime number is a natural number greater than 1 with only two factors, 1 and itself.

**Example 1:**

**Input:** n = 10, m = 12

**Output:** 85

**Explanation:**

We perform the following operations:

*   Increase the first digit, now <code>n = <ins>**2**</ins>0</code>.
*   Increase the second digit, now <code>n = 2**<ins>1</ins>**</code>.
*   Increase the second digit, now <code>n = 2**<ins>2</ins>**</code>.
*   Decrease the first digit, now <code>n = **<ins>1</ins>**2</code>.

**Example 2:**

**Input:** n = 4, m = 8

**Output:** \-1

**Explanation:**

It is impossible to make `n` equal to `m`.

**Example 3:**

**Input:** n = 6, m = 2

**Output:** \-1

**Explanation:**

Since 2 is already a prime, we can't make `n` equal to `m`.

**Constraints:**

*   <code>1 <= n, m < 10<sup>4</sup></code>
*   `n` and `m` consist of the same number of digits.

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public int minOperations(int n, int m) {
        int limit = 100000;
        boolean[] sieve = new boolean[limit + 1];
        boolean[] visited = new boolean[limit];
        Arrays.fill(sieve, true);
        sieve[0] = false;
        sieve[1] = false;
        for (int i = 2; i * i <= limit; i++) {
            if (sieve[i]) {
                for (int j = i * i; j <= limit; j += i) {
                    sieve[j] = false;
                }
            }
        }
        if (sieve[n]) {
            return -1;
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        visited[n] = true;
        pq.add(new int[] {n, n});
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int cost = current[0];
            int num = current[1];
            char[] temp = Integer.toString(num).toCharArray();
            if (num == m) {
                return cost;
            }
            for (int j = 0; j < temp.length; j++) {
                char old = temp[j];
                for (int i = -1; i <= 1; i++) {
                    int digit = old - '0';
                    if ((digit == 9 && i == 1) || (digit == 0 && i == -1)) {
                        continue;
                    }
                    temp[j] = (char) (i + digit + '0');
                    int newnum = Integer.parseInt(new String(temp));
                    if (!sieve[newnum] && !visited[newnum]) {
                        visited[newnum] = true;
                        pq.add(new int[] {cost + newnum, newnum});
                    }
                }
                temp[j] = old;
            }
        }
        return -1;
    }
}
```