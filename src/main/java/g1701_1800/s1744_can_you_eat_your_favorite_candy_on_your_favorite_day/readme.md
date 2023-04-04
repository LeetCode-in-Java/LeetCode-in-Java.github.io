[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1744\. Can You Eat Your Favorite Candy on Your Favorite Day?

Medium

You are given a **(0-indexed)** array of positive integers `candiesCount` where `candiesCount[i]` represents the number of candies of the <code>i<sup>th</sup></code> type you have. You are also given a 2D array `queries` where <code>queries[i] = [favoriteType<sub>i</sub>, favoriteDay<sub>i</sub>, dailyCap<sub>i</sub>]</code>.

You play a game with the following rules:

*   You start eating candies on day `**0**`.
*   You **cannot** eat **any** candy of type `i` unless you have eaten **all** candies of type `i - 1`.
*   You must eat **at least** **one** candy per day until you have eaten all the candies.

Construct a boolean array `answer` such that `answer.length == queries.length` and `answer[i]` is `true` if you can eat a candy of type <code>favoriteType<sub>i</sub></code> on day <code>favoriteDay<sub>i</sub></code> without eating **more than** <code>dailyCap<sub>i</sub></code> candies on **any** day, and `false` otherwise. Note that you can eat different types of candy on the same day, provided that you follow rule 2.

Return _the constructed array_ `answer`.

**Example 1:**

**Input:** candiesCount = [7,4,5,3,8], queries = \[\[0,2,2],[4,2,4],[2,13,1000000000]]

**Output:** [true,false,true]

**Explanation:** 

1- If you eat 2 candies (type 0) on day 0 and 2 candies (type 0) on day 1, you will eat a candy of type 0 on day 2. 

2- You can eat at most 4 candies each day. 

If you eat 4 candies every day, you will eat 4 candies (type 0) on day 0 and 4 candies (type 0 and type 1) on day 1. 

On day 2, you can only eat 4 candies (type 1 and type 2), so you cannot eat a candy of type 4 on day 2. 

3- If you eat 1 candy each day, you will eat a candy of type 2 on day 13.

**Example 2:**

**Input:** candiesCount = [5,2,6,4,1], queries = \[\[3,1,2],[4,10,3],[3,10,100],[4,100,30],[1,3,1]]

**Output:** [false,true,true,false,false]

**Constraints:**

*   <code>1 <= candiesCount.length <= 10<sup>5</sup></code>
*   <code>1 <= candiesCount[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 3`
*   <code>0 <= favoriteType<sub>i</sub> < candiesCount.length</code>
*   <code>0 <= favoriteDay<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= dailyCap<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public boolean[] canEat(int[] candiesCount, int[][] queries) {
        boolean[] result = new boolean[queries.length];
        long[] candiesComm = new long[candiesCount.length + 1];
        for (int i = 1; i <= candiesCount.length; i++) {
            candiesComm[i] = candiesComm[i - 1] + candiesCount[i - 1];
        }
        for (int i = 0; i < queries.length; i++) {
            int type = queries[i][0];
            long day = queries[i][1];
            long cap = queries[i][2];
            result[i] = ((day + 1) * cap > candiesComm[type]) && day < candiesComm[type + 1];
        }
        return result;
    }
}
```