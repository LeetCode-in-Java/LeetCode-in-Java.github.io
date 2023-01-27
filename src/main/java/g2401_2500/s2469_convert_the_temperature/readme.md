[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2469\. Convert the Temperature

Easy

You are given a non-negative floating point number rounded to two decimal places `celsius`, that denotes the **temperature in Celsius**.

You should convert Celsius into **Kelvin** and **Fahrenheit** and return it as an array `ans = [kelvin, fahrenheit]`.

Return _the array `ans`._ Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Note that:**

*   `Kelvin = Celsius + 273.15`
*   `Fahrenheit = Celsius * 1.80 + 32.00`

**Example 1:**

**Input:** celsius = 36.50

**Output:** [309.65000,97.70000]

**Explanation:** Temperature at 36.50 Celsius converted in Kelvin is 309.65 and converted in Fahrenheit is 97.70. 

**Example 2:**

**Input:** celsius = 122.11

**Output:** [395.26000,251.79800]

**Explanation:** Temperature at 122.11 Celsius converted in Kelvin is 395.26 and converted in Fahrenheit is 251.798. 

**Constraints:**

*   `0 <= celsius <= 1000`

## Solution

```java
public class Solution {
    public double[] convertTemperature(double celsius) {
        double kelvin = celsius + 273.15;
        double fahrenheit = celsius * 1.80 + 32.00;
        double[] arr = new double[2];
        arr[0] = kelvin;
        arr[1] = fahrenheit;
        return arr;
    }
}
```