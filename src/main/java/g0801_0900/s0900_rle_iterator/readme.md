[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 900\. RLE Iterator

Medium

We can use run-length encoding (i.e., **RLE**) to encode a sequence of integers. In a run-length encoded array of even length `encoding` (**0-indexed**), for all even `i`, `encoding[i]` tells us the number of times that the non-negative integer value `encoding[i + 1]` is repeated in the sequence.

*   For example, the sequence `arr = [8,8,8,5,5]` can be encoded to be `encoding = [3,8,2,5]`. `encoding = [3,8,0,9,2,5]` and `encoding = [2,8,1,8,2,5]` are also valid **RLE** of `arr`.

Given a run-length encoded array, design an iterator that iterates through it.

Implement the `RLEIterator` class:

*   `RLEIterator(int[] encoded)` Initializes the object with the encoded array `encoded`.
*   `int next(int n)` Exhausts the next `n` elements and returns the last element exhausted in this way. If there is no element left to exhaust, return `-1` instead.

**Example 1:**

**Input**

["RLEIterator", "next", "next", "next", "next"]

[[[3, 8, 0, 9, 2, 5]], [2], [1], [1], [2]]

**Output:** [null, 8, 8, 5, -1]

**Explanation:**

    RLEIterator rLEIterator = new RLEIterator([3, 8, 0, 9, 2, 5]); // This maps to the sequence [8,8,8,5,5].
    rLEIterator.next(2); // exhausts 2 terms of the sequence, returning 8. The remaining sequence is now [8, 5, 5].
    rLEIterator.next(1); // exhausts 1 term of the sequence, returning 8. The remaining sequence is now [5, 5].
    rLEIterator.next(1); // exhausts 1 term of the sequence, returning 5. The remaining sequence is now [5].
    rLEIterator.next(2); // exhausts 2 terms, returning -1. This is because the first term exhausted was 5,
    // but the second term did not exist. Since the last term exhausted does not exist, we return -1. 

**Constraints:**

*   `2 <= encoding.length <= 1000`
*   `encoding.length` is even.
*   <code>0 <= encoding[i] <= 10<sup>9</sup></code>
*   <code>1 <= n <= 10<sup>9</sup></code>
*   At most `1000` calls will be made to `next`.

## Solution

```java
@SuppressWarnings("java:S135")
public class RLEIterator {
    private int index;
    private final int[] array;

    public RLEIterator(int[] encoding) {
        index = 0;
        array = encoding;
    }

    public int next(int n) {
        int lastElement = -1;
        while (n > 0 && index < array.length) {
            if (array[index] > n) {
                array[index] -= n;
                lastElement = array[index + 1];
                break;
            } else if (array[index] == n) {
                array[index] = 0;
                lastElement = array[index + 1];
                index += 2;
                break;
            } else {
                n -= array[index];
                index += 2;
            }
        }
        return lastElement;
    }
}
```