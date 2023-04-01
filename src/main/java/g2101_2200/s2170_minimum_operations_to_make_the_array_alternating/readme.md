[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2170\. Minimum Operations to Make the Array Alternating

Medium

You are given a **0-indexed** array `nums` consisting of `n` positive integers.

The array `nums` is called **alternating** if:

*   `nums[i - 2] == nums[i]`, where `2 <= i <= n - 1`.
*   `nums[i - 1] != nums[i]`, where `1 <= i <= n - 1`.

In one **operation**, you can choose an index `i` and **change** `nums[i]` into **any** positive integer.

Return _the **minimum number of operations** required to make the array alternating_.

**Example 1:**

**Input:** nums = [3,1,3,2,4,3]

**Output:** 3

**Explanation:**

One way to make the array alternating is by converting it to [3,1,3,**1**,**3**,**1**].

The number of operations required in this case is 3.

It can be proven that it is not possible to make the array alternating in less than 3 operations. 

**Example 2:**

**Input:** nums = [1,2,2,2,2]

**Output:** 2

**Explanation:**

One way to make the array alternating is by converting it to [1,2,**1**,2,**1**].

The number of operations required in this case is 2.

Note that the array cannot be converted to [**2**,2,2,2,2] because in this case nums[0] == nums[1] which violates the conditions of an alternating array. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minimumOperations(int[] nums) {
        int maxOdd = 0;
        int maxEven = 0;
        int max = 0;
        int n = nums.length;
        for (int num : nums) {
            max = Math.max(max, num);
        }
        int[] even = new int[max + 1];
        int[] odd = new int[max + 1];
        for (int i = 0; i < n; i++) {
            if (i % 2 == 0) {
                even[nums[i]]++;
            } else {
                odd[nums[i]]++;
            }
        }
        int t1 = 0;
        int t2 = 0;
        for (int i = 0; i < max + 1; i++) {
            if (even[i] > maxEven) {
                maxEven = even[i];
                t1 = i;
            }
            if (odd[i] > maxOdd) {
                maxOdd = odd[i];
                t2 = i;
            }
        }
        int ans;
        if (t1 == t2) {
            int secondEven = 0;
            int secondOdd = 0;
            for (int i = 0; i < max + 1; i++) {
                if (i != t1 && even[i] > secondEven) {
                    secondEven = even[i];
                }
                if (i != t2 && odd[i] > secondOdd) {
                    secondOdd = odd[i];
                }
            }
            ans =
                    Math.min(
                            (n / 2 + n % 2 - maxEven) + (n / 2 - secondOdd),
                            (n / 2 + n % 2 - secondEven) + (n / 2 - maxOdd));
        } else {
            ans = n / 2 + n % 2 - maxEven + n / 2 - maxOdd;
        }
        return ans;
    }
}
```