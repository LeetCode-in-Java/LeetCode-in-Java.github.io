## 117\. Populating Next Right Pointers in Each Node II

Medium

Given a binary tree

struct Node { int val; Node \*left; Node \*right; Node \*next; } 

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

**Input:** root = [1,2,3,4,5,null,7]

**Output:** [1,#,2,3,#,4,5,7,#]

**Explanation:** Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level. 

**Example 2:**

**Input:** root = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 6000]`.
*   `-100 <= Node.val <= 100`

**Follow-up:**

*   You may only use constant extra space.
*   The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

## Solution

```java
import com_github_leetcode.left_right.Node;

/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

public class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        if (root.left == null && root.right == null) {
            return root;
        }

        if (root.left != null) {
            if (root.right != null) {
                root.left.next = root.right;
            } else if (root.next != null) {
                root.left.next = adjacentRightNode(root.next);
            }
        }
        if (root.right != null) {
            root.right.next = adjacentRightNode(root.next);
        }
        connect(root.right);
        connect(root.left);
        return root;
    }

    private Node adjacentRightNode(Node root) {
        Node temp = root;
        while (temp != null) {
            if (temp.left == null && temp.right == null) {
                temp = temp.next;
            } else {
                if (temp.left != null) {
                    return temp.left;
                }
                if (temp.right != null) {
                    return temp.right;
                }
            }
        }
        return null;
    }
}
```