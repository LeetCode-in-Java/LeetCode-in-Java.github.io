[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 968\. Binary Tree Cameras

Hard

You are given the `root` of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children.

Return _the minimum number of cameras needed to monitor all nodes of the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

**Input:** root = [0,0,null,0,0]

**Output:** 1

**Explanation:** One camera is enough to monitor all nodes if placed as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png)

**Input:** root = [0,0,null,0,null,0,null,null,0]

**Output:** 2

**Explanation:** At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `Node.val == 0`

## Solution

```java
import com_github_leetcode.TreeNode;

/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    private int cameras;

    public int minCameraCover(TreeNode root) {
        cameras = 0;
        if (minCameras(root) == -1) {
            // root needs a camera
            cameras++;
        }
        return cameras;
    }

    //  States =>
    // -1 : Node needs a camera
    //  0 : Node has a camera placed
    //  1 : Node is covered somehow
    private int minCameras(TreeNode root) {
        if (root == null) {
            return 1;
        }
        int leftChildState = minCameras(root.left);
        int rightChildState = minCameras(root.right);
        // One of the two or both children need a camera
        if (leftChildState == -1 || rightChildState == -1) {
            // installed
            cameras++;
            return 0;
        }
        // One of the two or both children have a camera placed
        if (leftChildState == 0 || rightChildState == 0) {
            // gets covered by the children
            return 1;
        }
        // needs a camera
        return -1;
    }
}
```