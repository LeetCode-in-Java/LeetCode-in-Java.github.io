[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3213\. Construct String with Minimum Cost

Hard

You are given a string `target`, an array of strings `words`, and an integer array `costs`, both arrays of the same length.

Imagine an empty string `s`.

You can perform the following operation any number of times (including **zero**):

*   Choose an index `i` in the range `[0, words.length - 1]`.
*   Append `words[i]` to `s`.
*   The cost of operation is `costs[i]`.

Return the **minimum** cost to make `s` equal to `target`. If it's not possible, return `-1`.

**Example 1:**

**Input:** target = "abcdef", words = ["abdef","abc","d","def","ef"], costs = [100,1,1,10,5]

**Output:** 7

**Explanation:**

The minimum cost can be achieved by performing the following operations:

*   Select index 1 and append `"abc"` to `s` at a cost of 1, resulting in `s = "abc"`.
*   Select index 2 and append `"d"` to `s` at a cost of 1, resulting in `s = "abcd"`.
*   Select index 4 and append `"ef"` to `s` at a cost of 5, resulting in `s = "abcdef"`.

**Example 2:**

**Input:** target = "aaaa", words = ["z","zz","zzz"], costs = [1,10,100]

**Output:** \-1

**Explanation:**

It is impossible to make `s` equal to `target`, so we return -1.

**Constraints:**

*   <code>1 <= target.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= words.length == costs.length <= 5 * 10<sup>4</sup></code>
*   `1 <= words[i].length <= target.length`
*   The total sum of `words[i].length` is less than or equal to <code>5 * 10<sup>4</sup></code>.
*   `target` and `words[i]` consist only of lowercase English letters.
*   <code>1 <= costs[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static class ACAutomaton {
        private static class Node {
            private char key;
            private Integer val = null;
            private int len;
            private final Node[] next = new Node[26];
            private Node suffix = null;
            private Node output = null;
            private Node parent = null;
        }

        public Node build(String[] patterns, int[] values) {
            Node root = new Node();
            root.suffix = root;
            root.output = root;
            for (int i = 0; i < patterns.length; i++) {
                put(root, patterns[i], values[i]);
            }
            for (int i = 0; i < root.next.length; i++) {
                if (root.next[i] == null) {
                    root.next[i] = root;
                } else {
                    root.next[i].suffix = root;
                }
            }
            return root;
        }

        private void put(Node root, String s, int val) {
            Node node = root;
            for (char c : s.toCharArray()) {
                if (node.next[c - 'a'] == null) {
                    node.next[c - 'a'] = new Node();
                    node.next[c - 'a'].parent = node;
                    node.next[c - 'a'].key = c;
                }
                node = node.next[c - 'a'];
            }
            if (node.val == null || node.val > val) {
                node.val = val;
                node.len = s.length();
            }
        }

        public Node getOutput(Node node) {
            if (node.output == null) {
                Node suffix = getSuffix(node);
                node.output = suffix.val != null ? suffix : getOutput(suffix);
            }
            return node.output;
        }

        private Node go(Node node, char c) {
            if (node.next[c - 'a'] == null) {
                node.next[c - 'a'] = go(getSuffix(node), c);
            }
            return node.next[c - 'a'];
        }

        private Node getSuffix(Node node) {
            if (node.suffix == null) {
                node.suffix = go(getSuffix(node.parent), node.key);
                if (node.suffix.val != null) {
                    node.output = node.suffix;
                } else {
                    node.output = node.suffix.output;
                }
            }
            return node.suffix;
        }
    }

    public int minimumCost(String target, String[] words, int[] costs) {
        ACAutomaton ac = new ACAutomaton();
        ACAutomaton.Node root = ac.build(words, costs);
        int[] dp = new int[target.length() + 1];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = 0;
        ACAutomaton.Node node = root;
        for (int i = 1; i < dp.length; i++) {
            if (node != null) {
                node = ac.go(node, target.charAt(i - 1));
            }
            for (ACAutomaton.Node temp = node;
                    temp != null && temp != root;
                    temp = ac.getOutput(temp)) {
                if (temp.val != null && dp[i - temp.len] < Integer.MAX_VALUE / 2) {
                    dp[i] = Math.min(dp[i], dp[i - temp.len] + temp.val);
                }
            }
        }
        return dp[dp.length - 1] >= Integer.MAX_VALUE / 2 ? -1 : dp[dp.length - 1];
    }
}
```