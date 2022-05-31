## 377\. Combination Sum IV

Medium

Given an array of **distinct** integers `nums` and a target integer `target`, return _the number of possible combinations that add up to_ `target`.

The test cases are generated so that the answer can fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [1,2,3], target = 4

**Output:** 7

**Explanation:**

    The possible combination ways are:
    (1, 1, 1, 1)
    (1, 1, 2)
    (1, 2, 1)
    (1, 3)
    (2, 1, 1)
    (2, 2)
    (3, 1)
    Note that different sequences are counted as different combinations.

**Example 2:**

**Input:** nums = [9], target = 3

**Output:** 0

**Constraints:**

*   `1 <= nums.length <= 200`
*   `1 <= nums[i] <= 1000`
*   All the elements of `nums` are **unique**.
*   `1 <= target <= 1000`

**Follow up:** What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

## Solution

```java
import java.util.Arrays;

public class Solution {
    int[] storage;

    public int combinationSum4(int[] nums, int target) {
        storage = new int[target + 1];
        Arrays.fill(storage, -1);
        return result(nums, target);
    }

    private int result(int[] nums, int target) {
        if (target < 0) {
            return 0;
        }
        if (target == 0) {
            return 1;
        }
        if (storage[target] != -1) {
            return storage[target];
        }
        int count = 0;
        for (int i : nums) {
            count += result(nums, target - i);
        }
        storage[target] = count;
        return count;
    }
}
```