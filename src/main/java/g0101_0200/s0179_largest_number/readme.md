[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 179\. Largest Number

Medium

Given a list of non-negative integers `nums`, arrange them such that they form the largest number.

**Note:** The result may be very large, so you need to return a string instead of an integer.

**Example 1:**

**Input:** nums = [10,2]

**Output:** "210" 

**Example 2:**

**Input:** nums = [3,30,34,5,9]

**Output:** "9534330" 

**Example 3:**

**Input:** nums = [1]

**Output:** "1" 

**Example 4:**

**Input:** nums = [10]

**Output:** "10" 

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public String largestNumber(int[] nums) {
        int n = nums.length;
        String[] s = new String[n];
        for (int i = 0; i < n; i++) {
            s[i] = String.valueOf(nums[i]);
        }
        Arrays.sort(s, (a, b) -> (b + a).compareTo(a + b));
        StringBuilder sb = new StringBuilder();
        for (String str : s) {
            sb.append(str);
        }
        String result = sb.toString();
        return result.startsWith("0") ? "0" : result;
    }
}
```