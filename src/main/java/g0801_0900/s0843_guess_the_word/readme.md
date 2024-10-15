[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 843\. Guess the Word

Hard

This is an **_interactive problem_**.

You are given an array of **unique** strings `wordlist` where `wordlist[i]` is `6` letters long, and one word in this list is chosen as `secret`.

You may call `Master.guess(word)` to guess a word. The guessed word should have type `string` and must be from the original list with `6` lowercase letters.

This function returns an `integer` type, representing the number of exact matches (value and position) of your guess to the `secret` word. Also, if your guess is not in the given wordlist, it will return `-1` instead.

For each test case, you have exactly `10` guesses to guess the word. At the end of any number of calls, if you have made `10` or fewer calls to `Master.guess` and at least one of these guesses was `secret`, then you pass the test case.

**Example 1:**

**Input:** secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"], numguesses = 10

**Output:** You guessed the secret word correctly.

**Explanation:**

    master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
    master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
    master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
    master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
    master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
    We made 5 calls to master.guess and one of them was the secret, so we pass the test case. 

**Example 2:**

**Input:** secret = "hamada", wordlist = ["hamada","khaled"], numguesses = 10

**Output:** You guessed the secret word correctly.

**Constraints:**

*   `1 <= wordlist.length <= 100`
*   `wordlist[i].length == 6`
*   `wordlist[i]` consist of lowercase English letters.
*   All the strings of `wordlist` are **unique**.
*   `secret` exists in `wordlist`.
*   `numguesses == 10`

## Solution

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

/*
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface Master {
 *     public int guess(String word) {}
 * }
 */
public class Solution {
    public interface Master {
        int guess(String word);
    }

    private int next = 0;

    public void findSecretWord(String[] wordlist, Master master) {
        List<String> list = Arrays.asList(wordlist);
        Collections.shuffle(list);
        boolean[] test = new boolean[wordlist.length];
        while (true) {
            int num = master.guess(list.get(next));
            if (num == 6) {
                break;
            }
            updateList(list, test, num);
        }
    }

    private void updateList(List<String> list, boolean[] test, int num) {
        int index = next;
        for (int i = index + 1; i < test.length; i++) {
            if (test[i]) {
                continue;
            }
            int samePart = getSame(list.get(index), list.get(i));
            if (samePart != num) {
                test[i] = true;
            } else if (next == index) {
                next = i;
            }
        }
    }

    private int getSame(String word1, String word2) {
        int ret = 0;
        for (int i = 0; i < 6; i++) {
            if (word1.charAt(i) == word2.charAt(i)) {
                ret++;
            }
        }
        return ret;
    }
}
```