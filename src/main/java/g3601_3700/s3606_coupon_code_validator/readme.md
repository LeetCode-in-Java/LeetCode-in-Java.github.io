[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3606\. Coupon Code Validator

Easy

You are given three arrays of length `n` that describe the properties of `n` coupons: `code`, `businessLine`, and `isActive`. The <code>i<sup>th</sup></code> coupon has:

*   `code[i]`: a **string** representing the coupon identifier.
*   `businessLine[i]`: a **string** denoting the business category of the coupon.
*   `isActive[i]`: a **boolean** indicating whether the coupon is currently active.

A coupon is considered **valid** if all of the following conditions hold:

1.  `code[i]` is non-empty and consists only of alphanumeric characters (a-z, A-Z, 0-9) and underscores (`_`).
2.  `businessLine[i]` is one of the following four categories: `"electronics"`, `"grocery"`, `"pharmacy"`, `"restaurant"`.
3.  `isActive[i]` is **true**.

Return an array of the **codes** of all valid coupons, **sorted** first by their **businessLine** in the order: `"electronics"`, `"grocery"`, `"pharmacy", "restaurant"`, and then by **code** in lexicographical (ascending) order within each category.

**Example 1:**

**Input:** code = ["SAVE20","","PHARMA5","SAVE@20"], businessLine = ["restaurant","grocery","pharmacy","restaurant"], isActive = [true,true,true,true]

**Output:** ["PHARMA5","SAVE20"]

**Explanation:**

*   First coupon is valid.
*   Second coupon has empty code (invalid).
*   Third coupon is valid.
*   Fourth coupon has special character `@` (invalid).

**Example 2:**

**Input:** code = ["GROCERY15","ELECTRONICS\_50","DISCOUNT10"], businessLine = ["grocery","electronics","invalid"], isActive = [false,true,true]

**Output:** ["ELECTRONICS\_50"]

**Explanation:**

*   First coupon is inactive (invalid).
*   Second coupon is valid.
*   Third coupon has invalid business line (invalid).

**Constraints:**

*   `n == code.length == businessLine.length == isActive.length`
*   `1 <= n <= 100`
*   `0 <= code[i].length, businessLine[i].length <= 100`
*   `code[i]` and `businessLine[i]` consist of printable ASCII characters.
*   `isActive[i]` is either `true` or `false`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

@SuppressWarnings("java:S135")
public class Solution {
    public List<String> validateCoupons(String[] code, String[] businessLine, boolean[] isActive) {
        List<String> lt = new ArrayList<>();
        List<List<String>> lst = new ArrayList<>();
        HashSet<String> hs = new HashSet<>();
        hs.add("electronics");
        hs.add("grocery");
        hs.add("pharmacy");
        hs.add("restaurant");
        int n = code.length;
        for (int i = 0; i < n; i++) {
            if (!isActive[i]) {
                continue;
            }
            String s = businessLine[i];
            if (!hs.contains(s)) {
                continue;
            }
            String st = code[i];
            if (st.isEmpty()) {
                continue;
            }
            int a = 0;
            for (int j = 0; j < st.length(); j++) {
                char ch = st.charAt(j);
                if (!((ch == '_')
                        || (ch >= 'a' && ch <= 'z')
                        || (ch >= 'A' && ch <= 'Z')
                        || (ch >= '0' && ch <= '9'))) {
                    a = 1;
                    break;
                }
            }
            if (a == 0) {
                List<String> lst2 = new ArrayList<>();
                lst2.add(code[i]);
                lst2.add(businessLine[i]);
                lst.add(lst2);
            }
        }
        lst.sort(
                (l, m) -> {
                    if (!(l.get(1).equals(m.get(1)))) {
                        return l.get(1).compareTo(m.get(1));
                    } else {
                        return l.get(0).compareTo(m.get(0));
                    }
                });
        for (List<String> strings : lst) {
            lt.add(strings.get(0));
        }
        return lt;
    }
}
```