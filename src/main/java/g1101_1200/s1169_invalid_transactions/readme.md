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
import java.util.TreeMap;

public class Solution {
    public List<String> invalidTransactions(String[] transactions) {
        Map<String, TreeMap<Integer, Transaction>> map = new HashMap<>();
        List<String> result = new ArrayList<>();
        for (String transaction : transactions) {
            String[] split = transaction.split(",");
            String name = split[0];
            int time = Integer.parseInt(split[1]);
            String city = split[3];
            map.putIfAbsent(name, new TreeMap<>());
            map.get(name).put(time, new Transaction(time, city));
        }
        for (String transaction : transactions) {
            String[] split = transaction.split(",");
            String name = split[0];
            int time = Integer.parseInt(split[1]);
            int amount = Integer.parseInt(split[2]);
            String city = split[3];
            if (amount > 1000) {
                result.add(transaction);
                continue;
            }
            for (Map.Entry<Integer, Transaction> entry :
                    map.get(name).subMap(time - 60, time + 60).entrySet()) {
                if (Math.abs(time - entry.getKey()) <= 60 && !entry.getValue().city.equals(city)) {
                    result.add(transaction);
                    break;
                }
            }
        }
        return result;
    }

    private static class Transaction {
        int amount;
        String city;

        public Transaction(int amount, String city) {
            this.amount = amount;
            this.city = city;
        }
    }
}
```