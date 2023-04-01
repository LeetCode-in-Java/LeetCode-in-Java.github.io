[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1255\. Maximum Score Words Formed by Letters

Hard

Given a list of `words`, list of single `letters` (might be repeating) and `score` of every character.

Return the maximum score of **any** valid set of words formed by using the given letters (`words[i]` cannot be used two or more times).

It is not necessary to use all characters in `letters` and each letter can only be used once. Score of letters `'a'`, `'b'`, `'c'`, ... ,`'z'` is given by `score[0]`, `score[1]`, ... , `score[25]` respectively.

**Example 1:**

**Input:** words = ["dog","cat","dad","good"], letters = ["a","a","c","d","d","d","g","o","o"], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]

**Output:** 23

**Explanation:** 

Score a=1, c=9, d=5, g=3, o=2 

Given letters, we can form the words "dad" (5+1+5) and "good" (3+2+2+5) with a score of 23. 

Words "dad" and "dog" only get a score of 21.

**Example 2:**

**Input:** words = ["xxxz","ax","bx","cx"], letters = ["z","a","b","c","x","x","x"], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]

**Output:** 27

**Explanation:** 

Score a=4, b=4, c=4, x=5, z=10 

Given letters, we can form the words "ax" (4+5), "bx" (4+5) and "cx" (4+5) with a score of 27. 

Word "xxxz" only get a score of 25.

**Example 3:**

**Input:** words = ["leetcode"], letters = ["l","e","t","c","o","d"], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]

**Output:** 0

**Explanation:** Letter "e" can only be used once.

**Constraints:**

*   `1 <= words.length <= 14`
*   `1 <= words[i].length <= 15`
*   `1 <= letters.length <= 100`
*   `letters[i].length == 1`
*   `score.length == 26`
*   `0 <= score[i] <= 10`
*   `words[i]`, `letters[i]` contains only lower case English letters.

## Solution

```java
public class Solution {
    private int[] score;

    private int updateArr(int[] arr, String s, int add) {
        int sum = 0;
        for (char c : s.toCharArray()) {
            int ind = c - 'a';
            arr[ind] += add;
            if (arr[ind] < 0) {
                sum = -1;
            }
            if (sum != -1) {
                sum += this.score[ind];
            }
        }
        return sum;
    }

    private int findMaxScore(String[] words, int ind, int[] arr) {
        if (ind == words.length) {
            return 0;
        }
        int excl = findMaxScore(words, ind + 1, arr);
        int incl = 0;
        int cscore = updateArr(arr, words[ind], -1);
        if (cscore != -1) {
            incl = cscore + findMaxScore(words, ind + 1, arr);
        }
        updateArr(arr, words[ind], 1);
        return Math.max(incl, excl);
    }

    public int maxScoreWords(String[] words, char[] letters, int[] score) {
        int[] arr = new int[26];
        for (char c : letters) {
            arr[c - 'a']++;
        }
        this.score = score;
        return findMaxScore(words, 0, arr);
    }
}
```