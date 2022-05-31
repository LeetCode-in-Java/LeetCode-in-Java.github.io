## 297\. Serialize and Deserialize Binary Tree

Hard

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as _how LeetCode serializes a binary tree_. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

**Input:** root = [1,2,3,null,null,4,5]

**Output:** [1,2,3,null,null,4,5] 

**Example 2:**

**Input:** root = []

**Output:** [] 

**Example 3:**

**Input:** root = [1]

**Output:** [1] 

**Example 4:**

**Input:** root = [1,2]

**Output:** [1,2] 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   `-1000 <= Node.val <= 1000`

## Solution

```java
import com_github_leetcode.TreeNode;

/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    private static final int BASE_OFFSET = 1000;
    private static final String DELIM = "*";
    private int offset;

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        offset = 0;
        serialize(root, sb);
        return sb.toString();
    }

    public void serialize(TreeNode root, StringBuilder sb) {
        // all nodes fit into 4 bits.
        // IFF we offset at 0. So encode(val) = val + min(default - 1000)
        if (root == null) {
            sb.append(DELIM);
            return;
        }
        String s = Integer.toHexString(root.val + BASE_OFFSET);
        StringBuilder sb2 = new StringBuilder();
        for (int i = 0; i < 3 - s.length(); i++) {
            sb2.append('0');
        }
        sb2.append(s);
        sb.append(sb2);
        serialize(root.left, sb);
        serialize(root.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.charAt(offset) == '*') {
            offset++;
            return null;
        }
        TreeNode root =
                new TreeNode(
                        Integer.parseInt(data.substring(offset, offset + 3), 16) - BASE_OFFSET);
        offset += 3;
        root.left = deserialize(data);
        root.right = deserialize(data);
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```