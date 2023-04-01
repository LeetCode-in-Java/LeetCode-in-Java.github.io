[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1825\. Finding MK Average

Hard

You are given two integers, `m` and `k`, and a stream of integers. You are tasked to implement a data structure that calculates the **MKAverage** for the stream.

The **MKAverage** can be calculated using these steps:

1.  If the number of the elements in the stream is less than `m` you should consider the **MKAverage** to be `-1`. Otherwise, copy the last `m` elements of the stream to a separate container.
2.  Remove the smallest `k` elements and the largest `k` elements from the container.
3.  Calculate the average value for the rest of the elements **rounded down to the nearest integer**.

Implement the `MKAverage` class:

*   `MKAverage(int m, int k)` Initializes the **MKAverage** object with an empty stream and the two integers `m` and `k`.
*   `void addElement(int num)` Inserts a new element `num` into the stream.
*   `int calculateMKAverage()` Calculates and returns the **MKAverage** for the current stream **rounded down to the nearest integer**.

**Example 1:**

**Input** ["MKAverage", "addElement", "addElement", "calculateMKAverage", "addElement", "calculateMKAverage", "addElement", "addElement", "addElement", "calculateMKAverage"] [[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]

**Output:** [null, null, null, -1, null, 3, null, null, null, 5]

**Explanation:** MKAverage obj = new MKAverage(3, 1); obj.addElement(3); // current elements are [3] obj.addElement(1); // current elements are [3,1] obj.calculateMKAverage(); // return -1, because m = 3 and only 2 elements exist. obj.addElement(10); // current elements are [3,1,10] obj.calculateMKAverage(); // The last 3 elements are [3,1,10]. // After removing smallest and largest 1 element the container will be ```[3]. // The average of [3] equals 3/1 = 3, return 3 obj.addElement(5); // current elements are [3,1,10,5] obj.addElement(5); // current elements are [3,1,10,5,5] obj.addElement(5); // current elements are [3,1,10,5,5,5] obj.calculateMKAverage(); // The last 3 elements are [5,5,5]. // After removing smallest and largest 1 element the container will be `[5]. // The average of [5] equals 5/1 = 5, return 5` ```

**Constraints:**

*   <code>3 <= m <= 10<sup>5</sup></code>
*   `1 <= k*2 < m`
*   <code>1 <= num <= 10<sup>5</sup></code>
*   At most <code>10<sup>5</sup></code> calls will be made to `addElement` and `calculateMKAverage`.

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.TreeMap;

@SuppressWarnings("java:S2184")
public class MKAverage {
    private final double m;
    private final double k;
    private final double c;
    private double avg;
    private final Bst middle;
    private final Bst min;
    private final Bst max;
    private final Deque<Integer> q;

    public MKAverage(int m, int k) {
        this.m = m;
        this.k = k;
        this.c = m - k * 2;
        this.avg = 0;
        this.middle = new Bst();
        this.min = new Bst();
        this.max = new Bst();
        this.q = new ArrayDeque<>();
    }

    public void addElement(int num) {
        if (min.size < k) {
            min.add(num);
            q.offer(num);
            return;
        }
        if (max.size < k) {
            min.add(num);
            max.add(min.removeMax());
            q.offer(num);
            return;
        }

        if (num >= min.lastKey() && num <= max.firstKey()) {
            middle.add(num);
            avg += num / c;
        } else if (num < min.lastKey()) {
            min.add(num);
            int val = min.removeMax();
            middle.add(val);
            avg += val / c;
        } else if (num > max.firstKey()) {
            max.add(num);
            int val = max.removeMin();
            middle.add(val);
            avg += val / c;
        }

        q.offer(num);

        if (q.size() > m) {
            num = q.poll();
            if (middle.containsKey(num)) {
                avg -= num / c;
                middle.remove(num);
            } else if (min.containsKey(num)) {
                min.remove(num);
                int val = middle.removeMin();
                avg -= val / c;
                min.add(val);
            } else if (max.containsKey(num)) {
                max.remove(num);
                int val = middle.removeMax();
                avg -= val / c;
                max.add(val);
            }
        }
    }

    public int calculateMKAverage() {
        if (q.size() < m) {
            return -1;
        }
        return (int) avg;
    }

    static class Bst {
        TreeMap<Integer, Integer> map;
        int size;

        public Bst() {
            this.map = new TreeMap<>();
            this.size = 0;
        }

        void add(int num) {
            int count = map.getOrDefault(num, 0) + 1;
            map.put(num, count);
            size++;
        }

        void remove(int num) {
            int count = map.getOrDefault(num, 1) - 1;
            if (count > 0) {
                map.put(num, count);
            } else {
                map.remove(num);
            }
            size--;
        }

        int removeMin() {
            int key = map.firstKey();

            remove(key);

            return key;
        }

        int removeMax() {
            int key = map.lastKey();

            remove(key);

            return key;
        }

        boolean containsKey(int key) {
            return map.containsKey(key);
        }

        int firstKey() {
            return map.firstKey();
        }

        int lastKey() {
            return map.lastKey();
        }
    }
}
```