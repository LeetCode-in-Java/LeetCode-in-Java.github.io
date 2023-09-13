[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 11\. Container With Most Water

Medium

Given `n` non-negative integers <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub></code> , where each represents a point at coordinate <code>(i, a<sub>i</sub>)</code>. `n` vertical lines are drawn such that the two endpoints of the line `i` is at <code>(i, a<sub>i</sub>)</code> and `(i, 0)`. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

**Notice** that you may not slant the container.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

**Input:** height = [1,8,6,2,5,4,8,3,7]

**Output:** 49

**Explanation:** The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49. 

**Example 2:**

**Input:** height = [1,1]

**Output:** 1 

**Example 3:**

**Input:** height = [4,3,2,1,4]

**Output:** 16 

**Example 4:**

**Input:** height = [1,2,1]

**Output:** 2 

**Constraints:**

*   `n == height.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>0 <= height[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maxArea(int[] height) {
        int maxArea = -1;
        int left = 0;
        int right = height.length - 1;
        while (left < right) {
            if (height[left] < height[right]) {
                maxArea = Math.max(maxArea, height[left] * (right - left));
                left++;
            } else {
                maxArea = Math.max(maxArea, height[right] * (right - left));
                right--;
            }
        }
        return maxArea;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where "n" represents the number of elements in the `height` array. Here's the breakdown:

1. The program uses a two-pointer approach with `left` and `right` pointers initialized at the beginning and end of the `height` array.
2. It iterates through the array using a `while` loop, and in each iteration, it performs constant-time operations:
   - It calculates the area as the product of the heights at the current `left` and `right` pointers and the distance between them (constant time).
   - It updates the `maxArea` variable by taking the maximum of the current `maxArea` and the calculated area (constant time).
   - It moves either the `left` or `right` pointer toward the center of the array based on the condition `height[left] < height[right]` or `height[left] >= height[right]`.

Since the `while` loop runs until the `left` and `right` pointers meet in the middle, and each iteration performs constant-time operations, the overall time complexity is O(n), where "n" is the number of elements in the `height` array.

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of extra space that does not depend on the input size. The program uses only a few integer variables (`maxArea`, `left`, and `right`) to keep track of the maximum area and the positions of the two pointers. The space used by these variables remains constant regardless of the input size.

Therefore, the overall space complexity is O(1).
