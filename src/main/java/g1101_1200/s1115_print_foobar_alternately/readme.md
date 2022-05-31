## 1115\. Print FooBar Alternately

Medium

Suppose you are given the following code:

class FooBar { public void foo() { for (int i = 0; i < n; i++) { print("foo"); } } public void bar() { for (int i = 0; i < n; i++) { print("bar"); } } }

The same instance of `FooBar` will be passed to two different threads:

*   thread `A` will call `foo()`, while
*   thread `B` will call `bar()`.

Modify the given program to output `"foobar"` `n` times.

**Example 1:**

**Input:** n = 1

**Output:** "foobar"

**Explanation:** There are two threads being fired asynchronously. One of them calls foo(), while the other calls bar(). "foobar" is being output 1 time.

**Example 2:**

**Input:** n = 2

**Output:** "foobarfoobar"

**Explanation:** "foobar" is being output 2 times.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
import java.util.concurrent.Semaphore;

@SuppressWarnings("java:S2142")
public class FooBar {
    private int n;

    private final Semaphore fooSemaphore;
    private final Semaphore barSemaphore;

    public FooBar(int n) {
        this.n = n;
        fooSemaphore = new Semaphore(1);
        barSemaphore = new Semaphore(1);
        try {
            barSemaphore.acquire();
        } catch (InterruptedException ignored) {
            // ignored
        }
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            fooSemaphore.acquire();
            // printFoo.run() outputs "foo". Do not change or remove this line.
            printFoo.run();
            barSemaphore.release();
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            barSemaphore.acquire();
            // printBar.run() outputs "bar". Do not change or remove this line.
            printBar.run();
            fooSemaphore.release();
        }
    }
}
```