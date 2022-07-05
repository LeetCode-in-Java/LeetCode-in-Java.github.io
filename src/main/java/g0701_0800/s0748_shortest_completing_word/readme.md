[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 748\. Shortest Completing Word

Easy

Given a string `licensePlate` and an array of strings `words`, find the **shortest completing** word in `words`.

A **completing** word is a word that **contains all the letters** in `licensePlate`. **Ignore numbers and spaces** in `licensePlate`, and treat letters as **case insensitive**. If a letter appears more than once in `licensePlate`, then it must appear in the word the same number of times or more.

For example, if `licensePlate` `= "aBc 12c"`, then it contains letters `'a'`, `'b'` (ignoring case), and `'c'` twice. Possible **completing** words are `"abccdef"`, `"caaacab"`, and `"cbca"`.

Return _the shortest **completing** word in_ `words`_._ It is guaranteed an answer exists. If there are multiple shortest **completing** words, return the **first** one that occurs in `words`.

**Example 1:**

**Input:** licensePlate = "1s3 PSt", words = ["step","steps","stripe","stepple"]

**Output:** "steps"

**Explanation:** licensePlate contains letters 's', 'p', 's' (ignoring case), and 't'. 

"step" contains 't' and 'p', but only contains 1 's'. 

"steps" contains 't', 'p', and both 's' characters. 

"stripe" is missing an 's'. 

"stepple" is missing an 's'. 

Since "steps" is the only word containing all the letters, that is the answer.

**Example 2:**

**Input:** licensePlate = "1s3 456", words = ["looks","pest","stew","show"]

**Output:** "pest"

**Explanation:** licensePlate only contains the letter 's'. All the words contain 's', but among these "pest", "stew", and "show" are shortest. The answer is "pest" because it is the word that appears earliest of the 3.

**Constraints:**

*   `1 <= licensePlate.length <= 7`
*   `licensePlate` contains digits, letters (uppercase or lowercase), or space `' '`.
*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 15`
*   `words[i]` consists of lower case English letters.

## Solution

```java
public class Solution {
    public String shortestCompletingWord(String licensePlate, String[] words) {
        licensePlate = licensePlate.toLowerCase();
        int[] a = new int[26];

        for (int i = 0; i < licensePlate.length(); i++) {
            if (Character.isLetter(licensePlate.charAt(i))) {
                a[licensePlate.charAt(i) - 'a']++;
            }
        }
        String ans = "";
        for (String str : words) {
            int[] a1 = new int[26];
            for (int j = 0; j < str.length(); j++) {
                a1[str.charAt(j) - 'a']++;
            }
            int j = 0;
            while (j < 26) {
                if (a[j] <= a1[j]) {
                    j++;
                } else {
                    break;
                }
            }

            if (j == 26 && (ans.isEmpty() || ans.length() > str.length())) {
                ans = str;
            }
        }
        return ans;
    }
}
```