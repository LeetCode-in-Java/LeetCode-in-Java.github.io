## 1995\. Count Special Quadruplets

Easy

Given a **0-indexed** integer array `nums`, return _the number of **distinct** quadruplets_ `(a, b, c, d)` _such that:_

*   `nums[a] + nums[b] + nums[c] == nums[d]`, and
*   `a < b < c < d`

**Example 1:**

**Input:** nums = [1,2,3,6]

**Output:** 1

**Explanation:** The only quadruplet that satisfies the requirement is (0, 1, 2, 3) because 1 + 2 + 3 == 6. 

**Example 2:**

**Input:** nums = [3,3,6,4,5]

**Output:** 0

**Explanation:** There are no such quadruplets in [3,3,6,4,5]. 

**Example 3:**

**Input:** nums = [1,1,1,3,5]

**Output:** 4

**Explanation:** The 4 quadruplets that satisfy the requirement are:

- (0, 1, 2, 3): 1 + 1 + 1 == 3

- (0, 1, 3, 4): 1 + 1 + 3 == 5

- (0, 2, 3, 4): 1 + 1 + 3 == 5

- (1, 2, 3, 4): 1 + 1 + 3 == 5 

**Constraints:**

*   `4 <= nums.length <= 50`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int countQuadruplets(int[] nums) {
        int count = 0;
        // max nums value is 100 so two elements sum can be max 200
        int[] m = new int[201];
        for (int i = 1; i < nums.length - 2; i++) {
            for (int j = 0; j < i; j++) {
                // update all possible 2 sums
                m[nums[j] + nums[i]]++;
            }
            for (int j = i + 2; j < nums.length; j++) {
                // fix third element and search for fourth - third in 2 sums as a  + b + c = d == a
                // +  b = d - c
                int diff = nums[j] - nums[i + 1];
                if (diff >= 0) {
                    count += m[diff];
                }
            }
        }
        return count;
    }
}
```