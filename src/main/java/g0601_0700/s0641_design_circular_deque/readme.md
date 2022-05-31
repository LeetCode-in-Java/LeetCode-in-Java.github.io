## 641\. Design Circular Deque

Medium

Design your implementation of the circular double-ended queue (deque).

Implement the `MyCircularDeque` class:

*   `MyCircularDeque(int k)` Initializes the deque with a maximum size of `k`.
*   `boolean insertFront()` Adds an item at the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean insertLast()` Adds an item at the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean deleteFront()` Deletes an item from the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean deleteLast()` Deletes an item from the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `int getFront()` Returns the front item from the Deque. Returns `-1` if the deque is empty.
*   `int getRear()` Returns the last item from Deque. Returns `-1` if the deque is empty.
*   `boolean isEmpty()` Returns `true` if the deque is empty, or `false` otherwise.
*   `boolean isFull()` Returns `true` if the deque is full, or `false` otherwise.

**Example 1:**

**Input** 

["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"] 

[[3], [1], [2], [3], [4], [], [], [], [4], []]

**Output:** [null, true, true, true, false, 2, true, true, true, 4]

**Explanation:** 

MyCircularDeque myCircularDeque = new MyCircularDeque(3); 

myCircularDeque.insertLast(1); // return True 

myCircularDeque.insertLast(2); // return True 

myCircularDeque.insertFront(3); // return True 

myCircularDeque.insertFront(4); // return False, the queue is full. 

myCircularDeque.getRear(); // return 2 

myCircularDeque.isFull(); // return True 

myCircularDeque.deleteLast(); // return True 

myCircularDeque.insertFront(4); // return True 

myCircularDeque.getFront(); // return 4

**Constraints:**

*   `1 <= k <= 1000`
*   `0 <= value <= 1000`
*   At most `2000` calls will be made to `insertFront`, `insertLast`, `deleteFront`, `deleteLast`, `getFront`, `getRear`, `isEmpty`, `isFull`.

## Solution

```java
public class MyCircularDeque {
    private final int[] data;
    private int front;
    private int rear;
    private int size;

    public MyCircularDeque(int k) {
        data = new int[k];
        front = 0;
        rear = k - 1;
        size = 0;
    }

    public boolean insertFront(int value) {
        if (size == data.length) {
            return false;
        }
        data[front] = value;
        front = (front + 1) % data.length;
        size++;
        return true;
    }

    public boolean insertLast(int value) {
        if (size == data.length) {
            return false;
        }
        data[rear] = value;
        rear = (rear - 1 + data.length) % data.length;
        size++;
        return true;
    }

    public boolean deleteFront() {
        if (size == 0) {
            return false;
        }
        front = (front - 1 + data.length) % data.length;
        size--;
        return true;
    }

    public boolean deleteLast() {
        if (size == 0) {
            return false;
        }
        rear = (rear + 1) % data.length;
        size--;
        return true;
    }

    public int getFront() {
        if (size == 0) {
            return -1;
        }

        return data[(front - 1 + data.length) % data.length];
    }

    public int getRear() {
        if (size == 0) {
            return -1;
        }
        return data[(rear + 1) % data.length];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == data.length;
    }
}

/*
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
```