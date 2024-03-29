[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1146\. Snapshot Array

Medium

Implement a SnapshotArray that supports the following interface:

*   `SnapshotArray(int length)` initializes an array-like data structure with the given length. **Initially, each element equals 0**.
*   `void set(index, val)` sets the element at the given `index` to be equal to `val`.
*   `int snap()` takes a snapshot of the array and returns the `snap_id`: the total number of times we called `snap()` minus `1`.
*   `int get(index, snap_id)` returns the value at the given `index`, at the time we took the snapshot with the given `snap_id`

**Example 1:**

**Input:** ["SnapshotArray","set","snap","set","get"] [[3],[0,5],[],[0,6],[0,0]]

**Output:** [null,null,0,null,5]

**Explanation:**  

SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3 
snapshotArr.set(0,5); // Set array[0] = 5 
snapshotArr.snap(); // Take a snapshot, return snap_id = 0 
snapshotArr.set(0,6); 
snapshotArr.get(0,0); // Get the value of array[0] with snap_id = 0, return 5

**Constraints:**

*   `1 <= length <= 50000`
*   At most `50000` calls will be made to `set`, `snap`, and `get`.
*   `0 <= index < length`
*   `0 <= snap_id < `(the total number of times we call `snap()`)
*   `0 <= val <= 10^9`

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.TreeMap;

public class SnapshotArray {
    private int snapId = -1;
    private final Map<Integer, TreeMap<Integer, Integer>> indexToSnapMap;
    private final int[] ar;

    public SnapshotArray(int length) {
        indexToSnapMap = new HashMap<>();
        ar = new int[length];
    }

    public void set(int index, int val) {
        if (indexToSnapMap.containsKey(index)) {
            if (!indexToSnapMap.get(index).containsKey(snapId)) {
                indexToSnapMap.get(index).put(snapId, ar[index]);
            }
        } else {
            TreeMap<Integer, Integer> snapToValueMap = new TreeMap<>();
            snapToValueMap.put(snapId, ar[index]);
            indexToSnapMap.put(index, snapToValueMap);
        }
        ar[index] = val;
    }

    public int snap() {
        snapId++;
        return snapId;
    }

    public int get(int index, int snapId) {
        if (indexToSnapMap.containsKey(index)) {
            Map.Entry<Integer, Integer> value = indexToSnapMap.get(index).ceilingEntry(snapId);
            if (value != null) {
                return value.getValue();
            }
        }
        return ar[index];
    }
}

/*
 * Your SnapshotArray object will be instantiated and called as such:
 * SnapshotArray obj = new SnapshotArray(length);
 * obj.set(index,val);
 * int param_2 = obj.snap();
 * int param_3 = obj.get(index,snap_id);
 */
```