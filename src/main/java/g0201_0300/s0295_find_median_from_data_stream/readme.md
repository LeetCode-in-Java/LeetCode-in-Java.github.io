[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 295\. Find Median from Data Stream

Hard

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

*   For example, for `arr = [2,3,4]`, the median is `3`.
*   For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

*   `MedianFinder()` initializes the `MedianFinder` object.
*   `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
*   `double findMedian()` returns the median of all elements so far. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input**

    ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
    [[], [1], [2], [], [3], []]

**Output:** [null, null, null, 1.5, null, 2.0]

**Explanation:**

    MedianFinder medianFinder = new MedianFinder();
    medianFinder.addNum(1); // arr = [1]
    medianFinder.addNum(2); // arr = [1, 2]
    medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
    medianFinder.addNum(3); // arr[1, 2, 3]
    medianFinder.findMedian(); // return 2.0 

**Constraints:**

*   <code>-10<sup>5</sup> <= num <= 10<sup>5</sup></code>
*   There will be at least one element in the data structure before calling `findMedian`.
*   At most <code>5 * 10<sup>4</sup></code> calls will be made to `addNum` and `findMedian`.

**Follow up:**

*   If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
*   If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

## Solution

```java
import java.util.PriorityQueue;

public class MedianFinder {
    // take two queues one is for storing upper half and the other is for lowerhalf
    // max stores the lower half
    // min heap stores the upper half
    private PriorityQueue<Integer> maxHeap;
    private PriorityQueue<Integer> minHeap;

    // initialize your data structure here.
    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> (b - a));
        minHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        if (maxHeap.isEmpty() || maxHeap.peek() > num) {
            maxHeap.offer(num);
        } else {
            minHeap.offer(num);
        }
        if (Math.abs(maxHeap.size() - minHeap.size()) > 1) {
            balance(maxHeap, minHeap);
        }
    }

    public void balance(PriorityQueue<Integer> maxHeap, PriorityQueue<Integer> minHeap) {
        PriorityQueue<Integer> large = maxHeap.size() > minHeap.size() ? maxHeap : minHeap;
        PriorityQueue<Integer> small = maxHeap.size() > minHeap.size() ? minHeap : maxHeap;
        small.offer(large.poll());
    }

    public double findMedian() {
        PriorityQueue<Integer> large = maxHeap.size() > minHeap.size() ? maxHeap : minHeap;
        PriorityQueue<Integer> small = maxHeap.size() > minHeap.size() ? minHeap : maxHeap;
        if (large.size() == small.size()) {
            return (double) (large.peek() + small.peek()) / 2;
        } else {
            return large.peek();
        }
    }
}

/*
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

**Time Complexity (Big O Time):**

1. The `addNum` method inserts an element into one of the two priority queues (maxHeap or minHeap) and balances the heaps if necessary. The balancing operation has a time complexity of O(log n), where 'n' is the number of elements in the larger heap. Since the elements are inserted one by one, the total time complexity for 'n' insertions is O(n * log n).

2. The `findMedian` method retrieves the median based on the sizes of the two heaps, which takes constant time O(1).

Overall, the time complexity of the program is O(n * log n) for 'n' insertions, where 'n' is the number of elements inserted.

**Space Complexity (Big O Space):**

1. The program uses two priority queues (`maxHeap` and `minHeap`) to store elements. The space complexity is determined by the space required for these priority queues. In the worst case, both heaps can store up to n/2 elements each, where 'n' is the number of elements inserted.

2. Therefore, the space complexity of the program is O(n/2 + n/2) = O(n).

In summary, the provided program has a time complexity of O(n * log n) for 'n' insertions and a space complexity of O(n) due to the storage of elements in two priority queues.
