## 957\. Prison Cells After N Days

Medium

There are `8` prison cells in a row and each cell is either occupied or vacant.

Each day, whether the cell is occupied or vacant changes according to the following rules:

*   If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
*   Otherwise, it becomes vacant.

**Note** that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.

You are given an integer array `cells` where `cells[i] == 1` if the <code>i<sup>th</sup></code> cell is occupied and `cells[i] == 0` if the <code>i<sup>th</sup></code> cell is vacant, and you are given an integer `n`.

Return the state of the prison after `n` days (i.e., `n` such changes described above).

**Example 1:**

**Input:** cells = [0,1,0,1,1,0,0,1], n = 7

**Output:** [0,0,1,1,0,0,0,0]

**Explanation:** The following table summarizes the state of the prison on each day:

Day 0: [0, 1, 0, 1, 1, 0, 0, 1]

Day 1: [0, 1, 1, 0, 0, 0, 0, 0]

Day 2: [0, 0, 0, 0, 1, 1, 1, 0]

Day 3: [0, 1, 1, 0, 0, 1, 0, 0]

Day 4: [0, 0, 0, 0, 0, 1, 0, 0]

Day 5: [0, 1, 1, 1, 0, 1, 0, 0]

Day 6: [0, 0, 1, 0, 1, 1, 0, 0]

Day 7: [0, 0, 1, 1, 0, 0, 0, 0]

**Example 2:**

**Input:** cells = [1,0,0,1,0,0,1,0], n = 1000000000

**Output:** [0,0,1,1,1,1,1,0]

**Constraints:**

*   `cells.length == 8`
*   `cells[i]` is either `0` or `1`.
*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] prisonAfterNDays(int[] cells, int n) {
        if (n == 0) {
            return cells;
        }
        int[] first = null;
        int[] prev = cells;
        int period;
        int day = 0;
        while (n > 0) {
            day++;
            n--;
            int[] next = getNextDay(prev);
            if (Arrays.equals(next, first)) {
                period = day - 1;
                n %= period;
            }
            if (day == 1) {
                first = next;
            }
            prev = next;
        }
        return prev;
    }

    private int[] getNextDay(int[] arr) {
        int[] ret = new int[8];
        for (int i = 1; i < 7; ++i) {
            if (arr[i - 1] == arr[i + 1]) {
                ret[i] = 1;
            }
        }
        return ret;
    }
}
```