[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3272\. Find the Count of Good Integers

Hard

You are given two **positive** integers `n` and `k`.

An integer `x` is called **k-palindromic** if:

*   `x` is a palindrome.
*   `x` is divisible by `k`.

An integer is called **good** if its digits can be _rearranged_ to form a **k-palindromic** integer. For example, for `k = 2`, 2020 can be rearranged to form the _k-palindromic_ integer 2002, whereas 1010 cannot be rearranged to form a _k-palindromic_ integer.

Return the count of **good** integers containing `n` digits.

**Note** that _any_ integer must **not** have leading zeros, **neither** before **nor** after rearrangement. For example, 1010 _cannot_ be rearranged to form 101.

**Example 1:**

**Input:** n = 3, k = 5

**Output:** 27

**Explanation:**

_Some_ of the good integers are:

*   551 because it can be rearranged to form 515.
*   525 because it is already k-palindromic.

**Example 2:**

**Input:** n = 1, k = 4

**Output:** 2

**Explanation:**

The two good integers are 4 and 8.

**Example 3:**

**Input:** n = 5, k = 6

**Output:** 2468

**Constraints:**

*   `1 <= n <= 10`
*   `1 <= k <= 9`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class Solution {
    private final List<String> palindromes = new ArrayList<>();

    private long factorial(int n) {
        long res = 1;
        for (int i = 2; i <= n; i++) {
            res *= i;
        }
        return res;
    }

    private Map<Character, Integer> countDigits(String s) {
        Map<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray()) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }
        return freq;
    }

    private long calculatePermutations(Map<Character, Integer> freq, int length) {
        long totalPermutations = factorial(length);
        for (int count : freq.values()) {
            totalPermutations /= factorial(count);
        }
        return totalPermutations;
    }

    private long calculateValidPermutations(String s) {
        Map<Character, Integer> freq = countDigits(s);
        int n = s.length();
        long totalPermutations = calculatePermutations(freq, n);
        if (freq.getOrDefault('0', 0) > 0) {
            freq.put('0', freq.get('0') - 1);
            long invalidPermutations = calculatePermutations(freq, n - 1);
            totalPermutations -= invalidPermutations;
        }
        return totalPermutations;
    }

    private void generatePalindromes(
            int f, int r, int k, int lb, int sum, StringBuilder ans, int[] rem) {
        if (f > r) {
            if (sum == 0) {
                palindromes.add(ans.toString());
            }
            return;
        }
        for (int i = lb; i <= 9; i++) {
            ans.setCharAt(f, (char) ('0' + i));
            ans.setCharAt(r, (char) ('0' + i));
            int chk = sum;
            chk = (chk + rem[f] * i) % k;
            if (f != r) {
                chk = (chk + rem[r] * i) % k;
            }
            generatePalindromes(f + 1, r - 1, k, 0, chk, ans, rem);
        }
    }

    private List<String> allKPalindromes(int n, int k) {
        StringBuilder ans = new StringBuilder(n);
        ans.append("0".repeat(Math.max(0, n)));
        int[] rem = new int[n];
        rem[0] = 1;
        for (int i = 1; i < n; i++) {
            rem[i] = (rem[i - 1] * 10) % k;
        }
        palindromes.clear();
        generatePalindromes(0, n - 1, k, 1, 0, ans, rem);
        return palindromes;
    }

    public long countGoodIntegers(int n, int k) {
        List<String> ans = allKPalindromes(n, k);
        Set<String> st = new HashSet<>();
        for (String str : ans) {
            char[] arr = str.toCharArray();
            Arrays.sort(arr);
            st.add(new String(arr));
        }
        List<String> v = new ArrayList<>(st);
        long chk = 0;
        for (String str : v) {
            long cc = calculateValidPermutations(str);
            chk += cc;
        }
        return chk;
    }
}
```