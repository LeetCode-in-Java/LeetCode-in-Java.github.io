[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1116\. Print Zero Even Odd

Medium

You have a function `printNumber` that can be called with an integer parameter and prints it to the console.

*   For example, calling `printNumber(7)` prints `7` to the console.

You are given an instance of the class `ZeroEvenOdd` that has three functions: `zero`, `even`, and `odd`. The same instance of `ZeroEvenOdd` will be passed to three different threads:

*   **Thread A:** calls `zero()` that should only output `0`'s.
*   **Thread B:** calls `even()` that should only output even numbers.
*   **Thread C:** calls `odd()` that should only output odd numbers.

Modify the given class to output the series `"010203040506..."` where the length of the series must be `2n`.

Implement the `ZeroEvenOdd` class:

*   `ZeroEvenOdd(int n)` Initializes the object with the number `n` that represents the numbers that should be printed.
*   `void zero(printNumber)` Calls `printNumber` to output one zero.
*   `void even(printNumber)` Calls `printNumber` to output one even number.
*   `void odd(printNumber)` Calls `printNumber` to output one odd number.

**Example 1:**

**Input:** n = 2

**Output:** "0102"

**Explanation:** There are three threads being fired asynchronously. One of them calls zero(), the other calls even(), and the last one calls odd(). "0102" is the correct output.

**Example 2:**

**Input:** n = 5

**Output:** "0102030405"

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
import java.util.concurrent.Semaphore;
import java.util.function.IntConsumer;

@SuppressWarnings({"java:S2142", "java:S2583"})
public class ZeroEvenOdd {
    private int n;

    private final Semaphore zeroSemaphore = new Semaphore(1);
    private final Semaphore oddSemaphore = new Semaphore(1);
    private final Semaphore evenSemaphore = new Semaphore(1);

    public ZeroEvenOdd(int n) {
        this.n = n;
        try {
            oddSemaphore.acquire();
            evenSemaphore.acquire();
        } catch (InterruptedException ignored) {
            // ignored
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i++) {
            zeroSemaphore.acquire();
            printNumber.accept(0);
            if (i % 2 == 0) {
                oddSemaphore.release();
            } else {
                evenSemaphore.release();
            }
        }
        oddSemaphore.release();
        evenSemaphore.release();
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i += 2) {
            evenSemaphore.acquire();
            if (i > n) {
                zeroSemaphore.release();
                evenSemaphore.release();
                break;
            }
            printNumber.accept(i);
            zeroSemaphore.release();
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for (int i = 2; i <= n; i += 2) {
            oddSemaphore.acquire();
            if (i > n) {
                zeroSemaphore.release();
                oddSemaphore.release();
                break;
            }
            printNumber.accept(i);
            zeroSemaphore.release();
        }
    }
}
```