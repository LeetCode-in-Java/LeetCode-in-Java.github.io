[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 128\. Longest Consecutive Sequence

Medium

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]

**Output:** 4

**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4. 

**Example 2:**

**Input:** nums = [0,3,7,2,5,8,4,6,0,1]

**Output:** 9 

**Constraints:**

*   <code>0 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S135")
public class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int max = Integer.MIN_VALUE;
        int thsMax = 1;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i + 1] == nums[i] + 1) {
                thsMax += 1;
                continue;
            }
            if (nums[i + 1] == nums[i]) {
                continue;
            }
            // Start of a new Sequene
            max = Math.max(max, thsMax);
            thsMax = 1;
        }
        return Math.max(max, thsMax);
    }
}
```

**Time Complexity (Big O Time):**

1. The program begins by checking if the input array `nums` is empty. This operation is done in constant time and does not depend on the size of the array. Therefore, it can be considered O(1).

2. The program then sorts the `nums` array using `Arrays.sort()`. The time complexity of the sorting operation is O(N log N), where N is the number of elements in the array. This is the most time-consuming operation in the program.

3. After sorting, the program iterates through the sorted array once, comparing adjacent elements. The loop runs from `i = 0` to `i < nums.length - 1`. In each iteration, it checks if `nums[i + 1]` is consecutive to `nums[i]` and updates the `thsMax` variable accordingly. This loop is a linear pass over the sorted array and runs in O(N) time.

4. The program uses a few constant-time operations such as comparisons and updates within the loop.

5. Finally, the program returns the maximum of `max` and `thsMax`. This operation is done in constant time.

Therefore, the overall time complexity of the program is dominated by the sorting step, which is O(N log N) due to the use of `Arrays.sort()`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the space required for the input `nums` array and a few integer variables (`max` and `thsMax`) that occupy constant space regardless of the input size. Therefore, the space complexity is O(1), as it does not depend on the size of the input array.

In summary, the time complexity of the program is O(N log N) due to the sorting operation, and the space complexity is O(1) because it uses only a constant amount of additional space.
