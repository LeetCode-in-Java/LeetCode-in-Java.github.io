## 169\. Majority Element

Easy

Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

**Input:** nums = [3,2,3]

**Output:** 3 

**Example 2:**

**Input:** nums = [2,2,1,1,1,2,2]

**Output:** 2 

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

## Solution

```java
public class Solution {
    public int majorityElement(int[] arr) {
        int count = 1;
        int majority = arr[0];
        // For Potential Majority Element
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] == majority) {
                count++;
            } else {
                if (count > 1) {
                    count--;
                } else {
                    majority = arr[i];
                }
            }
        }

        // For Confirmation
        count = 0;
        for (int j : arr) {
            if (j == majority) {
                count++;
            }
        }

        if (count >= (arr.length / 2) + 1) {
            return majority;
        } else {
            return -1;
        }
    }
}
```