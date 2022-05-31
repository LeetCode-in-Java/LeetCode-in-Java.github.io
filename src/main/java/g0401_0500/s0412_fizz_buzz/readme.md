## 412\. Fizz Buzz

Easy

Given an integer `n`, return _a string array_ `answer` _(**1-indexed**) where_:

*   `answer[i] == "FizzBuzz"` if `i` is divisible by `3` and `5`.
*   `answer[i] == "Fizz"` if `i` is divisible by `3`.
*   `answer[i] == "Buzz"` if `i` is divisible by `5`.
*   `answer[i] == i` (as a string) if none of the above conditions are true.

**Example 1:**

**Input:** n = 3

**Output:** ["1","2","Fizz"] 

**Example 2:**

**Input:** n = 5

**Output:** ["1","2","Fizz","4","Buzz"] 

**Example 3:**

**Input:** n = 15

**Output:** ["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"] 

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> result = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                result.add("FizzBuzz");
            } else if (i % 3 == 0) {
                result.add("Fizz");
            } else if (i % 5 == 0) {
                result.add("Buzz");
            } else {
                result.add(Integer.toString(i));
            }
        }
        return result;
    }
}
```