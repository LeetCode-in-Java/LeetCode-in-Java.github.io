## 707\. Design Linked List

Medium

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.  
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.  
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

*   `MyLinkedList()` Initializes the `MyLinkedList` object.
*   `int get(int index)` Get the value of the <code>index<sup>th</sup></code> node in the linked list. If the index is invalid, return `-1`.
*   `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
*   `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
*   `void addAtIndex(int index, int val)` Add a node of value `val` before the <code>index<sup>th</sup></code> node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
*   `void deleteAtIndex(int index)` Delete the <code>index<sup>th</sup></code> node in the linked list, if the index is valid.

**Example 1:**

**Input** 

["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"] 

[[], [1], [3], [1, 2], [1], [1], [1]]

**Output:** [null, null, null, null, 2, null, 3]

**Explanation:** 

    MyLinkedList myLinkedList = new MyLinkedList(); 
    myLinkedList.addAtHead(1); 
    myLinkedList.addAtTail(3); 
    myLinkedList.addAtIndex(1, 2); // linked list becomes 1->2->3 
    myLinkedList.get(1); // return 2 
    myLinkedList.deleteAtIndex(1); // now the linked list is 1->3 
    myLinkedList.get(1); // return 3

**Constraints:**

*   `0 <= index, val <= 1000`
*   Please do not use the built-in LinkedList library.
*   At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.

## Solution

```java
public class MyLinkedList {
    private static class Node {
        int val;
        Node next;

        Node(int val) {
            this.val = val;
            this.next = null;
        }
    }

    private Node head;
    private Node tail;
    private int size;

    public MyLinkedList() {
        this.head = this.tail = null;
        this.size = 0;
    }

    public int get(int index) {
        if (index >= size) {
            return -1;
        }
        if (index == 0) {
            return head.val;
        }
        int i = 0;
        Node ptr = head;
        while (i++ < index - 1) {
            ptr = ptr.next;
        }
        return ptr.next.val;
    }

    public void addAtHead(int val) {
        Node node = new Node(val);
        if (head == null) {
            head = tail = node;
            size++;
            return;
        }
        node.next = head;
        head = node;
        size++;
    }

    public void addAtTail(int val) {
        if (head == null) {
            addAtHead(val);
            return;
        }
        Node node = new Node(val);
        tail.next = node;
        tail = node;
        size++;
    }

    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index == 0) {
            addAtHead(val);
            return;
        }
        if (index == size) {
            addAtTail(val);
            return;
        }
        int i = 0;
        Node node = new Node(val);
        Node ptr = head;
        while (i++ < index - 1) {
            ptr = ptr.next;
        }
        node.next = ptr.next;
        ptr.next = node;
        size++;
    }

    public void deleteAtIndex(int index) {
        if (index >= size) {
            return;
        }
        if (index == 0) {
            head = head.next;
            size--;
            return;
        }
        int i = 0;
        Node ptr = head;
        while (i++ < index - 1) {
            ptr = ptr.next;
        }
        ptr.next = ptr.next.next;
        if (index == size - 1) {
            tail = ptr;
        }
        size--;
    }
}

/*
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```