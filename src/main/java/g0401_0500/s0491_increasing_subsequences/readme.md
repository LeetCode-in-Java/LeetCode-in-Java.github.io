## 491\. Increasing Subsequences

Medium

Given an integer array `nums`, return all the different possible increasing subsequences of the given array with **at least two elements**. You may return the answer in **any order**.

The given array may contain duplicates, and two equal integers should also be considered a special case of increasing sequence.

**Example 1:**

**Input:** nums = [4,6,7,7]

**Output:** [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

**Example 2:**

**Input:** nums = [4,4,3,2,1]

**Output:** [[4,4]]

**Constraints:**

*   `1 <= nums.length <= 15`
*   `-100 <= nums[i] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

@SuppressWarnings("java:S5413")
public class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        if (nums == null || nums.length == 1) {
            return new ArrayList<>();
        }
        Set<List<Integer>> answer = new HashSet<>();
        List<Integer> list = new ArrayList<>();
        return new ArrayList<>(backtracking(nums, 0, list, answer));
    }

    private Set<List<Integer>> backtracking(
            int[] nums, int start, List<Integer> currList, Set<List<Integer>> answer) {
        if (currList.size() >= 2) {
            answer.add(new ArrayList<>(currList));
        }
        for (int i = start; i < nums.length; i++) {
            if (currList.isEmpty() || currList.get(currList.size() - 1) <= nums[i]) {
                currList.add(nums[i]);
                backtracking(nums, i + 1, currList, answer);
                currList.remove(currList.size() - 1);
            }
        }
        return answer;
    }
}
```