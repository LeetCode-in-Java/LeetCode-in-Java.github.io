[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2284\. Sender With Largest Word Count

Medium

You have a chat log of `n` messages. You are given two string arrays `messages` and `senders` where `messages[i]` is a **message** sent by `senders[i]`.

A **message** is list of **words** that are separated by a single space with no leading or trailing spaces. The **word count** of a sender is the total number of **words** sent by the sender. Note that a sender may send more than one message.

Return _the sender with the **largest** word count_. If there is more than one sender with the largest word count, return _the one with the **lexicographically largest** name_.

**Note:**

*   Uppercase letters come before lowercase letters in lexicographical order.
*   `"Alice"` and `"alice"` are distinct.

**Example 1:**

**Input:** messages = ["Hello userTwooo","Hi userThree","Wonderful day Alice","Nice day userThree"], senders = ["Alice","userTwo","userThree","Alice"]

**Output:** "Alice"

**Explanation:** Alice sends a total of 2 + 3 = 5 words.

userTwo sends a total of 2 words.

userThree sends a total of 3 words.

Since Alice has the largest word count, we return "Alice".

**Example 2:**

**Input:** messages = ["How is leetcode for everyone","Leetcode is useful for practice"], senders = ["Bob","Charlie"]

**Output:** "Charlie"

**Explanation:** Bob sends a total of 5 words.

Charlie sends a total of 5 words.

Since there is a tie for the largest word count, we return the sender with the lexicographically larger name, Charlie.

**Constraints:**

*   `n == messages.length == senders.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   `1 <= messages[i].length <= 100`
*   `1 <= senders[i].length <= 10`
*   `messages[i]` consists of uppercase and lowercase English letters and `' '`.
*   All the words in `messages[i]` are separated by **a single space**.
*   `messages[i]` does not have leading or trailing spaces.
*   `senders[i]` consists of uppercase and lowercase English letters only.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String largestWordCount(String[] messages, String[] senders) {
        HashMap<String, Integer> x = new HashMap<>();
        for (int i = 0; i < messages.length; i++) {
            int words = messages[i].length() - messages[i].replace(" ", "").length() + 1;
            if (x.containsKey(senders[i])) {
                x.put(senders[i], x.get(senders[i]) + words);
            } else {
                x.put(senders[i], words);
            }
        }
        String result = "";
        int max = 0;
        for (Map.Entry<String, Integer> entry : x.entrySet()) {
            if (entry.getValue() > max
                    || entry.getValue() == max && result.compareTo(entry.getKey()) < 0) {
                max = entry.getValue();
                result = entry.getKey();
            }
        }
        return result;
    }
}
```