## 1734\. Decode XORed Permutation

Medium

There is an integer array `perm` that is a permutation of the first `n` positive integers, where `n` is always **odd**.

It was encoded into another integer array `encoded` of length `n - 1`, such that `encoded[i] = perm[i] XOR perm[i + 1]`. For example, if `perm = [1,3,2]`, then `encoded = [2,1]`.

Given the `encoded` array, return _the original array_ `perm`. It is guaranteed that the answer exists and is unique.

**Example 1:**

**Input:** encoded = [3,1]

**Output:** [1,2,3]

**Explanation:** If perm = [1,2,3], then encoded = [1 XOR 2,2 XOR 3] = [3,1]

**Example 2:**

**Input:** encoded = [6,5,4,6]

**Output:** [2,4,1,5,3]

**Constraints:**

*   <code>3 <= n < 10<sup>5</sup></code>
*   `n` is odd.
*   `encoded.length == n - 1`

## Solution

```java
public class Solution {
    public int[] decode(int[] encoded) {
        int[] decoded = new int[encoded.length + 1];
        for (int i = 1; i < encoded.length; i = i + 2) {
            decoded[0] = decoded[0] ^ encoded[i];
            decoded[0] = decoded[0] ^ i;
            decoded[0] = decoded[0] ^ i + 1;
        }
        decoded[0] = decoded[0] ^ decoded.length;
        for (int i = 1; i < decoded.length; i++) {
            decoded[i] = decoded[i - 1] ^ encoded[i - 1];
        }
        return decoded;
    }
}
```