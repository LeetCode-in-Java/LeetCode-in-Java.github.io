## 350\. Intersection of Two Arrays II

Easy

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]

**Output:** [2,2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]

**Output:** [4,9]

**Explanation:** [9,4] is also accepted.

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   `0 <= nums1[i], nums2[i] <= 1000`

**Follow up:**

*   What if the given array is already sorted? How would you optimize your algorithm?
*   What if `nums1`'s size is small compared to `nums2`'s size? Which algorithm is better?
*   What if elements of `nums2` are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        // First sort the array
        Arrays.sort(nums1);
        // Similar this one as well
        Arrays.sort(nums2);
        // "i" will point at nums1
        int i = 0;
        // "j" will point at nums2
        int j = 0;
        // "k" will point at nums1 and helps in getting the intersection answer;
        int k = 0;
        // Loop will run until "i" & "j" doesn't reach the array boundary;
        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] < nums2[j]) {
                // Check if nums1 value is less then nums2 value;
                // Increment "i"
                i++;
            } else if (nums1[i] > nums2[j]) {
                // Check if nums2 value is less then nums1 value;
                // Increment "j"
                j++;

            } else {
                // Check if nums1 value is equals to nums2 value;
                // Dump into nums1 and increment k, increment i & increment j as well;
                nums1[k++] = nums1[i++];
                j++;
            }
        }
        // Only return nums1 0th index to kth index value, because that's will be our intersection;
        return Arrays.copyOfRange(nums1, 0, k);
    }
}
```