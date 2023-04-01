[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1345\. Jump Game IV

Hard

Given an array of integers `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index `i` to index:

*   `i + 1` where: `i + 1 < arr.length`.
*   `i - 1` where: `i - 1 >= 0`.
*   `j` where: `arr[i] == arr[j]` and `i != j`.

Return _the minimum number of steps_ to reach the **last index** of the array.

Notice that you can not jump outside of the array at any time.

**Example 1:**

**Input:** arr = [100,-23,-23,404,100,23,23,23,3,404]

**Output:** 3

**Explanation:** You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.

**Example 2:**

**Input:** arr = [7]

**Output:** 0

**Explanation:** Start index is the last index. You do not need to jump.

**Example 3:**

**Input:** arr = [7,6,9,6,9,6,9,7]

**Output:** 1

**Explanation:** You can jump directly from index 0 to index 7 which is last index of the array.

**Constraints:**

*   <code>1 <= arr.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>8</sup> <= arr[i] <= 10<sup>8</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Deque;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public int minJumps(int[] arr) {
        if (arr.length == 1) {
            return 0;
        }

        int len = arr.length;
        HashMap<Integer, List<Integer>> myHash = new HashMap<>();

        int i = 0;
        while (i < arr.length) {
            List<Integer> curList = myHash.getOrDefault(arr[i], new ArrayList<>());
            curList.add(i);
            int tempNum = arr[i];
            int tempIndex = i;
            while (i < arr.length && arr[i] == tempNum) {
                i++;
            }
            if (i != tempIndex + 1) {
                curList.add(i - 1);
            }
            myHash.put(tempNum, curList);
        }

        Deque<Integer> myQueue = new LinkedList<>();
        int step = 0;
        myQueue.offerLast(0);

        boolean[] visited = new boolean[arr.length];
        visited[0] = true;

        while (!myQueue.isEmpty()) {
            int curCount = myQueue.size();
            int j = 0;
            while (j < curCount) {
                int curIndex = myQueue.pollFirst();
                if (curIndex == len - 1) {
                    return step;
                }
                if (curIndex + 1 < len && !visited[curIndex + 1]) {
                    myQueue.offerLast(curIndex + 1);
                    visited[curIndex + 1] = true;
                }
                if (curIndex - 1 >= 0 && !visited[curIndex - 1]) {
                    myQueue.offerLast(curIndex - 1);
                    visited[curIndex - 1] = true;
                }
                List<Integer> tempList = myHash.getOrDefault(arr[curIndex], new ArrayList<>());
                for (Integer integer : tempList) {
                    if (!visited[integer]) {
                        myQueue.offerLast(integer);
                        visited[integer] = true;
                    }
                }
                myHash.remove(arr[curIndex]);
                j++;
            }
            step++;
        }

        return step;
    }
}
```