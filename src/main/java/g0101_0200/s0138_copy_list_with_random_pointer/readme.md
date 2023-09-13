[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 138\. Copy List with Random Pointer

Medium

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return _the head of the copied linked list_.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

*   `val`: an integer representing `Node.val`
*   `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

**Input:** head = \[\[7,null],[13,0],[11,4],[10,2],[1,0]]

**Output:** [[7,null],[13,0],[11,4],[10,2],[1,0]] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

**Input:** head = \[\[1,1],[2,1]]

**Output:** [[1,1],[2,1]] 

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/12/18/e3.png)**

**Input:** head = \[\[3,null],[3,0],[3,null]]

**Output:** [[3,null],[3,0],[3,null]] 

**Example 4:**

**Input:** head = []

**Output:** []

**Explanation:** The given linked list is empty (null pointer), so return null. 

**Constraints:**

*   `0 <= n <= 1000`
*   `-10000 <= Node.val <= 10000`
*   `Node.random` is `null` or is pointing to some node in the linked list.

## Solution

```java
import com_github_leetcode.random.Node;

/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;
    public Node random;

    public Node() {}

    public Node(int _val,Node _next,Node _random) {
        val = _val;
        next = _next;
        random = _random;
    }
};
*/
public class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        // first pass to have a clone node point to the next node. ie A->B becomes A->clonedNode->B
        Node curr = head;
        while (curr != null) {
            Node clonedNode = new Node(curr.val);
            clonedNode.next = curr.next;
            curr.next = clonedNode;
            curr = clonedNode.next;
        }
        curr = head;
        // second pass to make the cloned node's random pointer point to the orginal node's randome
        // pointer.
        // ie. A's random pointer becomes ClonedNode's random pointer
        while (curr != null) {
            if (curr.random != null) {
                curr.next.random = curr.random.next;
            } else {
                curr.next.random = null;
            }
            curr = curr.next.next;
        }
        curr = head;
        // third pass to restore the links and return the head of the cloned nodes' list.
        Node newHead = null;
        while (curr != null) {
            Node clonedNode;
            if (newHead == null) {
                clonedNode = curr.next;
                newHead = clonedNode;
            } else {
                clonedNode = curr.next;
            }
            curr.next = clonedNode.next;
            if (curr.next != null) {
                clonedNode.next = curr.next.next;
            } else {
                clonedNode.next = null;
            }
            curr = curr.next;
        }
        return newHead;
    }
}
```

**Time Complexity (Big O Time):**

1. **First Pass**: In the first pass, the program iterates through the original linked list and creates a cloned node for each node in the original list. This operation takes O(N) time, where N is the number of nodes in the original list, as it processes each node once.

2. **Second Pass**: In the second pass, the program updates the random pointers of the cloned nodes based on the random pointers of the original nodes. This operation also takes O(N) time since it processes each node once.

3. **Third Pass**: In the third pass, the program separates the cloned linked list from the original linked list and restores the original linked list's next pointers. This final pass also takes O(N) time, as it processes each node once.

Overall, the time complexity of the program is O(N), where N is the number of nodes in the linked list. All three passes contribute linearly to the time complexity.

**Space Complexity (Big O Space):**

The space complexity of the program is O(N), where N is the number of nodes in the linked list. Here's how the space is allocated:

1. **Cloned Nodes**: During the first pass, the program creates cloned nodes for each node in the original linked list. This results in N additional nodes, each taking constant space. Therefore, the space used for the cloned nodes is O(N).

2. **Random Pointers**: No additional space is used for random pointers since the program manipulates the existing nodes in the original linked list to connect to their corresponding cloned nodes.

3. **New Head Node**: A single variable `newHead` is used to store the head of the cloned linked list. This variable takes constant space.

4. **Other Variables**: The program uses a few additional variables (`curr`, `clonedNode`) to traverse and manipulate the linked list. These variables take constant space.

Overall, the space complexity is O(N) due to the cloned nodes created during the first pass, which directly corresponds to the number of nodes in the linked list.
