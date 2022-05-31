## 1998\. GCD Sort of an Array

Hard

You are given an integer array `nums`, and you can perform the following operation **any** number of times on `nums`:

*   Swap the positions of two elements `nums[i]` and `nums[j]` if `gcd(nums[i], nums[j]) > 1` where `gcd(nums[i], nums[j])` is the **greatest common divisor** of `nums[i]` and `nums[j]`.

Return `true` _if it is possible to sort_ `nums` _in **non-decreasing** order using the above swap method, or_ `false` _otherwise._

**Example 1:**

**Input:** nums = [7,21,3]

**Output:** true

**Explanation:** We can sort [7,21,3] by performing the following operations:

- Swap 7 and 21 because gcd(7,21) = 7. nums = [**21**,**7**,3]

- Swap 21 and 3 because gcd(21,3) = 3. nums = [**3**,7,**21**] 

**Example 2:**

**Input:** nums = [5,2,6,2]

**Output:** false

**Explanation:** It is impossible to sort the array because 5 cannot be swapped with any other element. 

**Example 3:**

**Input:** nums = [10,5,9,3,15]

**Output:** true We can sort [10,5,9,3,15] by performing the following operations:

- Swap 10 and 15 because gcd(10,15) = 5. nums = [**15**,5,9,3,**10**]

- Swap 15 and 3 because gcd(15,3) = 3. nums = [**3**,5,9,**15**,10]

- Swap 10 and 15 because gcd(10,15) = 5. nums = [3,5,9,**10**,**15**] 

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>2 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean gcdSort(int[] nums) {
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        int len = nums.length;
        int max = sorted[len - 1];
        // grouping tree child(index)->parent(value), index==value is root
        int[] nodes = new int[max + 1];
        for (int j : nums) {
            nodes[j] = -1;
        }
        // value: <=0 not sieved, <0 leaf node, 0 or 1 not in nums, >1 grouped
        for (int p = 2; p <= max / 2; p++) {
            if (nodes[p] > 0) {
                // sieved so not a prime number.
                continue;
            }
            // p is now a prime number, set self as root.
            nodes[p] = p;
            int group = p;
            int num = p + p;
            while (num <= max) {
                int existing = nodes[num];
                if (existing < 0) {
                    // 1st hit, set group
                    nodes[num] = group;
                } else if (existing <= 1) {
                    // value doesn't exist in nums
                    nodes[num] = 1;
                } else if ((existing = root(nodes, existing)) < group) {
                    nodes[group] = existing;
                    group = existing;
                } else {
                    nodes[existing] = group;
                }
                num += p;
            }
        }
        for (int i = 0; i < len; i++) {
            if (root(nodes, nums[i]) != root(nodes, (sorted[i]))) {
                return false;
            }
        }
        return true;
    }

    private static int root(final int[] nodes, int num) {
        int group;
        while ((group = nodes[num]) > 0 && group != num) {
            num = group;
        }
        return num;
    }
}
```