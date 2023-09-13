[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 46\. Permutations

Medium

Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]] 

**Example 2:**

**Input:** nums = [0,1]

**Output:** [[0,1],[1,0]] 

**Example 3:**

**Input:** nums = [1]

**Output:** [[1]] 

**Constraints:**

*   `1 <= nums.length <= 6`
*   `-10 <= nums[i] <= 10`
*   All the integers of `nums` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new ArrayList<>();
        }
        List<List<Integer>> finalResult = new ArrayList<>();
        permuteRecur(nums, finalResult, new ArrayList<>(), new boolean[nums.length]);
        return finalResult;
    }

    private void permuteRecur(
            int[] nums, List<List<Integer>> finalResult, List<Integer> currResult, boolean[] used) {
        if (currResult.size() == nums.length) {
            finalResult.add(new ArrayList<>(currResult));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            currResult.add(nums[i]);
            used[i] = true;
            permuteRecur(nums, finalResult, currResult, used);
            used[i] = false;
            currResult.remove(currResult.size() - 1);
        }
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a recursive function, `permuteRecur`, to generate permutations. In the worst case, it explores all possible permutations.

2. The recursive function is called once for each element in the `nums` array, and in each call, it has a loop that iterates through the `nums` array. Therefore, the program generates `n!` permutations, where `n` is the number of elements in the input array.

3. The time complexity of generating each permutation is O(n), where `n` is the number of elements in the input array, because in each recursive call, we iterate through the `nums` array once.

4. Therefore, the overall time complexity of the program is O(n * n!), where `n` is the number of elements in the input array `nums`. This is because we generate `n!` permutations, and generating each permutation takes O(n) time.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the space used for the call stack during the recursion and the space used for data structures to store permutations.

2. During the recursive calls, the call stack can have a depth of up to `n`, where `n` is the number of elements in the input array `nums`. This is because the recursive function makes `n` recursive calls, each corresponding to an element in `nums`.

3. The program uses a few data structures:
   - `finalResult`: A list of lists to store permutations. In the worst case, it can contain `n!` permutations.
   - `currResult`: A list to store the current permutation being generated. Its size is at most `n`.
   - `used`: A boolean array to keep track of whether an element has been used in the current permutation. It has a size of `n`.

4. Therefore, the overall space complexity of the program is O(n + n!) in the worst case, where `n` is the number of elements in the input array `nums`. The dominant factor is `n!` due to the storage of permutations.

In summary, the time complexity of the provided program is O(n * n!), and the space complexity is O(n + n!) in the worst case, where `n` is the number of elements in the input array `nums`.
