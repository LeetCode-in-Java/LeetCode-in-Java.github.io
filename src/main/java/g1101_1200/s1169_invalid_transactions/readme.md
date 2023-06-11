[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1169\. Invalid Transactions

Medium

A transaction is possibly invalid if:

*   the amount exceeds `$1000`, or;
*   if it occurs within (and including) `60` minutes of another transaction with the **same name** in a **different city**.

You are given an array of strings `transaction` where `transactions[i]` consists of comma-separated values representing the name, time (in minutes), amount, and city of the transaction.

Return a list of `transactions` that are possibly invalid. You may return the answer in **any order**.

**Example 1:**

**Input:** transactions = ["alice,20,800,mtv","alice,50,100,beijing"]

**Output:** ["alice,20,800,mtv","alice,50,100,beijing"]

**Explanation:** The first transaction is invalid because the second transaction occurs within a difference of 60 minutes, have the same name and is in a different city. Similarly the second one is invalid too.

**Example 2:**

**Input:** transactions = ["alice,20,800,mtv","alice,50,1200,mtv"]

**Output:** ["alice,50,1200,mtv"]

**Example 3:**

**Input:** transactions = ["alice,20,800,mtv","bob,50,1200,mtv"]

**Output:** ["bob,50,1200,mtv"]

**Constraints:**

*   `transactions.length <= 1000`
*   Each `transactions[i]` takes the form `"{name},{time},{amount},{city}"`
*   Each `{name}` and `{city}` consist of lowercase English letters, and have lengths between `1` and `10`.
*   Each `{time}` consist of digits, and represent an integer between `0` and `1000`.
*   Each `{amount}` consist of digits, and represent an integer between `0` and `2000`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    private static class Transaction {
        String name;
        int time;
        int amount;
        String city;

        Transaction(String trans) {
            String[] s = trans.split(",");
            name = s[0];
            time = Integer.parseInt(s[1]);
            amount = Integer.parseInt(s[2]);
            city = s[3];
        }
    }

    public List<String> invalidTransactions(String[] input) {
        List<String> res = new ArrayList<>();
        if (input == null || input.length == 0) {
            return res;
        }
        Map<String, List<Transaction>> map = new HashMap<>();
        for (String s : input) {
            Transaction trans = new Transaction(s);
            if (!map.containsKey(trans.name)) {
                map.put(trans.name, new ArrayList<>());
            }
            map.get(trans.name).add(trans);
        }
        for (String s : input) {
            Transaction trans = new Transaction(s);
            if (!isValid(trans, map)) {
                res.add(s);
            }
        }
        return res;
    }

    private boolean isValid(Transaction transaction, Map<String, List<Transaction>> map) {
        if (transaction.amount > 1000) {
            return false;
        }
        for (Transaction s : map.get(transaction.name)) {
            if (Math.abs(s.time - transaction.time) <= 60 && !s.city.equals(transaction.city)) {
                return false;
            }
        }
        return true;
    }
}
```