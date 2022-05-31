## 442\. Find All Duplicates in an Array

Medium

Given an integer array `nums` of length `n` where all the integers of `nums` are in the range `[1, n]` and each integer appears **once** or **twice**, return _an array of all the integers that appears **twice**_.

You must write an algorithm that runs in `O(n) `time and uses only constant extra space.

**Example 1:**

**Input:** nums = [4,3,2,7,8,2,3,1]

**Output:** [2,3] 

**Example 2:**

**Input:** nums = [1,1,2]

**Output:** [1] 

**Example 3:**

**Input:** nums = [1]

**Output:** [] 

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= n`
*   Each element in `nums` appears **once** or **twice**.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        int num = nums.length;
        int[] count = new int[num];
        List<Integer> list = new ArrayList<>();
        for (int j : nums) {
            // count number of times each number appears
            count[j - 1] += 1;
        }
        for (int i = 0; i < num; i++) {
            // if count of num is 2 add pos to list
            if (count[i] == 2) {
                list.add(i + 1);
            }
        }
        return list;
    }
}
```