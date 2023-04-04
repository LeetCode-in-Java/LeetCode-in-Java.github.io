[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1195\. Fizz Buzz Multithreaded

Medium

You have the four functions:

*   `printFizz` that prints the word `"Fizz"` to the console,
*   `printBuzz` that prints the word `"Buzz"` to the console,
*   `printFizzBuzz` that prints the word `"FizzBuzz"` to the console, and
*   `printNumber` that prints a given integer to the console.

You are given an instance of the class `FizzBuzz` that has four functions: `fizz`, `buzz`, `fizzbuzz` and `number`. The same instance of `FizzBuzz` will be passed to four different threads:

*   **Thread A:** calls `fizz()` that should output the word `"Fizz"`.
*   **Thread B:** calls `buzz()` that should output the word `"Buzz"`.
*   **Thread C:** calls `fizzbuzz()` that should output the word `"FizzBuzz"`.
*   **Thread D:** calls `number()` that should only output the integers.

Modify the given class to output the series `[1, 2, "Fizz", 4, "Buzz", ...]` where the <code>i<sup>th</sup></code> token (**1-indexed**) of the series is:

*   `"FizzBuzz"` if `i` is divisible by `3` and `5`,
*   `"Fizz"` if `i` is divisible by `3` and not `5`,
*   `"Buzz"` if `i` is divisible by `5` and not `3`, or
*   `i` if `i` is not divisible by `3` or `5`.

Implement the `FizzBuzz` class:

*   `FizzBuzz(int n)` Initializes the object with the number `n` that represents the length of the sequence that should be printed.
*   `void fizz(printFizz)` Calls `printFizz` to output `"Fizz"`.
*   `void buzz(printBuzz)` Calls `printBuzz` to output `"Buzz"`.
*   `void fizzbuzz(printFizzBuzz)` Calls `printFizzBuzz` to output `"FizzBuzz"`.
*   `void number(printNumber)` Calls `printnumber` to output the numbers.

**Example 1:**

**Input:** n = 15

**Output:** [1,2,"fizz",4,"buzz","fizz",7,8,"fizz","buzz",11,"fizz",13,14,"fizzbuzz"]

**Example 2:**

**Input:** n = 5

**Output:** [1,2,"fizz",4,"buzz"]

**Constraints:**

*   `1 <= n <= 50`

## Solution

```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.IntConsumer;

@SuppressWarnings("java:S1130")
public class FizzBuzz {
    private final AtomicInteger count = new AtomicInteger(1);

    private final int n;

    public FizzBuzz(int n) {
        this.n = n;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        int i;
        while ((i = count.get()) <= n) {
            if (i % 3 == 0 && i % 5 != 0) {
                printFizz.run();
                count.compareAndSet(i, i + 1);
            }
        }
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        int i;
        while ((i = count.get()) <= n) {
            count.get();
            if (i % 5 == 0 && i % 3 != 0) {
                printBuzz.run();
                count.compareAndSet(i, i + 1);
            }
        }
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        int i;
        while ((i = count.get()) <= n) {
            if (i % 15 == 0) {
                printFizzBuzz.run();
                count.compareAndSet(i, i + 1);
            }
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        int i;
        while ((i = count.get()) <= n) {
            if (i % 5 != 0 && i % 3 != 0) {
                printNumber.accept(i);
                count.compareAndSet(i, i + 1);
            }
        }
    }
}
```