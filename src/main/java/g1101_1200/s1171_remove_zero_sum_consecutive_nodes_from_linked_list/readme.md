[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1171\. Remove Zero Sum Consecutive Nodes from Linked List

Medium

Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list. You may return any such answer.

(Note that in the examples below, all sequences are serializations of `ListNode` objects.)

**Example 1:**

**Input:** head = [1,2,-3,3,1]

**Output:** [3,1] **Note:** The answer [1,2,1] would also be accepted.

**Example 2:**

**Input:** head = [1,2,3,-3,4]

**Output:** [1,2,4]

**Example 3:**

**Input:** head = [1,2,3,-3,-2]

**Output:** [1]

**Constraints:**

*   The given linked list will contain between `1` and `1000` nodes.
*   Each node in the linked list has `-1000 <= node.val <= 1000`.

## Solution

```java
import com_github_leetcode.ListNode;
import java.util.HashMap;
import java.util.Map;

/*
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode pre = new ListNode(-1);
        ListNode curr = pre;
        pre.next = head;
        Map<Integer, ListNode> map = new HashMap<>();
        int preSum = 0;
        while (curr != null) {
            preSum += curr.val;
            if (map.containsKey(preSum)) {
                curr = map.get(preSum).next;
                int key = preSum + curr.val;
                while (key != preSum) {
                    map.remove(key);
                    curr = curr.next;
                    key += curr.val;
                }
                map.get(preSum).next = curr.next;
            } else {
                map.put(preSum, curr);
            }
            curr = curr.next;
        }
        return pre.next;
    }
}
```