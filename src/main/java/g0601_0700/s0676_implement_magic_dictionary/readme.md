## 676\. Implement Magic Dictionary

Medium

Design a data structure that is initialized with a list of **different** words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.

Implement the `MagicDictionary` class:

*   `MagicDictionary()` Initializes the object.
*   `void buildDict(String[] dictionary)` Sets the data structure with an array of distinct strings `dictionary`.
*   `bool search(String searchWord)` Returns `true` if you can change **exactly one character** in `searchWord` to match any string in the data structure, otherwise returns `false`.

**Example 1:**

**Input** 

["MagicDictionary", "buildDict", "search", "search", "search", "search"] [[], 

[["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]

**Output:** [null, null, false, true, false, false]

**Explanation:** 

    MagicDictionary magicDictionary = new MagicDictionary(); 
    magicDictionary.buildDict(["hello", "leetcode"]); 
    magicDictionary.search("hello"); // return False 
    magicDictionary.search("hhllo"); // We can change the second 'h' to 'e' to match "hello" so we return True 
    magicDictionary.search("hell"); // return False 
    magicDictionary.search("leetcoded"); // return False

**Constraints:**

*   `1 <= dictionary.length <= 100`
*   `1 <= dictionary[i].length <= 100`
*   `dictionary[i]` consists of only lower-case English letters.
*   All the strings in `dictionary` are **distinct**.
*   `1 <= searchWord.length <= 100`
*   `searchWord` consists of only lower-case English letters.
*   `buildDict` will be called only once before `search`.
*   At most `100` calls will be made to `search`.

## Solution

```java
public class MagicDictionary {
    private String[] dictionaryWords;

    public MagicDictionary() {
        dictionaryWords = null;
    }

    public void buildDict(String[] dictionary) {
        dictionaryWords = dictionary;
    }

    public boolean search(String searchWord) {
        for (String word : dictionaryWords) {
            if (isOffByOneLetter(word, searchWord)) {
                return true;
            }
        }
        return false;
    }

    private boolean isOffByOneLetter(String word, String searchWord) {
        if (isDifferentLengths(word, searchWord) || word.equals(searchWord)) {
            return false;
        }
        int numDifferentLetters = 0;
        for (int i = 0; i < word.length(); i++) {
            if (isNotTheSameLetter(word.charAt(i), searchWord.charAt(i))) {
                numDifferentLetters++;
            }
            if (numDifferentLetters > 1) {
                return false;
            }
        }
        return numDifferentLetters == 1;
    }

    private boolean isDifferentLengths(String word, String searchWord) {
        return word.length() != searchWord.length();
    }

    private boolean isNotTheSameLetter(char c1, char c2) {
        return c1 != c2;
    }
}

/*
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```