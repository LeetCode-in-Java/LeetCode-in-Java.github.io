## 1712\. Ways to Split Array Into Three Subarrays

Medium

A split of an integer array is **good** if:

*   The array is split into three **non-empty** contiguous subarrays - named `left`, `mid`, `right` respectively from left to right.
*   The sum of the elements in `left` is less than or equal to the sum of the elements in `mid`, and the sum of the elements in `mid` is less than or equal to the sum of the elements in `right`.

Given `nums`, an array of **non-negative** integers, return _the number of **good** ways to split_ `nums`. As the number may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,1,1]

**Output:** 1

**Explanation:** The only good way to split nums is [1] [1] [1].

**Example 2:**

**Input:** nums = [1,2,2,2,5,0]

**Output:** 3

**Explanation:** There are three good ways of splitting nums:

[1] [2] [2,2,5,0] 

[1] [2,2] [2,5,0] 

[1,2] [2,2] [5,0]

**Example 3:**

**Input:** nums = [3,2,1]

**Output:** 0

**Explanation:** There is no good way to split nums.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int waysToSplit(int[] nums) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        int cur = 0;
        long res = 0;
        int i = 0;
        int idx1 = 1;
        int sum1 = nums[0];
        int idx2 = 1;
        int sum2 = nums[0];
        while (i < nums.length) {
            cur += nums[i];
            int right = sum - cur;
            if (i == 0 || i == nums.length - 1) {
                i++;
                continue;
            }
            while (idx1 <= i && sum1 <= cur - sum1) {
                sum1 += nums[idx1++];
            }
            while (idx2 < idx1 && cur - sum2 > right) {
                sum2 += nums[idx2++];
            }
            if (idx1 > idx2) {
                res = (res + idx1 - idx2) % 1000_000_007;
            }
            i++;
        }
        return (int) res;
    }
}
```