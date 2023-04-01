[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1726\. Tuple with Same Product

Medium

Given an array `nums` of **distinct** positive integers, return _the number of tuples_ `(a, b, c, d)` _such that_ `a * b = c * d` _where_ `a`_,_ `b`_,_ `c`_, and_ `d` _are elements of_ `nums`_, and_ `a != b != c != d`_._

**Example 1:**

**Input:** nums = [2,3,4,6]

**Output:** 8

**Explanation:** There are 8 valid tuples: 

(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3) 

(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)

**Example 2:**

**Input:** nums = [1,2,4,5,10]

**Output:** 16

**Explanation:** There are 16 valid tuples: 

(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2) 

(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1) 

(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,5,4) 

(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   All elements in `nums` are **distinct**.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int tupleSameProduct(int[] nums) {
        HashMap<Integer, Integer> ab = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                ab.put(nums[i] * nums[j], ab.getOrDefault(nums[i] * nums[j], 0) + 1);
            }
        }
        int count = 0;
        for (Map.Entry<Integer, Integer> entry : ab.entrySet()) {
            int val = entry.getValue();
            count = count + (val * (val - 1)) / 2;
        }
        return count * 8;
    }
}
```