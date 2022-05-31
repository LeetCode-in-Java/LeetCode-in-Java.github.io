## 1287\. Element Appearing More Than 25% In Sorted Array

Easy

Given an integer array **sorted** in non-decreasing order, there is exactly one integer in the array that occurs more than 25% of the time, return that integer.

**Example 1:**

**Input:** arr = [1,2,2,6,6,6,6,7,10]

**Output:** 6

**Example 2:**

**Input:** arr = [1,1]

**Output:** 1

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>0 <= arr[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int findSpecialInteger(int[] arr) {
        int quarter = arr.length / 4;
        for (int i = 0; i < arr.length - quarter; i++) {
            if (arr[i] == arr[i + quarter]) {
                return arr[i];
            }
        }
        return -1;
    }
}
```