[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3191\. Minimum Operations to Make Binary Array Elements Equal to One I

Medium

You are given a binary array `nums`.

You can do the following operation on the array **any** number of times (possibly zero):

*   Choose **any** 3 **consecutive** elements from the array and **flip** **all** of them.

**Flipping** an element means changing its value from 0 to 1, and from 1 to 0.

Return the **minimum** number of operations required to make all elements in `nums` equal to 1. If it is impossible, return -1.

**Example 1:**

**Input:** nums = [0,1,1,1,0,0]

**Output:** 3

**Explanation:**   
 We can do the following operations:

*   Choose the elements at indices 0, 1 and 2. The resulting array is <code>nums = [<ins>**1**</ins>,<ins>**0**</ins>,<ins>**0**</ins>,1,0,0]</code>.
*   Choose the elements at indices 1, 2 and 3. The resulting array is <code>nums = [1,<ins>**1**</ins>,<ins>**1**</ins>,**<ins>0</ins>**,0,0]</code>.
*   Choose the elements at indices 3, 4 and 5. The resulting array is <code>nums = [1,1,1,**<ins>1</ins>**,<ins>**1**</ins>,<ins>**1**</ins>]</code>.

**Example 2:**

**Input:** nums = [0,1,1,1]

**Output:** \-1

**Explanation:**   
 It is impossible to make all elements equal to 1.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   `0 <= nums[i] <= 1`

## Solution

```java
public class Solution {
    public int minOperations(int[] nums) {
        int ans = 0;
        // Iterate through the array up to the third-last element
        for (int i = 0; i < nums.length - 2; i++) {
            // If the current element is 0, perform an operation
            if (nums[i] == 0) {
                ans++;
                // Flip the current element and the next two elements
                nums[i] = 1;
                nums[i + 1] = nums[i + 1] == 0 ? 1 : 0;
                nums[i + 2] = nums[i + 2] == 0 ? 1 : 0;
            }
        }
        // Check the last two elements if they are 0, return -1 as they cannot be flipped
        for (int i = nums.length - 2; i < nums.length; i++) {
            if (nums[i] == 0) {
                return -1;
            }
        }
        return ans;
    }
}
```