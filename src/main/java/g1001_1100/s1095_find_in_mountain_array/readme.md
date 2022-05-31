## 1095\. Find in Mountain Array

Hard

_(This problem is an **interactive problem**.)_

You may recall that an array `arr` is a **mountain array** if and only if:

*   `arr.length >= 3`
*   There exists some `i` with `0 < i < arr.length - 1` such that:
    *   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    *   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `mountainArr`, return the **minimum** `index` such that `mountainArr.get(index) == target`. If such an `index` does not exist, return `-1`.

**You cannot access the mountain array directly.** You may only access the array using a `MountainArray` interface:

*   `MountainArray.get(k)` returns the element of the array at index `k` (0-indexed).
*   `MountainArray.length()` returns the length of the array.

Submissions making more than `100` calls to `MountainArray.get` will be judged _Wrong Answer_. Also, any solutions that attempt to circumvent the judge will result in disqualification.

**Example 1:**

**Input:** array = [1,2,3,4,5,3,1], target = 3

**Output:** 2

**Explanation:** 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.

**Example 2:**

**Input:** array = [0,1,2,4,2,1], target = 3

**Output:** -1

**Explanation:** 3 does not exist in `the array,` so we return -1.

**Constraints:**

*   <code>3 <= mountain_arr.length() <= 10<sup>4</sup></code>
*   <code>0 <= target <= 10<sup>9</sup></code>
*   <code>0 <= mountain_arr.get(index) <= 10<sup>9</sup></code>

## Solution

```java
/*
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
public class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int peakIndex = findPeak(mountainArr);
        if (target == mountainArr.get(peakIndex)) {
            return peakIndex;
        }
        int leftResult = findInPeakLeft(target, peakIndex, mountainArr);
        if (leftResult != -1) {
            return leftResult;
        }
        return findInPeakRight(target, peakIndex, mountainArr);
    }

    private int findPeak(MountainArray mountainArray) {
        int len = mountainArray.length();
        int left = 0;
        int right = len - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (mountainArray.get(mid) < mountainArray.get(mid + 1)) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

    private int findInPeakLeft(int target, int peakIndex, MountainArray mountainArray) {
        int leftIndex = 0;
        int rightIndex = peakIndex - 1;
        while (leftIndex < rightIndex) {
            int midIndex = leftIndex + (rightIndex - leftIndex) / 2;
            if (target > mountainArray.get(midIndex)) {
                leftIndex = midIndex + 1;
            } else {
                rightIndex = midIndex;
            }
        }
        return target == mountainArray.get(leftIndex) ? leftIndex : -1;
    }

    private int findInPeakRight(int target, int peakIndex, MountainArray mountainArray) {
        int leftIndex = peakIndex + 1;
        int rightIndex = mountainArray.length() - 1;
        while (leftIndex < rightIndex) {
            int midIndex = leftIndex + (rightIndex - leftIndex) / 2;
            if (target < mountainArray.get(midIndex)) {
                leftIndex = midIndex + 1;
            } else {
                rightIndex = midIndex;
            }
        }
        return target == mountainArray.get(leftIndex) ? leftIndex : -1;
    }
}
```