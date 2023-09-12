[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2671\. Frequency Tracker

Medium

Design a data structure that keeps track of the values in it and answers some queries regarding their frequencies.

Implement the `FrequencyTracker` class.

*   `FrequencyTracker()`: Initializes the `FrequencyTracker` object with an empty array initially.
*   `void add(int number)`: Adds `number` to the data structure.
*   `void deleteOne(int number)`: Deletes **one** occurrence of `number` from the data structure. The data structure **may not contain** `number`, and in this case nothing is deleted.
*   `bool hasFrequency(int frequency)`: Returns `true` if there is a number in the data structure that occurs `frequency` number of times, otherwise, it returns `false`.

**Example 1:**

**Input** ["FrequencyTracker", "add", "add", "hasFrequency"] [[], [3], [3], [2]]

**Output:** [null, null, null, true]

**Explanation:** 

    FrequencyTracker frequencyTracker = new FrequencyTracker(); 
    frequencyTracker.add(3); // The data structure now contains [3] 
    frequencyTracker.add(3); // The data structure now contains [3, 3] 
    frequencyTracker.hasFrequency(2); // Returns true, because 3 occurs twice

**Example 2:**

**Input** ["FrequencyTracker", "add", "deleteOne", "hasFrequency"] [[], [1], [1], [1]]

**Output:** [null, null, null, false]

**Explanation:** 

    FrequencyTracker frequencyTracker = new FrequencyTracker(); 
    frequencyTracker.add(1); // The data structure now contains [1]
    frequencyTracker.deleteOne(1); // The data structure becomes empty [] 
    frequencyTracker.hasFrequency(1); // Returns false, because the data structure is empty

**Example 3:**

**Input** ["FrequencyTracker", "hasFrequency", "add", "hasFrequency"] [[], [2], [3], [1]]

**Output:** [null, false, null, true]

**Explanation:** 

    FrequencyTracker frequencyTracker = new FrequencyTracker(); 
    frequencyTracker.hasFrequency(2); // Returns false, because the data structure is empty 
    frequencyTracker.add(3); // The data structure now contains [3] 
    frequencyTracker.hasFrequency(1); // Returns true, because 3 occurs once

**Constraints:**

*   <code>1 <= number <= 10<sup>5</sup></code>
*   <code>1 <= frequency <= 10<sup>5</sup></code>
*   At most, <code>2 * 10<sup>5</sup></code> calls will be made to `add`, `deleteOne`, and `hasFrequency` in **total**.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class FrequencyTracker {
    private final Map<Integer, Integer> numCount;
    private final Map<Integer, Integer> frequencies;

    public FrequencyTracker() {
        numCount = new HashMap<>();
        frequencies = new HashMap<>();
    }

    public void add(int number) {
        int newCount = numCount.merge(number, 1, Integer::sum);
        frequencies.merge(newCount, 1, Integer::sum);
        if (newCount > 1) {
            frequencies.merge(newCount - 1, -1, Integer::sum);
        }
    }

    public void deleteOne(int number) {
        Integer currentVal = numCount.get(number);
        if (currentVal != null && currentVal > 0) {
            int newCount = numCount.merge(number, -1, Integer::sum);
            frequencies.merge(newCount, 1, Integer::sum);
            frequencies.merge(newCount + 1, -1, Integer::sum);
        }
    }

    public boolean hasFrequency(int frequency) {
        Integer existing = frequencies.get(frequency);
        return existing != null && existing > 0;
    }
}

/*
 * Your FrequencyTracker object will be instantiated and called as such:
 * FrequencyTracker obj = new FrequencyTracker();
 * obj.add(number);
 * obj.deleteOne(number);
 * boolean param_3 = obj.hasFrequency(frequency);
 */
```