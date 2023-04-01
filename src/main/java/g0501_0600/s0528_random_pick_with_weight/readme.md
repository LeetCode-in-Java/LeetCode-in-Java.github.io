[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 528\. Random Pick with Weight

Medium

You are given a **0-indexed** array of positive integers `w` where `w[i]` describes the **weight** of the <code>i<sup>th</sup></code> index.

You need to implement the function `pickIndex()`, which **randomly** picks an index in the range `[0, w.length - 1]` (**inclusive**) and returns it. The **probability** of picking an index `i` is `w[i] / sum(w)`.

*   For example, if `w = [1, 3]`, the probability of picking index `0` is `1 / (1 + 3) = 0.25` (i.e., `25%`), and the probability of picking index `1` is `3 / (1 + 3) = 0.75` (i.e., `75%`).

**Example 1:**

**Input** ["Solution","pickIndex"] [[[1]],[]]

**Output:** [null,0]

**Explanation:** 

    Solution solution = new Solution([1]); 
    solution.pickIndex(); // return 0. The only option is to return 0 since there is only one element in w.

**Example 2:**

**Input**

    ["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
    [[[1,3]],[],[],[],[],[]]

**Output:** [null,1,1,1,1,0]

**Explanation:** 

    Solution solution = new Solution([1, 3]); 
    solution.pickIndex(); // return 1. It is returning the second element (index = 1) that has a probability of 3/4. 
    solution.pickIndex(); // return 1 
    solution.pickIndex(); // return 1 
    solution.pickIndex(); // return 1 
    solution.pickIndex(); // return 0. It is returning the first element (index = 0) that has a probability of 1/4. 

    Since this is a randomization problem, multiple answers are allowed. 
    All of the following outputs can be considered correct: 
    [null,1,1,1,1,0] 
    [null,1,1,1,1,1] 
    [null,1,1,1,0,0] 
    [null,1,1,1,0,1] 
    [null,1,0,1,0,0] 
    ...... 
    and so on.

**Constraints:**

*   <code>1 <= w.length <= 10<sup>4</sup></code>
*   <code>1 <= w[i] <= 10<sup>5</sup></code>
*   `pickIndex` will be called at most <code>10<sup>4</sup></code> times.

## Solution

```java
import java.util.Comparator;
import java.util.Objects;
import java.util.Random;
import java.util.TreeSet;

@SuppressWarnings("java:S2245")
public class Solution {
    private int prefix;
    private final Random random;
    private final TreeSet<int[]> treeSet;

    public Solution(int[] w) {
        prefix = 0;
        treeSet = new TreeSet<>(Comparator.comparingInt(a -> a[0]));
        for (int i = 0; i < w.length; i++) {
            prefix += w[i];
            treeSet.add(new int[] {prefix, i});
        }
        random = new Random();
    }

    public int pickIndex() {
        int target = random.nextInt(prefix) + 1;
        return Objects.requireNonNull(treeSet.ceiling(new int[] {target, 1}))[1];
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
```