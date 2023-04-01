[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1813\. Sentence Similarity III

Medium

A sentence is a list of words that are separated by a single space with no leading or trailing spaces. For example, `"Hello World"`, `"HELLO"`, `"hello world hello world"` are all sentences. Words consist of **only** uppercase and lowercase English letters.

Two sentences `sentence1` and `sentence2` are **similar** if it is possible to insert an arbitrary sentence **(possibly empty)** inside one of these sentences such that the two sentences become equal. For example, `sentence1 = "Hello my name is Jane"` and `sentence2 = "Hello Jane"` can be made equal by inserting `"my name is"` between `"Hello"` and `"Jane"` in `sentence2`.

Given two sentences `sentence1` and `sentence2`, return `true` _if_ `sentence1` _and_ `sentence2` _are similar._ Otherwise, return `false`.

**Example 1:**

**Input:** sentence1 = "My name is Haley", sentence2 = "My Haley"

**Output:** true

**Explanation:** sentence2 can be turned to sentence1 by inserting "name is" between "My" and "Haley".

**Example 2:**

**Input:** sentence1 = "of", sentence2 = "A lot of words"

**Output:** false

**Explanation:** No single sentence can be inserted inside one of the sentences to make it equal to the other.

**Example 3:**

**Input:** sentence1 = "Eating right now", sentence2 = "Eating"

**Output:** true

**Explanation:** sentence2 can be turned to sentence1 by inserting "right now" at the end of the sentence.

**Constraints:**

*   `1 <= sentence1.length, sentence2.length <= 100`
*   `sentence1` and `sentence2` consist of lowercase and uppercase English letters and spaces.
*   The words in `sentence1` and `sentence2` are separated by a single space.

## Solution

```java
@SuppressWarnings("java:S2234")
public class Solution {
    public boolean areSentencesSimilar(String sentence1, String sentence2) {
        String[] words1 = sentence1.split(" ");
        String[] words2 = sentence2.split(" ");
        int i = 0;
        int n1 = words1.length;
        int n2 = words2.length;
        if (n1 > n2) {
            return areSentencesSimilar(sentence2, sentence1);
        }
        while (i < n1 && words1[i].equals(words2[i])) {
            ++i;
        }
        while (i < n1 && words1[i].equals(words2[n2 - n1 + i])) {
            ++i;
        }
        return i == n1;
    }
}
```