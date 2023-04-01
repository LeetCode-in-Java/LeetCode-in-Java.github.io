[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 331\. Verify Preorder Serialization of a Binary Tree

Medium

One way to serialize a binary tree is to use **preorder traversal**. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as `'#'`.

![](https://assets.leetcode.com/uploads/2021/03/12/pre-tree.jpg)

For example, the above binary tree can be serialized to the string `"9,3,4,#,#,1,#,#,2,#,6,#,#"`, where `'#'` represents a null node.

Given a string of comma-separated values `preorder`, return `true` if it is a correct preorder traversal serialization of a binary tree.

It is **guaranteed** that each comma-separated value in the string must be either an integer or a character `'#'` representing null pointer.

You may assume that the input format is always valid.

*   For example, it could never contain two consecutive commas, such as `"1,,3"`.

**Note: **You are not allowed to reconstruct the tree.

**Example 1:**

**Input:** preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#"

**Output:** true 

**Example 2:**

**Input:** preorder = "1,#"

**Output:** false 

**Example 3:**

**Input:** preorder = "9,#,#,1"

**Output:** false 

**Constraints:**

*   <code>1 <= preorder.length <= 10<sup>4</sup></code>
*   `preorder` consist of integers in the range `[0, 100]` and `'#'` separated by commas `','`.

## Solution

```java
public class Solution {
    public boolean isValidSerialization(String preorder) {
        int count = 1;
        int length = preorder.length();
        for (int i = 1; i <= length; i++) {
            if (i == length || preorder.charAt(i) == ',') {
                --count;
                if (count < 0) {
                    return false;
                }
                count += preorder.charAt(i - 1) == '#' ? 0 : 2;
            }
        }
        return count == 0;
    }
}
```