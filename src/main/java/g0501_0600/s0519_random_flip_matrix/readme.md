[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 519\. Random Flip Matrix

Medium

There is an `m x n` binary grid `matrix` with all the values set `0` initially. Design an algorithm to randomly pick an index `(i, j)` where `matrix[i][j] == 0` and flips it to `1`. All the indices `(i, j)` where `matrix[i][j] == 0` should be equally likely to be returned.

Optimize your algorithm to minimize the number of calls made to the **built-in** random function of your language and optimize the time and space complexity.

Implement the `Solution` class:

*   `Solution(int m, int n)` Initializes the object with the size of the binary matrix `m` and `n`.
*   `int[] flip()` Returns a random index `[i, j]` of the matrix where `matrix[i][j] == 0` and flips it to `1`.
*   `void reset()` Resets all the values of the matrix to be `0`.

**Example 1:**

**Input** ["Solution", "flip", "flip", "flip", "reset", "flip"] [[3, 1], [], [], [], [], []]

**Output:** [null, [1, 0], [2, 0], [0, 0], null, [2, 0]]

**Explanation:** Solution solution = new Solution(3, 1); solution.flip(); // return [1, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned. solution.flip(); // return [2, 0], Since [1,0] was returned, [2,0] and [0,0] solution.flip(); // return [0, 0], Based on the previously returned indices, only [0,0] can be returned. solution.reset(); // All the values are reset to 0 and can be returned. solution.flip(); // return [2, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>4</sup></code>
*   There will be at least one free cell for each call to `flip`.
*   At most `1000` calls will be made to `flip` and `reset`.

## Solution

```java
import java.util.HashSet;
import java.util.Random;
import java.util.Set;

@SuppressWarnings("java:S2245")
public class Solution {
    private final int cols;
    private final int total;
    private final Random rand;
    private final Set<Integer> available;

    public Solution(int nRows, int nCols) {
        this.cols = nCols;
        this.rand = new Random();
        this.available = new HashSet<>();
        this.total = nRows * this.cols;
    }

    public int[] flip() {
        int x = rand.nextInt(this.total);
        while (available.contains(x)) {
            x = rand.nextInt(this.total);
        }

        this.available.add(x);
        return new int[] {x / this.cols, x % this.cols};
    }

    public void reset() {
        this.available.clear();
    }
}
```