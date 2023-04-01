[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2418\. Sort the People

Easy

You are given an array of strings `names`, and an array `heights` that consists of **distinct** positive integers. Both arrays are of length `n`.

For each index `i`, `names[i]` and `heights[i]` denote the name and height of the <code>i<sup>th</sup></code> person.

Return `names` _sorted in **descending** order by the people's heights_.

**Example 1:**

**Input:** names = ["Mary","John","Emma"], heights = [180,165,170]

**Output:** ["Mary","Emma","John"]

**Explanation:** Mary is the tallest, followed by Emma and John. 

**Example 2:**

**Input:** names = ["Alice","Bob","Bob"], heights = [155,185,150]

**Output:** ["Bob","Alice","Bob"]

**Explanation:** The first Bob is the tallest, followed by Alice and the second Bob. 

**Constraints:**

*   `n == names.length == heights.length`
*   <code>1 <= n <= 10<sup>3</sup></code>
*   `1 <= names[i].length <= 20`
*   <code>1 <= heights[i] <= 10<sup>5</sup></code>
*   `names[i]` consists of lower and upper case English letters.
*   All the values of `heights` are distinct.

## Solution

```java
public class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        quicksort(names, heights, 0, heights.length - 1);
        return names;
    }

    private void quicksort(String[] names, int[] heights, int low, int high) {
        if (low >= high) {
            return;
        }
        int start = low;
        int end = high;
        int mid = start + (end - start) / 2;
        int pivot = heights[mid];
        while (start < end) {
            while (heights[start] > pivot) {
                start++;
            }
            while (heights[end] < pivot) {
                end--;
            }
            int tempHeight = heights[start];
            heights[start] = heights[end];
            heights[end] = tempHeight;
            String tempName = names[start];
            names[start] = names[end];
            names[end] = tempName;
        }
        quicksort(names, heights, low, end - 1);
        quicksort(names, heights, start + 1, high);
    }
}
```