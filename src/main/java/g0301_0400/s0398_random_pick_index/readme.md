## 398\. Random Pick Index

Medium

Given an integer array `nums` with possible **duplicates**, randomly output the index of a given `target` number. You can assume that the given target number must exist in the array.

Implement the `Solution` class:

*   `Solution(int[] nums)` Initializes the object with the array `nums`.
*   `int pick(int target)` Picks a random index `i` from `nums` where `nums[i] == target`. If there are multiple valid i's, then each index should have an equal probability of returning.

**Example 1:**

**Input** 

    ["Solution", "pick", "pick", "pick"] 
    [[[1, 2, 3, 3, 3]], [3], [1], [3]]

**Output:** [null, 4, 0, 2]

**Explanation:** 

    Solution solution = new Solution([1, 2, 3, 3, 3]); 
    solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning. 
    solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1. 
    solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   `target` is an integer from `nums`.
*   At most <code>10<sup>4</sup></code> calls will be made to `pick`.

## Solution

```java
/*
 * Using Reservoir Sampling
 *
 * Suppose the indexes of the target element in array are from 1 to N. You have
 * already picked i-1 elements. Now you are trying to pick ith element. The
 * probability to pick it is 1/i. Now you do not want to pick any future
 * numbers.. Thus, the final probability for ith element = 1/i * (1 - 1/(i+1)) *
 * (1 - 1/(i+2)) * .. * (1 - 1/N) = 1 / N.
 *
 * Time Complexity:
 * 1) Solution() Constructor -> O(1)
 * 2) pick() -> O(N)
 *
 * Space Complexity: O(1)
 *
 * N = Length of the input array.
 */

import java.util.Random;

@SuppressWarnings("java:S2245")
public class Solution {
    private final int[] nums;
    private final Random random;

    public Solution(int[] nums) {
        this.nums = nums;
        this.random = new Random();
    }

    public int pick(int target) {
        int idx = -1;
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                count++;
                if (random.nextInt(count) == 0) {
                    idx = i;
                }
            }
        }
        return idx;
    }
}
```