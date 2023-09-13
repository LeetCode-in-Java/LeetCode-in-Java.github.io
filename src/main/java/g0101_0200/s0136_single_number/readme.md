[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 136\. Single Number

Easy

Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

**Input:** nums = [2,2,1]

**Output:** 1 

**Example 2:**

**Input:** nums = [4,1,2,1,2]

**Output:** 4 

**Example 3:**

**Input:** nums = [1]

**Output:** 1 

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-3 * 10<sup>4</sup> <= nums[i] <= 3 * 10<sup>4</sup></code>
*   Each element in the array appears twice except for one element which appears only once.

## Solution

```java
public class Solution {
    public int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums) {
            res ^= num;
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

The program uses a single loop that iterates through all the elements of the `nums` array. Within each iteration, it performs a constant-time operation (XOR) on the current element and the result variable `res`. Therefore, the time complexity of this program is O(N), where N is the number of elements in the `nums` array. In other words, it has a linear time complexity because it processes each element once.

**Space Complexity (Big O Space):**

The program has a very low space complexity because it uses a constant amount of additional memory that doesn't depend on the size of the input `nums` array. It only uses a single integer variable `res` to store the result. Therefore, the space complexity of this program is O(1), indicating constant space usage.

In summary, the program has a time complexity of O(N), where N is the number of elements in the input array `nums`, and a space complexity of O(1), indicating that it uses a fixed amount of memory regardless of the size of the input.
