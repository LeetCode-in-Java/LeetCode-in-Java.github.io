[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

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

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where n is the length of the input array `arr`. Here's why:

1. The first loop iterates through the entire array once (`for (int i = 1; i < arr.length; i++)`).
   - Inside this loop, there are constant-time operations (assignments and comparisons) for each element of the array.
   - Therefore, this part has a time complexity of O(n).

2. The second loop also iterates through the entire array once (`for (int j : arr)`).
   - Inside this loop, there are constant-time operations (comparisons) for each element of the array.
   - Therefore, this part also has a time complexity of O(n).

In summary, both loops together contribute to the linear time complexity O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of extra space. Regardless of the size of the input array, the program only uses a fixed number of variables (`count` and `majority`) to keep track of the majority element.

In summary, the time complexity is O(n), where n is the length of the input array, and the space complexity is O(1), indicating a constant amount of additional memory usage.
