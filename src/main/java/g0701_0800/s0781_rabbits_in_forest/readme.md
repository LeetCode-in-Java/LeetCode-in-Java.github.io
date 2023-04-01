[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 781\. Rabbits in Forest

Medium

There is a forest with an unknown number of rabbits. We asked n rabbits **"How many rabbits have the same color as you?"** and collected the answers in an integer array `answers` where `answers[i]` is the answer of the <code>i<sup>th</sup></code> rabbit.

Given the array `answers`, return _the minimum number of rabbits that could be in the forest_.

**Example 1:**

**Input:** answers = [1,1,2]

**Output:** 5

**Explanation:**

    The two rabbits that answered "1" could both be the same color, say red.
    The rabbit that answered "2" can't be red or the answers would be inconsistent.
    Say the rabbit that answered "2" was blue.
    Then there should be 2 other blue rabbits in the forest that didn't answer into the array.
    The smallest possible number of rabbits in the forest is therefore 5: 3 that answered plus 2 that didn't. 

**Example 2:**

**Input:** answers = [10,10,10]

**Output:** 11 

**Constraints:**

*   `1 <= answers.length <= 1000`
*   `0 <= answers[i] < 1000`

## Solution

```java
public class Solution {
    public int numRabbits(int[] answers) {
        int[] counts = new int[1001];
        for (int element : answers) {
            counts[element]++;
        }
        int answer = counts[0];
        for (int i = 1; i <= 1000; i++) {
            if (counts[i] > 0) {
                int rabbitsInPartialGroup = counts[i] % (i + 1);
                int rabbitsInCompleteGroups = counts[i] - rabbitsInPartialGroup;
                answer += rabbitsInCompleteGroups;
                if (rabbitsInPartialGroup > 0) {
                    answer += (i + 1);
                }
            }
        }
        return answer;
    }
}
```