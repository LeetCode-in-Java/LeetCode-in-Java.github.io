## 448\. Find All Numbers Disappeared in an Array

Easy

Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return _an array of all the integers in the range_ `[1, n]` _that do not appear in_ `nums`.

**Example 1:**

**Input:** nums = [4,3,2,7,8,2,3,1]

**Output:** [5,6] 

**Example 2:**

**Input:** nums = [1,1]

**Output:** [2] 

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= n`

**Follow up:** Could you do it without extra space and in `O(n)` runtime? You may assume the returned list does not count as extra space.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int[] arr = new int[nums.length];
        int counter = 0;
        for (int i = 0; i < nums.length; i++) {
            arr[nums[counter] - 1] = 1;
            counter++;
        }
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == 0) {
                res.add(i + 1);
            }
        }
        return res;
    }
}
```