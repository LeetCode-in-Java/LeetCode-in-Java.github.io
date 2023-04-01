[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 380\. Insert Delete GetRandom O(1)

Medium

Implement the `RandomizedSet` class:

*   `RandomizedSet()` Initializes the `RandomizedSet` object.
*   `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
*   `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
*   `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

**Example 1:**

**Input** 

    ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"] 
    [[], [1], [2], [2], [], [1], [2], []]

**Output:** [null, true, false, true, 2, true, false, 2]

**Explanation:** 

    RandomizedSet randomizedSet = new RandomizedSet(); 
    randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully. 
    randomizedSet.remove(2); // Returns false as 2 does not exist in the set. 
    randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2]. 
    randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly. 
    randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2]. 
    randomizedSet.insert(2); // 2 was already in the set, so return false. 
    randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.

**Constraints:**

*   <code>-2<sup>31</sup> <= val <= 2<sup>31</sup> - 1</code>
*   At most `2 * `<code>10<sup>5</sup></code> calls will be made to `insert`, `remove`, and `getRandom`.
*   There will be **at least one** element in the data structure when `getRandom` is called.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Random;

@SuppressWarnings("java:S2245")
public class RandomizedSet {
    private final Random rand;
    private final List<Integer> list;
    private final Map<Integer, Integer> map;

    /* Initialize your data structure here. */
    public RandomizedSet() {
        this.rand = new Random();
        this.list = new ArrayList<>();
        this.map = new HashMap<>();
    }

    /* Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.containsKey(val)) {
            return false;
        }
        map.put(val, list.size());
        list.add(val);

        return true;
    }

    /* Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) {
            return false;
        }
        int swap1 = map.get(val);
        int swap2 = list.size() - 1;
        int val2 = list.get(swap2);
        map.put(val2, swap1);
        map.remove(val);
        list.set(swap1, val2);
        list.remove(list.size() - 1);
        return true;
    }

    /* Get a random element from the set. */
    public int getRandom() {
        return list.get(rand.nextInt(list.size()));
    }
}

/*
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```