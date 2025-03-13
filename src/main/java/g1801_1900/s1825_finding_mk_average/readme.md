[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
import java.util.LinkedList;
import java.util.TreeSet;

public class MKAverage {
    private final int capacity;
    private final int boundary;
    private final int[] nums;
    private final TreeSet<Integer> numSet;
    private final LinkedList<Integer> order;

    public MKAverage(int m, int k) {
        this.capacity = m;
        this.boundary = k;
        nums = new int[100001];
        numSet = new TreeSet<>();
        order = new LinkedList<>();
    }

    public void addElement(int num) {
        if (order.size() == capacity) {
            int numToDelete = order.removeFirst();
            nums[numToDelete] = nums[numToDelete] - 1;
            if (nums[numToDelete] == 0) {
                numSet.remove(numToDelete);
            }
        }
        nums[num]++;
        numSet.add(num);
        order.add(num);
    }

    public int calculateMKAverage() {
        if (order.size() == capacity) {
            int skipCount = boundary;
            int numsCount = capacity - 2 * boundary;
            int freq = capacity - 2 * boundary;
            int sum = 0;
            for (int num : numSet) {
                int count = nums[num];
                if (skipCount < 0) {
                    sum += num * Math.min(count, numsCount);
                    numsCount -= Math.min(count, numsCount);
                } else {
                    skipCount -= count;
                    if (skipCount < 0) {
                        sum += num * Math.min(Math.abs(skipCount), numsCount);
                        numsCount -= Math.min(Math.abs(skipCount), numsCount);
                    }
                }
                if (numsCount == 0) {
                    break;
                }
            }
            return sum / freq;
        }
        return -1;
    }
}

/*
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage obj = new MKAverage(m, k);
 * obj.addElement(num);
 * int param_2 = obj.calculateMKAverage();
 */
```