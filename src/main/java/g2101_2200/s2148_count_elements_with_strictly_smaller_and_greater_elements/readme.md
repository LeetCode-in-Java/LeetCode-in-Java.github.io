## 2148\. Count Elements With Strictly Smaller and Greater Elements

Easy

Given an integer array `nums`, return _the number of elements that have **both** a strictly smaller and a strictly greater element appear in_ `nums`.

**Example 1:**

**Input:** nums = [11,7,2,15]

**Output:** 2

**Explanation:** The element 7 has the element 2 strictly smaller than it and the element 11 strictly greater than it. 

Element 11 has element 7 strictly smaller than it and element 15 strictly greater than it. 

In total there are 2 elements having both a strictly smaller and a strictly greater element appear in `nums`.

**Example 2:**

**Input:** nums = [-3,3,3,90]

**Output:** 2

**Explanation:** The element 3 has the element -3 strictly smaller than it and the element 90 strictly greater than it. 

Since there are two elements with the value 3, in total there are 2 elements having both a strictly smaller and a strictly greater element appear in `nums`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int countElements(int[] nums) {
        int min = nums[0];
        int max = nums[0];
        int minocr = 1;
        int maxocr = 1;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] < min) {
                min = nums[i];
                minocr = 1;
            } else if (nums[i] == min) {
                minocr++;
            }
            if (nums[i] > max) {
                max = nums[i];
                maxocr = 1;
            } else if (nums[i] == max) {
                maxocr++;
            }
        }
        return min == max ? 0 : nums.length - minocr - maxocr;
    }
}
```