[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 55\. Jump Game

Medium

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [2,3,1,1,4]

**Output:** true

**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index. 

**Example 2:**

**Input:** nums = [3,2,1,0,4]

**Output:** false

**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public boolean canJump(int[] nums) {
        int sz = nums.length;
        // we set 1 so it won't break on the first iteration
        int tmp = 1;
        for (int i = 0; i < sz; i++) {
            // we always deduct tmp for every iteration
            tmp--;
            if (tmp < 0) {
                // if from previous iteration tmp is already 0, it will be <0 here
                // leading to false value
                return false;
            }
            // we get the maximum value because this value is supposed
            // to be our iterator, if both values are 0, then the next
            // iteration we will return false
            // if either both or one of them are not 0 then we will keep doing this and check.

            // We can stop the whole iteration with this condition. without this condition the code
            // runs in 2ms 79.6%, adding this condition improves the performance into 1ms 100%
            // because if the test case jump value is quite large, instead of just iterate, we can
            // just check using this condition
            // example: [10, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0] -> we can just jump to the end without
            // iterating whole array
            tmp = Math.max(tmp, nums[i]);
            if (i + tmp >= sz - 1) {
                return true;
            }
        }
        // we can just return true at the end, because if tmp is 0 on previous
        // iteration,
        // even though the next iteration index is the last one, it will return false under the
        // tmp<0 condition
        return true;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a single loop that iterates through the elements of the `nums` array. Let's assume there are 'n' elements in the array.

2. In each iteration of the loop, the program performs constant time operations such as subtraction, comparison, and taking the maximum value.

3. The loop iterates until the end of the array or until it determines that it's not possible to jump further.

4. In the worst case, the loop may iterate through all 'n' elements of the array.

5. Therefore, the time complexity of the program is O(n), where 'n' is the number of elements in the input array `nums`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array.

2. The program uses a few integer variables (`sz` and `tmp`) to keep track of the array size and the jump value. These variables require constant space.

3. The space complexity does not depend on the size of the input array `nums`.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where 'n' is the number of elements in the input array `nums`. The program efficiently checks if it's possible to jump to the end of the array using a single pass through the array.
