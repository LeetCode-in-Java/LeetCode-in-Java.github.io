[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 560\. Subarray Sum Equals K

Medium

Given an array of integers `nums` and an integer `k`, return _the total number of continuous subarrays whose sum equals to `k`_.

**Example 1:**

**Input:** nums = [1,1,1], k = 2

**Output:** 2 

**Example 2:**

**Input:** nums = [1,2,3], k = 3

**Output:** 2 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   <code>-10<sup>7</sup> <= k <= 10<sup>7</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        int tempSum = 0;
        int ret = 0;
        Map<Integer, Integer> sumCount = new HashMap<>();
        sumCount.put(0, 1);
        for (int i : nums) {
            tempSum += i;
            if (sumCount.containsKey(tempSum - k)) {
                ret += sumCount.get(tempSum - k);
            }
            if (sumCount.get(tempSum) != null) {
                sumCount.put(tempSum, sumCount.get(tempSum) + 1);
            } else {
                sumCount.put(tempSum, 1);
            }
        }
        return ret;
    }
}
```

**Time Complexity (Big O Time):**

The program uses a single pass through the input array `nums`, maintaining a running sum `tempSum`. Within the loop, it checks if `tempSum - k` exists in the `sumCount` map, and if it does, it increments the result variable `ret`. This loop iterates through the entire `nums` array once.

1. The loop iterates through the entire `nums` array, which has 'n' elements, where 'n' is the length of the input array.

2. Inside the loop, the operations involving the `sumCount` map, such as checking for a key's existence and updating its value, have an average time complexity of O(1) due to the use of a hash map.

Therefore, the overall time complexity of the program is O(n), where 'n' is the length of the input array `nums`. The dominant factor is the loop that iterates through the array.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional data structures used, including the `sumCount` map and a few integer variables.

1. The `sumCount` map stores at most 'n' unique sums from the input array `nums`, where 'n' is the length of the array. Therefore, the space used by the map is O(n) in the worst case.

2. The other integer variables (`tempSum` and `ret`) used to keep track of the current sum and the result require constant space, O(1).

Hence, the overall space complexity of the program is O(n), where 'n' is the length of the input array `nums`. The dominant factor in the space complexity is the `sumCount` map.

In summary, the provided program has a time complexity of O(n) and a space complexity of O(n), where 'n' is the length of the input array `nums`. It efficiently counts the number of subarrays with a sum equal to the target 'k' using a map to store intermediate sums.
