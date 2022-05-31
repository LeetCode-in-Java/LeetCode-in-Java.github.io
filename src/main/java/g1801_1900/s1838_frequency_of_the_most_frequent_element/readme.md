## 1838\. Frequency of the Most Frequent Element

Medium

The **frequency** of an element is the number of times it occurs in an array.

You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.

Return _the **maximum possible frequency** of an element after performing **at most**_ `k` _operations_.

**Example 1:**

**Input:** nums = [1,2,4], k = 5

**Output:** 3

**Explanation:** Increment the first element three times and the second element two times to make nums = [4,4,4]. 4 has a frequency of 3.

**Example 2:**

**Input:** nums = [1,4,8,13], k = 5

**Output:** 2

**Explanation:** There are multiple optimal solutions: 

- Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.

- Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2. 

- Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.

**Example 3:**

**Input:** nums = [3,9,6], k = 2

**Output:** 1

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maxFrequency(int[] nums, int k) {
        countingSort(nums);
        int start = 0;
        int preSum = 0;
        int total = 1;
        for (int i = 0; i < nums.length; i++) {
            int length = i - start + 1;
            int product = nums[i] * length;
            preSum += nums[i];
            while (product - preSum > k) {
                preSum -= nums[start++];
                length--;
                product = nums[i] * length;
            }
            total = Math.max(total, length);
        }

        return total;
    }

    private void countingSort(int[] nums) {
        int max = Integer.MIN_VALUE;
        for (int num : nums) {
            max = Math.max(max, num);
        }
        int[] map = new int[max + 1];
        for (int num : nums) {
            map[num]++;
        }
        int i = 0;
        int j = 0;
        while (i <= max) {
            if (map[i]-- > 0) {
                nums[j++] = i;
            } else {
                i++;
            }
        }
    }
}
```