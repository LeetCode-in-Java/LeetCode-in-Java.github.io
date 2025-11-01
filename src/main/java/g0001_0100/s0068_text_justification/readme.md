[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 68\. Text Justification

Hard

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

**Note:**

*   A word is defined as a character sequence consisting of non-space characters only.
*   Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
*   The input array `words` contains at least one word.

**Example 1:**

**Input:** words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16

**Output:** [ "This is an", "example of text", "justification. " ]

**Example 2:**

**Input:** words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16

**Output:** [ "What must be", "acknowledgment ", "shall be " ]

**Explanation:** Note that the last line is "shall be " instead of "shall be", because the last line must be left-justified instead of fully-justified. Note that the second line is also left-justified because it contains only one word.

**Example 3:**

**Input:** words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20

**Output:** [ "Science is what we", "understand well", "enough to explain to", "a computer. Art is", "everything else we", "do " ]

**Constraints:**

*   `1 <= words.length <= 300`
*   `1 <= words[i].length <= 20`
*   `words[i]` consists of only English letters and symbols.
*   `1 <= maxWidth <= 100`
*   `words[i].length <= maxWidth`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        // Trying to gauge the number of lines so the ArrayList doesn't need to resize
        List<String> output = new ArrayList<>((words.length + 1) / (1 + maxWidth / 7));
        // Setting StringBuilder capacity also
        StringBuilder sb = new StringBuilder(maxWidth);
        int lineTotal = 0;
        int numWordsOnLine = 0;
        int startWord = 0;
        // looping until the 2nd last word, since we're checking words[i + 1] for
        // overflows
        for (int i = 0; i < words.length - 1; i++) {
            lineTotal += words[i].length();
            // tracking line length + #words
            numWordsOnLine++;
            // checking if the next word causes an overflow
            if (lineTotal + numWordsOnLine + words[i + 1].length() > maxWidth) {
                // if only one word fits on the line...
                if (numWordsOnLine == 1) {
                    // append word
                    sb.append(words[i]);
                    // pad right with spaces
                    while (lineTotal++ < maxWidth) {
                        sb.append(' ');
                    }
                } else {
                    // # of extra spaces
                    int extraSp = (maxWidth - lineTotal) % (numWordsOnLine - 1);

                    // Creating the line
                    for (int j = startWord; j < (startWord + numWordsOnLine - 1); j++) {
                        // appending the word
                        sb.append(words[j]);
                        if (extraSp-- > 0) {
                            // appending an extra space, if required
                            sb.append(' ');
                        }
                        // appending the rest of the required spaces
                        int max = Math.max(0, (maxWidth - lineTotal) / (numWordsOnLine - 1));
                        sb.append(" ".repeat(max));
                    }
                    // appending the last word of the line
                    sb.append(words[startWord + numWordsOnLine - 1]);
                }
                // adding to output list
                output.add(sb.toString());
                // reset everything for next line
                // keeping track of the first word for next line
                startWord = i + 1;
                // resetting these to 0 for processing next line
                numWordsOnLine = lineTotal = 0;
                // need a new StringBuilder for the next line
                sb = new StringBuilder(maxWidth);
            }
        }
        // handling the final line (no justification, right padded with spaces)
        lineTotal = 0;
        for (int i = startWord; i < words.length; i++) {
            lineTotal += words[i].length();
            sb.append(words[i]);
            if (lineTotal++ < maxWidth) {
                sb.append(' ');
            }
        }
        // padding right side with spaces
        while (lineTotal++ < maxWidth) {
            sb.append(' ');
        }
        // add the final line to output list
        output.add(sb.toString());
        return output;
    }
}
```