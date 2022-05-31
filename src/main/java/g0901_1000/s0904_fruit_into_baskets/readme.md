## 904\. Fruit Into Baskets

Medium

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the <code>i<sup>th</sup></code> tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

*   You only have **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
*   Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
*   Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return _the **maximum** number of fruits you can pick_.

**Example 1:**

**Input:** fruits = [1,2,1]

**Output:** 3

**Explanation:** We can pick from all 3 trees.

**Example 2:**

**Input:** fruits = [0,1,2,2]

**Output:** 3

**Explanation:** We can pick from trees [1,2,2]. If we had started at the first tree, we would only pick from trees [0,1].

**Example 3:**

**Input:** fruits = [1,2,3,2,2]

**Output:** 4

**Explanation:** We can pick from trees [2,3,2,2]. If we had started at the first tree, we would only pick from trees [1,2].

**Constraints:**

*   <code>1 <= fruits.length <= 10<sup>5</sup></code>
*   `0 <= fruits[i] < fruits.length`

## Solution

```java
public class Solution {
    public int totalFruit(int[] fruits) {
        int end = 1;
        int basket1 = fruits[0];
        int basket2 = -1;
        int secondFruitIndex = -1;
        int maxTotal = 1;
        int counter = 1;
        while (end < fruits.length) {
            if (fruits[end - 1] != fruits[end]) {
                if (basket2 == -1) {
                    basket2 = fruits[end];
                    secondFruitIndex = end;
                    counter++;
                } else if (fruits[end] == basket1) {
                    basket1 = basket2;
                    basket2 = fruits[end];
                    secondFruitIndex = end;
                    counter++;
                } else {
                    counter = end - secondFruitIndex + 1;
                    basket1 = basket2;
                    basket2 = fruits[end];
                    secondFruitIndex = end;
                }
            } else {
                counter++;
            }
            end++;
            maxTotal = Math.max(maxTotal, counter);
        }
        return maxTotal;
    }
}
```