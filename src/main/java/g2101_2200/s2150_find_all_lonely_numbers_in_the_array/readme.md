[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2150\. Find All Lonely Numbers in the Array

Medium

You are given an integer array `nums`. A number `x` is **lonely** when it appears only **once**, and no **adjacent** numbers (i.e. `x + 1` and `x - 1)` appear in the array.

Return _**all** lonely numbers in_ `nums`. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [10,6,5,8]

**Output:** [10,8]

**Explanation:**

- 10 is a lonely number since it appears exactly once and 9 and 11 does not appear in nums. 

- 8 is a lonely number since it appears exactly once and 7 and 9 does not appear in nums. 

- 5 is not a lonely number since 6 appears in nums and vice versa. 

Hence, the lonely numbers in nums are [10, 8]. 

Note that [8, 10] may also be returned. 

**Example 2:**

**Input:** nums = [1,3,5,3]

**Output:** [1,5]

**Explanation:**

- 1 is a lonely number since it appears exactly once and 0 and 2 does not appear in nums. 

- 5 is a lonely number since it appears exactly once and 4 and 6 does not appear in nums. 

- 3 is not a lonely number since it appears twice. 

Hence, the lonely numbers in nums are [1, 5]. 

Note that [5, 1] may also be returned. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class Solution {
    public List<Integer> findLonely(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        HashMap<Integer, Integer> m = new HashMap<>();
        for (int i : nums) {
            m.put(i, m.getOrDefault(i, 0) + 1);
        }
        for (int i : nums) {
            if (m.get(i) == 1 && !m.containsKey(i - 1) && !m.containsKey(i + 1)) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```