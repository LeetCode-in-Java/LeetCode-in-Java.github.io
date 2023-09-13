[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 146\. LRU Cache

Medium

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

*   `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
*   `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
*   `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input** ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"] [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

**Output:** [null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation:**

    LRUCache lRUCache = new LRUCache(2);
    lRUCache.put(1, 1); // cache is {1=1}
    lRUCache.put(2, 2); // cache is {1=1, 2=2}
    lRUCache.get(1); // return 1
    lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
    lRUCache.get(2); // returns -1 (not found)
    lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
    lRUCache.get(1); // return -1 (not found)
    lRUCache.get(3); // return 3
    lRUCache.get(4); // return 4 

**Constraints:**

*   `1 <= capacity <= 3000`
*   <code>0 <= key <= 10<sup>4</sup></code>
*   <code>0 <= value <= 10<sup>5</sup></code>
*   At most 2<code> * 10<sup>5</sup></code> calls will be made to `get` and `put`.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class LRUCache {
    private static class LruCacheNode {
        int key;
        int value;
        LruCacheNode prev;
        LruCacheNode next;

        public LruCacheNode(int k, int v) {
            key = k;
            value = v;
        }
    }

    private int capacity;
    private Map<Integer, LruCacheNode> cacheMap = new HashMap<>();
    // insert here
    private LruCacheNode head;
    // remove here
    private LruCacheNode tail;

    public LRUCache(int cap) {
        capacity = cap;
    }

    public int get(int key) {
        LruCacheNode val = cacheMap.get(key);
        if (val == null) {
            return -1;
        }
        moveToHead(val);
        return val.value;
    }

    /*
     * Scenarios :
     * 1. Value key is already present.
     *          update
     *          move node to head
     * 2. cache is not full
     *          cache is empty (create node and assign head and tail)
     *          cache is partially empty (add node to head and update head pointer)
     * 3. cache is full
     *          remove node at tail, update head, tail pointers
     *          Recursively call put
     *
     *
     * move node to head Scenarios
     * 1. node is at head
     * 2. node is at tail
     * 3. node is in middle
     *
     */
    public void put(int key, int value) {
        LruCacheNode valNode = cacheMap.get(key);
        if (valNode != null) {
            valNode.value = value;
            moveToHead(valNode);
        } else {
            if (cacheMap.size() < capacity) {
                if (cacheMap.size() == 0) {
                    LruCacheNode node = new LruCacheNode(key, value);
                    cacheMap.put(key, node);
                    head = node;
                    tail = node;
                } else {
                    LruCacheNode node = new LruCacheNode(key, value);
                    cacheMap.put(key, node);
                    node.next = head;
                    head.prev = node;
                    head = node;
                }
            } else {
                // remove from tail
                LruCacheNode last = tail;
                tail = last.prev;
                if (tail != null) {
                    tail.next = null;
                }
                cacheMap.remove(last.key);
                if (cacheMap.size() == 0) {
                    head = null;
                }
                // Call recursively
                put(key, value);
            }
        }
    }

    /*
     * check for 3 conditions
     * 1. node is already at head
     * 2. Node is tail node
     * 3. Node in middle node
     */
    private void moveToHead(LruCacheNode node) {
        if (node == head) {
            return;
        }
        if (node == tail) {
            tail = node.prev;
        }
        // node is not head, it should have some valid prev node
        LruCacheNode prev = node.prev;
        LruCacheNode next = node.next;
        prev.next = next;
        if (next != null) {
            next.prev = prev;
        }
        node.prev = null;
        node.next = head;
        head.prev = node;
        head = node;
    }
}

/*
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

**Time Complexity (Big O Time):**

1. `get(int key)` Method:
   - Accessing an element from the `cacheMap` is done in constant time (O(1)).
   - Moving a node to the head in the `moveToHead` method also operates in constant time (O(1)).
   - Overall, the `get` method's time complexity is O(1).

2. `put(int key, int value)` Method:
   - Updating the value of an existing key in the `cacheMap` is done in constant time (O(1)).
   - Adding a new node to the cache is done in constant time if the cache is not full (O(1)).
   - If the cache is full, it removes the node at the tail, which is also done in constant time (O(1)).
   - Recursively calling the `put` method when the cache is full does not result in nested loops because the eviction operation happens once in each recursion.
   - Overall, the `put` method's time complexity is O(1) on average, but in the worst case (when eviction happens multiple times), it can be considered O(N), where N is the capacity of the cache.

3. `moveToHead(LruCacheNode node)` Method:
   - Moving a node to the head of the cache is done in constant time (O(1)) because it involves changing pointers.
   - The time complexity of this method is O(1).

Overall, the time complexity of most operations in the LRU cache is O(1), except for the `put` method in the worst-case scenario.

**Space Complexity (Big O Space):**

The space complexity of this LRU cache implementation is O(capacity), where "capacity" represents the maximum number of key-value pairs the cache can store. The primary space usage comes from the `cacheMap`, which stores the key-value pairs, and the linked list of nodes (`head` and `tail`) used to maintain the order of access. Additionally, each node in the linked list consumes space proportional to the key and value sizes.

In summary, the space complexity of the LRU cache is determined by its capacity, and the primary space-consuming data structure is the `cacheMap`, resulting in an overall space complexity of O(capacity).
