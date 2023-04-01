[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 706\. Design HashMap

Easy

Design a HashMap without using any built-in hash table libraries.

Implement the `MyHashMap` class:

*   `MyHashMap()` initializes the object with an empty map.
*   `void put(int key, int value)` inserts a `(key, value)` pair into the HashMap. If the `key` already exists in the map, update the corresponding `value`.
*   `int get(int key)` returns the `value` to which the specified `key` is mapped, or `-1` if this map contains no mapping for the `key`.
*   `void remove(key)` removes the `key` and its corresponding `value` if the map contains the mapping for the `key`.

**Example 1:**

**Input** 

["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"] 

[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]

**Output:** [null, null, null, 1, -1, null, 1, null, -1]

**Explanation:** 

    MyHashMap myHashMap = new MyHashMap(); 
    myHashMap.put(1, 1); // The map is now [[1,1]] 
    myHashMap.put(2, 2); // The map is now [[1,1], [2,2]] 
    myHashMap.get(1); // return 1, The map is now [[1,1], [2,2]] 
    myHashMap.get(3); // return -1 (i.e., not found), The map is now [[1,1], [2,2]] 
    myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value) 
    myHashMap.get(2); // return 1, The map is now [[1,1], [2,1]] 
    myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]] 
    myHashMap.get(2); // return -1 (i.e., not found), The map is now [[1,1]]

**Constraints:**

*   <code>0 <= key, value <= 10<sup>6</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `put`, `get`, and `remove`.

## Solution

```java
import java.util.ArrayList;

@SuppressWarnings("unchecked")
public class MyHashMap {
    private ArrayList[] arr = null;

    public MyHashMap() {
        arr = new ArrayList[1000];
    }

    public void put(int key, int value) {
        int bucket = key % 1000;
        if (arr[bucket] == null) {
            ArrayList<Entry> list = new ArrayList<>();
            Entry e = new Entry();
            e.key = key;
            e.value = value;
            list.add(e);
            arr[bucket] = list;
        } else {
            ArrayList<Entry> list = arr[bucket];
            Entry e = new Entry();
            e.key = key;
            e.value = value;
            list.remove(e);
            list.add(e);
        }
    }

    public int get(int key) {
        int bucket = key % 1000;
        int ans = -1;
        ArrayList<Entry> list = arr[bucket];
        if (list != null) {
            for (Entry e : list) {
                if (e.key == key) {
                    ans = e.value;
                }
            }
        }
        return ans;
    }

    public void remove(int key) {
        int bucket = key % 1000;
        ArrayList<Entry> list = arr[bucket];
        Entry e = new Entry();
        e.key = key;
        if (list != null) {
            list.remove(e);
        }
    }

    static class Entry {
        int key;
        int value;

        @Override
        public int hashCode() {
            final int prime = 31;
            int result = 1;
            result = prime * result + key;
            return result;
        }

        @Override
        public boolean equals(Object obj) {
            if (this == obj) {
                return true;
            }
            if (obj == null) {
                return false;
            }
            if (getClass() != obj.getClass()) {
                return false;
            }
            Entry other = (Entry) obj;
            return key == other.key;
        }
    }
}
```