## 855\. Exam Room

Medium

There is an exam room with `n` seats in a single row labeled from `0` to `n - 1`.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person. If there are multiple such seats, they sit in the seat with the lowest number. If no one is in the room, then the student sits at seat number `0`.

Design a class that simulates the mentioned exam room.

Implement the `ExamRoom` class:

*   `ExamRoom(int n)` Initializes the object of the exam room with the number of the seats `n`.
*   `int seat()` Returns the label of the seat at which the next student will set.
*   `void leave(int p)` Indicates that the student sitting at seat `p` will leave the room. It is guaranteed that there will be a student sitting at seat `p`.

**Example 1:**

**Input** ["ExamRoom", "seat", "seat", "seat", "seat", "leave", "seat"] [[10], [], [], [], [], [4], []]

**Output:** [null, 0, 9, 4, 2, null, 5]

**Explanation:** 

    ExamRoom examRoom = new ExamRoom(10);
    examRoom.seat(); // return 0, no one is in the room, then the student sits at seat number 0. 
    examRoom.seat(); // return 9, the student sits at the last seat number 9. 
    examRoom.seat(); // return 4, the student sits at the last seat number 4. 
    examRoom.seat(); // return 2, the student sits at the last seat number 2. 
    examRoom.leave(4); 
    examRoom.seat(); // return 5, the student sits at the last seat number 5.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   It is guaranteed that there is a student sitting at seat `p`.
*   At most <code>10<sup>4</sup></code> calls will be made to `seat` and `leave`.

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

public class ExamRoom {
    private static class Node {
        Node pre;
        Node next;
        int val;

        Node(int val, Map<Integer, Node> map) {
            this.val = val;
            map.put(val, this);
        }

        int insert(Node left) {
            Node right = left.next;
            left.next = this;
            right.pre = this;
            this.next = right;
            this.pre = left;
            return val;
        }

        void delete() {
            Node left = this.pre;
            Node right = this.next;
            left.next = right;
            right.pre = left;
        }
    }

    private Map<Integer, Node> map = new HashMap<>();
    private Node head = new Node(-1, map);
    private Node tail = new Node(-1, map);
    private int n;

    public ExamRoom() {
        head.next = tail;
        tail.pre = head;
    }

    public ExamRoom(int n) {
        this();
        this.n = n;
    }

    public int seat() {
        int right = n - 1 - tail.pre.val;
        int left = head.next.val;
        int max = 0;
        int maxAt = -1;
        Node maxAtLeft = null;
        for (Node cur = tail.pre; cur != head && cur.pre != head; cur = cur.pre) {
            Node pre = cur.pre;
            int at = (cur.val + pre.val) / 2;
            int distance = at - pre.val;
            if (distance >= max) {
                maxAtLeft = pre;
                max = distance;
                maxAt = at;
            }
        }
        if (head.next == tail || left >= max && left >= right) {
            return new Node(0, map).insert(head);
        }
        if (right > max) {
            return new Node(n - 1, map).insert(tail.pre);
        }
        return new Node(maxAt, map).insert(Objects.requireNonNull(maxAtLeft));
    }

    public void leave(int p) {
        map.get(p).delete();
        map.remove(p);
    }
}

/*
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom obj = new ExamRoom(n);
 * int param_1 = obj.seat();
 * obj.leave(p);
 */
```