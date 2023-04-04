[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1268\. Search Suggestions System

Medium

You are given an array of strings `products` and a string `searchWord`.

Design a system that suggests at most three product names from `products` after each character of `searchWord` is typed. Suggested products should have common prefix with `searchWord`. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return _a list of lists of the suggested products after each character of_ `searchWord` _is typed_.

**Example 1:**

**Input:** products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"

**Output:** 
        
    [ 
        ["mobile","moneypot","monitor"], 
        ["mobile","moneypot","monitor"], 
        ["mouse","mousepad"], 
        ["mouse","mousepad"], 
        ["mouse","mousepad"] 
                                ]

**Explanation:** products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"] After typing m and mo all products match and we show user ["mobile","moneypot","monitor"] After typing mou, mous and mouse the system suggests ["mouse","mousepad"]

**Example 2:**

**Input:** products = ["havana"], searchWord = "havana"

**Output:** [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]

**Example 3:**

**Input:** products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"

**Output:** [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]

**Constraints:**

*   `1 <= products.length <= 1000`
*   `1 <= products[i].length <= 3000`
*   <code>1 <= sum(products[i].length) <= 2 * 10<sup>4</sup></code>
*   All the strings of `products` are **unique**.
*   `products[i]` consists of lowercase English letters.
*   `1 <= searchWord.length <= 1000`
*   `searchWord` consists of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        Trie trie = new Trie();
        Arrays.sort(products);
        for (String p : products) {
            trie.insert(p);
        }

        return trie.getResult(searchWord);
    }

    static class Trie {
        Node root;

        public Trie() {
            root = new Node();
        }

        void insert(String word) {
            Node curr = root;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                if (!curr.containsKey(c)) {
                    curr.put(c, new Node());
                }
                curr = curr.get(c);
                curr.addToList(word);
            }
        }

        List<List<String>> getResult(String searchWord) {
            Node curr = root;
            List<List<String>> res = new ArrayList<>();
            for (int i = 0; i < searchWord.length(); i++) {
                char c = searchWord.charAt(i);
                List<String> temp = new ArrayList<>();
                if (curr != null) {
                    curr = curr.get(c);
                }

                for (int j = 0; j < 3 && curr != null && j < curr.getList().size(); j++) {
                    temp.add(curr.getList().get(j));
                }
                res.add(new ArrayList<>(temp));
            }
            return res;
        }
    }

    static class Node {
        Node[] links;
        List<String> list;

        public Node() {
            links = new Node[26];
            list = new ArrayList<>();
        }

        boolean containsKey(char c) {
            return links[c - 'a'] != null;
        }

        Node get(char c) {
            return links[c - 'a'];
        }

        void put(char c, Node node) {
            links[c - 'a'] = node;
        }

        void addToList(String s) {
            list.add(s);
        }

        List<String> getList() {
            return list;
        }
    }
}
```