[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 457\. Circular Array Loop

Medium

You are playing a game involving a **circular** array of non-zero integers `nums`. Each `nums[i]` denotes the number of indices forward/backward you must move if you are located at index `i`:

*   If `nums[i]` is positive, move `nums[i]` steps **forward**, and
*   If `nums[i]` is negative, move `nums[i]` steps **backward**.

Since the array is **circular**, you may assume that moving forward from the last element puts you on the first element, and moving backwards from the first element puts you on the last element.

A **cycle** in the array consists of a sequence of indices `seq` of length `k` where:

*   Following the movement rules above results in the repeating index sequence `seq[0] -> seq[1] -> ... -> seq[k - 1] -> seq[0] -> ...`
*   Every `nums[seq[j]]` is either **all positive** or **all negative**.
*   `k > 1`

Return `true` _if there is a **cycle** in_ `nums`_, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [2,-1,1,2,2]

**Output:** true

**Explanation:** There is a cycle from index 0 -> 2 -> 3 -> 0 -> ... The cycle's length is 3.

**Example 2:**

**Input:** nums = [-1,2]

**Output:** false

**Explanation:** The sequence from index 1 -> 1 -> 1 -> ... is not a cycle because the sequence's length is 1. By definition the sequence's length must be strictly greater than 1 to be a cycle.

**Example 3:**

**Input:** nums = [-2,1,-1,-2,-2]

**Output:** false

**Explanation:** The sequence from index 1 -> 2 -> 1 -> ... is not a cycle because nums[1] is positive, but nums[2] is negative. Every nums[seq[j]] must be either all positive or all negative.

**Constraints:**

*   `1 <= nums.length <= 5000`
*   `-1000 <= nums[i] <= 1000`
*   `nums[i] != 0`

**Follow up:** Could you solve it in `O(n)` time complexity and `O(1)` extra space complexity?

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public boolean circularArrayLoop(int[] arr) {
        int n = arr.length;
        Map<Integer, Integer> map = new HashMap<>();
        // arr[i]%n==0 (cycle)
        for (int start = 0; start < n; start++) {
            if (map.containsKey(start)) {
                // check if already visited
                continue;
            }
            int curr = start;
            // Check if the current index is valid
            while (isValidCycle(start, curr, arr)) {
                // marking current index visited and set value as start of loop
                map.put(curr, start);
                // steps to jump;
                int jump = arr[curr] % n;
                // Jumping x steps backwards is same as jumping (n-x) steps forward
                // going to next index;
                curr = (curr + jump + n) % n;
                if (map.containsKey(curr)) {
                    // value already processed
                    if (map.get(curr) == start) {
                        // If equal to start then we have found a loop
                        return true;
                    }
                    // Else we can break since this has already been visited hence we will get the
                    // same result as before
                    break;
                }
            }
        }
        return false;
    }

    private boolean isValidCycle(int start, int curr, int[] arr) {
        return (arr[start] <= 0 || arr[curr] >= 0)
                && (arr[start] >= 0 || arr[curr] <= 0)
                && (arr[curr] % arr.length != 0);
    }
}
```