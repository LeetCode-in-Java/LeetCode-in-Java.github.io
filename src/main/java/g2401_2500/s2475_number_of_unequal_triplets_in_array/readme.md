[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2475\. Number of Unequal Triplets in Array

Easy

You are given a **0-indexed** array of positive integers `nums`. Find the number of triplets `(i, j, k)` that meet the following conditions:

*   `0 <= i < j < k < nums.length`
*   `nums[i]`, `nums[j]`, and `nums[k]` are **pairwise distinct**.
    *   In other words, `nums[i] != nums[j]`, `nums[i] != nums[k]`, and `nums[j] != nums[k]`.

Return _the number of triplets that meet the conditions._

**Example 1:**

**Input:** nums = [4,4,2,4,3]

**Output:** 3

**Explanation:** The following triplets meet the conditions: 

- (0, 2, 4) because 4 != 2 != 3
- (1, 2, 4) because 4 != 2 != 3
- (2, 3, 4) because 2 != 4 != 3
Since there are 3 triplets, we return 3. 
Note that (2, 0, 4) is not a valid triplet because 2 > 0.

**Example 2:**

**Input:** nums = [1,1,1,1,1]

**Output:** 0

**Explanation:** No triplets meet the conditions so we return 0.

**Constraints:**

*   `3 <= nums.length <= 100`
*   `1 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    public int unequalTriplets(int[] nums) {
        int trips = 0;
        int pairs = 0;
        int[] count = new int[1001];
        for (int i = 0; i < nums.length; ++i) {
            trips += pairs - count[nums[i]] * (i - count[nums[i]]);
            pairs += i - count[nums[i]];
            count[nums[i]] += 1;
        }
        return trips;
    }
}
```