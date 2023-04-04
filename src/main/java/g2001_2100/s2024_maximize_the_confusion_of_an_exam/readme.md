[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2024\. Maximize the Confusion of an Exam

Medium

A teacher is writing a test with `n` true/false questions, with `'T'` denoting true and `'F'` denoting false. He wants to confuse the students by **maximizing** the number of **consecutive** questions with the **same** answer (multiple trues or multiple falses in a row).

You are given a string `answerKey`, where `answerKey[i]` is the original answer to the <code>i<sup>th</sup></code> question. In addition, you are given an integer `k`, the maximum number of times you may perform the following operation:

*   Change the answer key for any question to `'T'` or `'F'` (i.e., set `answerKey[i]` to `'T'` or `'F'`).

Return _the **maximum** number of consecutive_ `'T'`s or `'F'`s _in the answer key after performing the operation at most_ `k` _times_.

**Example 1:**

**Input:** answerKey = "TTFF", k = 2

**Output:** 4

**Explanation:** We can replace both the 'F's with 'T's to make answerKey = "TTTT". 

There are four consecutive 'T's.

**Example 2:**

**Input:** answerKey = "TFFT", k = 1

**Output:** 3

**Explanation:** We can replace the first 'T' with an 'F' to make answerKey = "FFFT". 

Alternatively, we can replace the second 'T' with an 'F' to make answerKey = "TFFF". 

In both cases, there are three consecutive 'F's.

**Example 3:**

**Input:** answerKey = "TTFTTFTT", k = 1

**Output:** 5

**Explanation:** We can replace the first 'F' to make answerKey = "TTTTTFTT" 

Alternatively, we can replace the second 'F' to make answerKey = "TTFTTTTT". 

In both cases, there are five consecutive 'T's.

**Constraints:**

*   `n == answerKey.length`
*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   `answerKey[i]` is either `'T'` or `'F'`
*   `1 <= k <= n`

## Solution

```java
public class Solution {
    public int maxConsecutiveAnswers(String answerKey, int k) {
        int max;
        int right = 0;
        int originalK = k;
        while (k > 0 && right < answerKey.length()) {
            if (answerKey.charAt(right) == 'T') {
                k--;
            }
            right++;
        }
        max = right;
        int left = 0;
        while (right < answerKey.length() && left < answerKey.length()) {
            if (answerKey.charAt(right) == 'F') {
                right++;
                max = Math.max(max, right - left);
            } else {
                while (left < right && answerKey.charAt(left) == 'F') {
                    left++;
                }
                left++;
                right++;
            }
        }
        right = 0;
        k = originalK;
        while (k > 0 && right < answerKey.length()) {
            if (answerKey.charAt(right) == 'F') {
                k--;
            }
            right++;
        }
        max = Math.max(max, right);
        left = 0;
        while (right < answerKey.length() && left < answerKey.length()) {
            if (answerKey.charAt(right) == 'T') {
                right++;
                max = Math.max(max, right - left);
            } else {
                while (left < right && answerKey.charAt(left) == 'T') {
                    left++;
                }
                left++;
                right++;
            }
        }
        return max;
    }
}
```