[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3953\. Maximum Score with Co-Prime Element

Hard

You are given an integer array `nums` of length `n` and an integer `maxVal`.

You **may** change any element in `nums` to any positive integer **less than or equal** to `maxVal`. Each such change costs 1.

Two integers are **co-prime** if their **greatest common divisor (GCD)** is 1.

After all modifications, you **must** choose an index `i` such that, `nums[i]` is **co-prime** with every other element `nums[j]`.

Let:

*   `selectedValue` be the final value of `nums[i]` after modifications.
*   `modificationCost` be the total number of elements changed.

The score is defined as `score = selectedValue - modificationCost`.

Return the **maximum** possible score.

**Example 1:**

**Input:** nums = [3,4,6], maxVal = 5

**Output:** 4

**Explanation:**

Change `nums[2]` from 6 to 5, which costs 1. Choose `nums[2] = 5`, since it is co-prime with 3 and 4.

*   `selectedValue = 5`
*   `modificationCost = 1`
*   The score is `5 - 1 = 4`

**Example 2:**

**Input:** nums = [1,2,3], maxVal = 4

**Output:** 3

**Explanation:**

No modifications are required. Choose `nums[2] = 3`, since it is co-prime with 1 and 2.

*   `selectedValue = 3`
*   `modificationCost = 0`
*   The score is `3 - 0 = 3`

**Example 3:**

**Input:** nums = [2,2], maxVal = 1

**Output:** 1

**Explanation:**

Change `nums[0]` from 2 to 1, which costs 1. Choose `nums[1] = 2`, since it is co-prime with 1.

*   `selectedValue = 2`
*   `modificationCost = 1`
*   The score is `2 - 1 = 1`

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= maxVal <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int maxScore(int[] nums, int maxVal) {
        int max = findMax(nums, maxVal);
        int[] cnt = countNumbers(nums, max);
        int[] groupCnt = buildGroupCounts(cnt, max);
        int best = findBestAboveMaxVal(max, maxVal, cnt, groupCnt);
        return findBestWithinMaxVal(maxVal, cnt, groupCnt, best);
    }

    private int findMax(int[] nums, int maxVal) {
        int max = maxVal;
        for (int num : nums) {
            if (num > max) {
                max = num;
            }
        }
        return max;
    }

    private int[] countNumbers(int[] nums, int max) {
        int[] cnt = new int[max + 1];
        for (int num : nums) {
            cnt[num]++;
        }
        return cnt;
    }

    private int[] buildGroupCounts(int[] cnt, int max) {
        int[] groupCnt = new int[max + 1];
        for (int i = 1; i <= max; i++) {
            for (int j = 1; j * i <= max; j++) {
                groupCnt[i] += cnt[j * i];
            }
        }
        return groupCnt;
    }

    private int findBestAboveMaxVal(int max, int maxVal, int[] cnt, int[] groupCnt) {
        int best = -1;
        for (int i = max; i > maxVal; i--) {
            if (cnt[i] == 0) {
                continue;
            }
            int change = countConflict(i, groupCnt);
            best = Math.max(best, i - change + 1);
        }
        return best;
    }

    private int findBestWithinMaxVal(int maxVal, int[] cnt, int[] groupCnt, int best) {
        for (int i = maxVal; i > 0; i--) {
            if (best != -1 && i <= best) {
                break;
            }
            int change = countConflict(i, groupCnt);
            change = adjustChange(i, change, cnt);
            best = Math.max(best, i - change);
        }
        return best;
    }

    private int adjustChange(int target, int change, int[] cnt) {
        if (change > 0 && cnt[target] != 0) {
            change--;
        }
        if (change == 0 && cnt[target] == 0) {
            change++;
        }
        return change;
    }

    private int countConflict(int target, int[] groupCnt) {
        List<Integer> primeFactors = getFactor(target);
        int change = 0;
        int subsetCount = 1 << primeFactors.size();
        for (int mask = 1; mask < subsetCount; mask++) {
            change += calculateSubsetContribution(mask, primeFactors, groupCnt);
        }
        return change;
    }

    private int calculateSubsetContribution(int mask, List<Integer> primeFactors, int[] groupCnt) {
        int product = 1;
        int bits = 0;
        for (int k = 0; k < primeFactors.size(); k++) {
            if ((mask & (1 << k)) == 0) {
                continue;
            }
            bits++;
            product *= primeFactors.get(k);
        }
        int contribution = groupCnt[product];
        return bits % 2 == 1 ? contribution : -contribution;
    }

    private List<Integer> getFactor(long number) {
        long n = number;
        List<Integer> primeFactors = new ArrayList<>();
        for (int i = 2; i <= n / i; i++) {
            if (n % i != 0) {
                continue;
            }
            primeFactors.add(i);
            while (n % i == 0) {
                n /= i;
            }
        }
        if (n > 1) {
            primeFactors.add((int) n);
        }
        return primeFactors;
    }
}
```