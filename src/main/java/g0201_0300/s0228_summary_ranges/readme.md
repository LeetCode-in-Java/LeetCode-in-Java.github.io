## 228\. Summary Ranges

Easy

You are given a **sorted unique** integer array `nums`.

Return _the **smallest sorted** list of ranges that **cover all the numbers in the array exactly**_. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.

Each range `[a,b]` in the list should be output as:

*   `"a->b"` if `a != b`
*   `"a"` if `a == b`

**Example 1:**

**Input:** nums = [0,1,2,4,5,7]

**Output:** ["0->2","4->5","7"]

**Explanation:** The ranges are: [0,2] --> "0->2" [4,5] --> "4->5" [7,7] --> "7" 

**Example 2:**

**Input:** nums = [0,2,3,4,6,8,9]

**Output:** ["0","2->4","6","8->9"]

**Explanation:** The ranges are: [0,0] --> "0" [2,4] --> "2->4" [6,6] --> "6" [8,9] --> "8->9" 

**Example 3:**

**Input:** nums = []

**Output:** [] 

**Example 4:**

**Input:** nums = [-1]

**Output:** ["-1"] 

**Example 5:**

**Input:** nums = [0]

**Output:** ["0"] 

**Constraints:**

*   `0 <= nums.length <= 20`
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   All the values of `nums` are **unique**.
*   `nums` is sorted in ascending order.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> ranges = new ArrayList<>();
        if (nums.length == 0) {
            return ranges;
        }
        // size of array
        int n = nums.length;
        // start of range
        int a = nums[0];
        // end of range
        int b = a;
        StringBuilder strB = new StringBuilder();
        for (int i = 1; i < n; i++) {
            // we need to make a decision if the next element
            // will expand the range
            // i starts at 1, not 0, because 1 is the next
            // candidate for expanding the range
            if (nums[i] != b + 1) {
                // only when our next element does not expand the range
                // do we add the range a->b to our list of ranges
                strB.append(a);
                if (a != b) {
                    strB.append("->").append(b);
                }
                ranges.add(strB.toString());
                // since nums[i] is not accounted for by our range a->b
                // because nums[i] is not b+1, we need to set a and b
                // to this new range start point of bigger than b+1
                // maybe it is b+2? b+3? b+4? all we know is it is not b+1
                a = nums[i];
                b = a;
                // Reset string builder
                strB.setLength(0);
            } else {
                // if the next element expands our range we do so
                b++;
            }
        }
        // the only range that is not accounted for at this point is the last range
        // if our a and b are not equal then we add the range accordingly
        strB.append(a);
        if (a != b) {
            strB.append("->").append(b);
        }
        ranges.add(strB.toString());
        return ranges;
    }
}
```