[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 838\. Push Dominoes

Medium

There are `n` dominoes in a line, and we place each domino vertically upright. In the beginning, we simultaneously push some of the dominoes either to the left or to the right.

After each second, each domino that is falling to the left pushes the adjacent domino on the left. Similarly, the dominoes falling to the right push their adjacent dominoes standing on the right.

When a vertical domino has dominoes falling on it from both sides, it stays still due to the balance of the forces.

For the purposes of this question, we will consider that a falling domino expends no additional force to a falling or already fallen domino.

You are given a string `dominoes` representing the initial state where:

*   `dominoes[i] = 'L'`, if the <code>i<sup>th</sup></code> domino has been pushed to the left,
*   `dominoes[i] = 'R'`, if the <code>i<sup>th</sup></code> domino has been pushed to the right, and
*   `dominoes[i] = '.'`, if the <code>i<sup>th</sup></code> domino has not been pushed.

Return _a string representing the final state_.

**Example 1:**

**Input:** dominoes = "RR.L"

**Output:** "RR.L"

**Explanation:** The first domino expends no additional force on the second domino.

**Example 2:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/05/18/domino.png)

**Input:** dominoes = ".L.R...LR..L.."

**Output:** "LL.RR.LLRRLL.."

**Constraints:**

*   `n == dominoes.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `dominoes[i]` is either `'L'`, `'R'`, or `'.'`.

## Solution

```java
public class Solution {
    public String pushDominoes(String dominoes) {
        char[] ch = new char[dominoes.length() + 2];
        ch[0] = 'L';
        ch[ch.length - 1] = 'R';
        for (int k = 1; k < ch.length - 1; k++) {
            ch[k] = dominoes.charAt(k - 1);
        }
        int i = 0;
        int j = 1;
        while (j < ch.length) {
            while (ch[j] == '.') {
                j++;
            }
            if (ch[i] == 'L' && ch[j] == 'L') {
                while (i != j) {
                    ch[i] = 'L';
                    i++;
                }
                j++;
            } else if (ch[i] == 'R' && ch[j] == 'R') {
                while (i != j) {
                    ch[i] = 'R';
                    i++;
                }
                j++;
            } else if (ch[i] == 'L' && ch[j] == 'R') {
                i = j;
                j++;
            } else if (ch[i] == 'R' && ch[j] == 'L') {
                int rTemp = i + 1;
                int lTemp = j - 1;
                while (rTemp < lTemp) {
                    ch[rTemp] = 'R';
                    ch[lTemp] = 'L';
                    rTemp++;
                    lTemp--;
                }
                i = j;
                j++;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int k = 1; k < ch.length - 1; k++) {
            sb.append(ch[k]);
        }
        return sb.toString();
    }
}
```