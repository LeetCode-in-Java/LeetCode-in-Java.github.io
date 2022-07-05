[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2007\. Find Original Array From Doubled Array

Medium

An integer array `original` is transformed into a **doubled** array `changed` by appending **twice the value** of every element in `original`, and then randomly **shuffling** the resulting array.

Given an array `changed`, return `original` _if_ `changed` _is a **doubled** array. If_ `changed` _is not a **doubled** array, return an empty array. The elements in_ `original` _may be returned in **any** order_.

**Example 1:**

**Input:** changed = [1,3,4,2,6,8]

**Output:** [1,3,4]

**Explanation:** One possible original array could be [1,3,4]: 

- Twice the value of 1 is 1 \* 2 = 2. 

- Twice the value of 3 is 3 \* 2 = 6. 

- Twice the value of 4 is 4 \* 2 = 8. 
  
Other original arrays could be [4,3,1] or [3,1,4].

**Example 2:**

**Input:** changed = [6,3,0,1]

**Output:** []

**Explanation:** changed is not a doubled array.

**Example 3:**

**Input:** changed = [1]

**Output:** []

**Explanation:** changed is not a doubled array.

**Constraints:**

*   <code>1 <= changed.length <= 10<sup>5</sup></code>
*   <code>0 <= changed[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] findOriginalArray(int[] changed) {
        if (changed.length % 2 == 1) {
            return new int[0];
        }
        int[] a = new int[100001];
        for (int j : changed) {
            a[j]++;
        }
        if (a[0] % 2 == 1) {
            return new int[0];
        }
        int[] ans = new int[changed.length / 2];
        int p = 0;
        if (a[0] > 0) {
            a[0] /= 2;
            while (a[0] > 0) {
                ans[p++] = 0;
                a[0]--;
            }
        }
        for (int i = 1; i <= 100001 / 2; i++) {
            if (a[i] == 0) {
                continue;
            }
            int tmp = i * 2;
            if (a[tmp] >= a[i]) {
                a[tmp] = a[tmp] - a[i];
                while (a[i] > 0) {
                    ans[p++] = i;
                    a[i]--;
                }
            } else {
                return new int[0];
            }
        }
        for (int i = 1; i < a.length; i++) {
            if (a[i] != 0) {
                return new int[0];
            }
        }
        return ans;
    }
}
```