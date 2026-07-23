[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3830\. Longest Alternating Subarray After Removing At Most One Element

Hard

You are given an integer array `nums`.

A subarray `nums[l..r]` is **alternating** if one of the following holds:

*   `nums[l] < nums[l + 1] > nums[l + 2] < nums[l + 3] > ...`
*   `nums[l] > nums[l + 1] < nums[l + 2] > nums[l + 3] < ...`

In other words, if we compare adjacent elements in the subarray, then the comparisons alternate between **strictly** greater and **strictly** smaller.

You can remove **at most one** element from `nums`. Then, you select an alternating subarray from `nums`.

Return an integer denoting the **maximum** **length** of the alternating subarray you can select.

A subarray of length 1 is considered alternating.

**Example 1:**

**Input:** nums = [2,1,3,2]

**Output:** 4

**Explanation:**

*   Choose not to remove elements.
*   Select the entire array <code>[<ins>**2, 1, 3, 2**</ins>]</code>, which is alternating because `2 > 1 < 3 > 2`.

**Example 2:**

**Input:** nums = [3,2,1,2,3,2,1]

**Output:** 4

**Explanation:**

*   Choose to remove `nums[3]` i.e., <code>[3, 2, 1, <ins>**2**</ins>, 3, 2, 1]</code>. The array becomes `[3, 2, 1, 3, 2, 1]`.
*   Select the subarray <code>[3, **<ins>2, 1, 3, 2</ins>**, 1]</code>.

**Example 3:**

**Input:** nums = [100000,100000]

**Output:** 1

**Explanation:**

*   Choose not to remove elements.
*   Select the subarray <code>[100000, <ins>**100000**</ins>]</code>.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {

    public int longestAlternating(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }
        State state = new State();
        while (state.right < nums.length) {
            int direction = Integer.compare(nums[state.right], nums[state.right - 1]);
            processDirection(state, direction);
        }
        return state.maxLength;
    }

    private void processDirection(State state, int direction) {
        if (direction == 0) {
            processEqualValues(state);
            return;
        }
        boolean currentUp = direction > 0;
        if (isRepeatedDirection(state, currentUp)) {
            processRepeatedDirection(state);
        } else {
            processAlternatingDirection(state);
        }
        state.up = currentUp;
    }

    private boolean isRepeatedDirection(State state, boolean currentUp) {
        return state.up != null && state.up == currentUp;
    }

    private void processRepeatedDirection(State state) {
        if (state.used) {
            resetToUsedIndex(state);
            return;
        }
        state.used = true;
        state.usedIndex = state.right - 1;
        updateMax(state, state.right - state.left);
        state.right++;
    }

    private void processAlternatingDirection(State state) {
        int length = state.right - state.left;
        if (!state.used) {
            length++;
        }
        updateMax(state, length);
        state.right++;
    }

    private void processEqualValues(State state) {
        if (state.used) {
            resetToUsedIndex(state);
            return;
        }
        state.used = true;
        state.usedIndex = state.right;
        updateMax(state, state.right - state.left);
        state.right++;
    }

    private void resetToUsedIndex(State state) {
        state.used = false;
        state.left = state.usedIndex;
    }

    private void updateMax(State state, int length) {
        state.maxLength = Math.max(state.maxLength, length);
    }

    private static class State {
        private int left;
        private int right = 1;
        private Boolean up;
        private boolean used;
        private int usedIndex;
        private int maxLength;
    }
}
```