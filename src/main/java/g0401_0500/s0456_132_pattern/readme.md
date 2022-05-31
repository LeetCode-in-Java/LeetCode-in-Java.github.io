## 456\. 132 Pattern

Medium

Given an array of `n` integers `nums`, a **132 pattern** is a subsequence of three integers `nums[i]`, `nums[j]` and `nums[k]` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

Return _`true` if there is a **132 pattern** in `nums`, otherwise, return `false`._

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** false

**Explanation:** There is no 132 pattern in the sequence.

**Example 2:**

**Input:** nums = [3,1,4,2]

**Output:** true

**Explanation:** There is a 132 pattern in the sequence: [1, 4, 2].

**Example 3:**

**Input:** nums = [-1,3,2,0]

**Output:** true

**Explanation:** There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 2 * 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    /*
     * It scans only once, this is the power of using correct data structure.
     * It goes from the right to the left.
     * It keeps pushing elements into the stack,
     * but it also keeps poping elements out of the stack as long as the current element is bigger than this number.
     */
    public boolean find132pattern(int[] nums) {
        Deque<Integer> stack = new LinkedList<>();
        int s3 = Integer.MIN_VALUE;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[i] < s3) {
                return true;
            } else {
                while (!stack.isEmpty() && nums[i] > stack.peek()) {
                    s3 = Math.max(s3, stack.pop());
                }
            }
            stack.push(nums[i]);
        }
        return false;
    }
}
```