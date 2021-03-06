[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 682\. Baseball Game

Easy

You are keeping score for a baseball game with strange rules. The game consists of several rounds, where the scores of past rounds may affect future rounds' scores.

At the beginning of the game, you start with an empty record. You are given a list of strings `ops`, where `ops[i]` is the <code>i<sup>th</sup></code> operation you must apply to the record and is one of the following:

1.  An integer `x` - Record a new score of `x`.
2.  `"+"` - Record a new score that is the sum of the previous two scores. It is guaranteed there will always be two previous scores.
3.  `"D"` - Record a new score that is double the previous score. It is guaranteed there will always be a previous score.
4.  `"C"` - Invalidate the previous score, removing it from the record. It is guaranteed there will always be a previous score.

Return _the sum of all the scores on the record_.

**Example 1:**

**Input:** ops = ["5","2","C","D","+"]

**Output:** 30

**Explanation:** 

    "5" - Add 5 to the record, record is now [5]. 
    "2" - Add 2 to the record, record is now [5, 2]. 
    "C" - Invalidate and remove the previous score, record is now [5]. 
    "D" - Add 2 \* 5 = 10 to the record, record is now [5, 10]. 
    "+" - Add 5 + 10 = 15 to the record, record is now [5, 10, 15]. 
    The total sum is 5 + 10 + 15 = 30.

**Example 2:**

**Input:** ops = ["5","-2","4","C","D","9","+","+"]

**Output:** 27

**Explanation:** 

    "5" - Add 5 to the record, record is now [5]. 
    "-2" - Add -2 to the record, record is now [5, -2]. 
    "4" - Add 4 to the record, record is now [5, -2, 4]. 
    "C" - Invalidate and remove the previous score, record is now [5, -2]. 
    "D" - Add 2 \* -2 = -4 to the record, record is now [5, -2, -4]. 
    "9" - Add 9 to the record, record is now [5, -2, -4, 9]. 
    "+" - Add -4 + 9 = 5 to the record, record is now [5, -2, -4, 9, 5]. 
    "+" - Add 9 + 5 = 14 to the record, record is now [5, -2, -4, 9, 5, 14]. 
    The total sum is 5 + -2 + -4 + 9 + 5 + 14 = 27.

**Example 3:**

**Input:** ops = ["1"]

**Output:** 1

**Constraints:**

*   `1 <= ops.length <= 1000`
*   `ops[i]` is `"C"`, `"D"`, `"+"`, or a string representing an integer in the range <code>[-3 * 10<sup>4</sup>, 3 * 10<sup>4</sup>]</code>.
*   For operation `"+"`, there will always be at least two previous scores on the record.
*   For operations `"C"` and `"D"`, there will always be at least one previous score on the record.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int calPoints(String[] ops) {
        List<Integer> resultList = new ArrayList<>(ops.length);
        for (String op : ops) {
            int size = resultList.size();
            if ("C".equals(op)) {
                resultList.remove(size - 1);
            } else if ("D".equals(op)) {
                resultList.add(resultList.get(size - 1) * 2);
            } else if ("+".equals(op)) {
                resultList.add(resultList.get(size - 1) + resultList.get(size - 2));
            } else {
                resultList.add(Integer.parseInt(op));
            }
        }
        return resultList.stream().reduce(0, Integer::sum);
    }
}
```