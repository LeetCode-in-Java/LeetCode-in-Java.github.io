[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1773\. Count Items Matching a Rule

Easy

You are given an array `items`, where each <code>items[i] = [type<sub>i</sub>, color<sub>i</sub>, name<sub>i</sub>]</code> describes the type, color, and name of the <code>i<sup>th</sup></code> item. You are also given a rule represented by two strings, `ruleKey` and `ruleValue`.

The <code>i<sup>th</sup></code> item is said to match the rule if **one** of the following is true:

*   `ruleKey == "type"` and <code>ruleValue == type<sub>i</sub></code>.
*   `ruleKey == "color"` and <code>ruleValue == color<sub>i</sub></code>.
*   `ruleKey == "name"` and <code>ruleValue == name<sub>i</sub></code>.

Return _the number of items that match the given rule_.

**Example 1:**

**Input:** items = \[\["phone","blue","pixel"],["computer","silver","lenovo"],["phone","gold","iphone"]], ruleKey = "color", ruleValue = "silver"

**Output:** 1

**Explanation:** There is only one item matching the given rule, which is ["computer","silver","lenovo"].

**Example 2:**

**Input:** items = \[\["phone","blue","pixel"],["computer","silver","phone"],["phone","gold","iphone"]], ruleKey = "type", ruleValue = "phone"

**Output:** 2

**Explanation:** There are only two items matching the given rule, which are ["phone","blue","pixel"] and ["phone","gold","iphone"]. Note that the item ["computer","silver","phone"] does not match.

**Constraints:**

*   <code>1 <= items.length <= 10<sup>4</sup></code>
*   <code>1 <= type<sub>i</sub>.length, color<sub>i</sub>.length, name<sub>i</sub>.length, ruleValue.length <= 10</code>
*   `ruleKey` is equal to either `"type"`, `"color"`, or `"name"`.
*   All strings consist only of lowercase letters.

## Solution

```java
import java.util.List;

public class Solution {
    public int countMatches(List<List<String>> items, String ruleKey, String ruleValue) {
        int ans = 0;
        int checkRuleNum = 0;
        if (ruleKey.equals("color")) {
            checkRuleNum = 1;
        } else if (ruleKey.equals("name")) {
            checkRuleNum = 2;
        }
        for (List<String> item : items) {
            if (item.get(checkRuleNum).equals(ruleValue)) {
                ans++;
            }
        }
        return ans;
    }
}
```