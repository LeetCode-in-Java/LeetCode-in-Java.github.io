[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2064\. Minimized Maximum of Products Distributed to Any Store

Medium

You are given an integer `n` indicating there are `n` specialty retail stores. There are `m` product types of varying amounts, which are given as a **0-indexed** integer array `quantities`, where `quantities[i]` represents the number of products of the <code>i<sup>th</sup></code> product type.

You need to distribute **all products** to the retail stores following these rules:

*   A store can only be given **at most one product type** but can be given **any** amount of it.
*   After distribution, each store will have been given some number of products (possibly `0`). Let `x` represent the maximum number of products given to any store. You want `x` to be as small as possible, i.e., you want to **minimize** the **maximum** number of products that are given to any store.

Return _the minimum possible_ `x`.

**Example 1:**

**Input:** n = 6, quantities = [11,6]

**Output:** 3

**Explanation:** One optimal way is: 

- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3 

- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3 
  
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.

**Example 2:**

**Input:** n = 7, quantities = [15,10,10]

**Output:** 5

**Explanation:** One optimal way is: 

- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5 

- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5 

- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5 
  
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.

**Example 3:**

**Input:** n = 1, quantities = [100000]

**Output:** 100000

**Explanation:** The only optimal way is: 

- The 100000 products of type 0 are distributed to the only store.
  
The maximum number of products given to any store is max(100000) = 100000.

**Constraints:**

*   `m == quantities.length`
*   <code>1 <= m <= n <= 10<sup>5</sup></code>
*   <code>1 <= quantities[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minimizedMaximum(int n, int[] q) {
        int min = 1;
        int max = maxi(q);
        int ans = 0;
        while (min <= max) {
            int mid = min + (max - min) / 2;
            if (condition(q, mid, n)) {
                ans = mid;
                max = mid - 1;
            } else {
                min = mid + 1;
            }
        }
        return ans;
    }

    private boolean condition(int[] arr, int mid, int n) {
        int ans = 0;
        for (int num : arr) {
            ans += (num) / mid;
            if (num % mid != 0) {
                ans++;
            }
        }
        return ans <= n;
    }

    private int maxi(int[] arr) {
        int ans = 0;
        for (int n : arr) {
            ans = Math.max(ans, n);
        }
        return ans;
    }
}
```