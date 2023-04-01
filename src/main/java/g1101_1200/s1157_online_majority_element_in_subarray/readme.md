[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1157\. Online Majority Element In Subarray

Hard

Design a data structure that efficiently finds the **majority element** of a given subarray.

The **majority element** of a subarray is an element that occurs `threshold` times or more in the subarray.

Implementing the `MajorityChecker` class:

*   `MajorityChecker(int[] arr)` Initializes the instance of the class with the given array `arr`.
*   `int query(int left, int right, int threshold)` returns the element in the subarray `arr[left...right]` that occurs at least `threshold` times, or `-1` if no such element exists.

**Example 1:**

**Input** ["MajorityChecker", "query", "query", "query"] [[[1, 1, 2, 2, 1, 1]], [0, 5, 4], [0, 3, 3], [2, 3, 2]]

**Output:** [null, 1, -1, 2]

**Explanation:** 

MajorityChecker majorityChecker = new MajorityChecker([1, 1, 2, 2, 1, 1]); 
majorityChecker.query(0, 5, 4); // return 1 
majorityChecker.query(0, 3, 3); // return -1 
majorityChecker.query(2, 3, 2); // return 2

**Constraints:**

*   <code>1 <= arr.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 2 * 10<sup>4</sup></code>
*   `0 <= left <= right < arr.length`
*   `threshold <= right - left + 1`
*   `2 * threshold > right - left + 1`
*   At most <code>10<sup>4</sup></code> calls will be made to `query`.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class MajorityChecker {
    private final Map<Integer, List<Integer>> valToInd;
    private static final int NUM_OF_BITS = 15;
    private final int[][] bitCount;

    public MajorityChecker(int[] arr) {
        valToInd = new HashMap<>();
        bitCount = new int[arr.length + 1][NUM_OF_BITS];
        for (int i = 0; i < arr.length; i++) {
            int val = arr[i];
            List<Integer> indList = valToInd.computeIfAbsent(val, k -> new ArrayList<>());
            indList.add(i);
            for (int j = 0; j < NUM_OF_BITS; j++) {
                bitCount[i + 1][j] = bitCount[i][j] + (val & 1);
                val >>= 1;
            }
        }
    }

    public int query(int left, int right, int threshold) {
        int candidateVal = 0;
        for (int i = NUM_OF_BITS - 1; i >= 0; i--) {
            int curBit = bitCount[right + 1][i] - bitCount[left][i] >= threshold ? 1 : 0;
            candidateVal = (candidateVal << 1) + curBit;
        }
        List<Integer> indList = valToInd.get(candidateVal);
        if (indList == null || indList.size() < threshold) {
            return -1;
        }
        int indOfLeft = Collections.binarySearch(indList, left);
        if (indOfLeft < 0) {
            indOfLeft = -indOfLeft - 1;
        }
        int indOfRight = Collections.binarySearch(indList, right);
        if (indOfRight < 0) {
            indOfRight = -indOfRight - 2;
        }
        if (indOfRight - indOfLeft + 1 >= threshold) {
            return candidateVal;
        } else {
            return -1;
        }
    }
}

/*
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker obj = new MajorityChecker(arr);
 * int param_1 = obj.query(left,right,threshold);
 */
```