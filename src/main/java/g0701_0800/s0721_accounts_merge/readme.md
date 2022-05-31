## 721\. Accounts Merge

Medium

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example 1:**

**Input:** accounts = 
    
    [
        ["John","johnsmith@mail.com","john\_newyork@mail.com"],
        ["John","johnsmith@mail.com","john00@mail.com"],
        ["Mary","mary@mail.com"],
        ["John","johnnybravo@mail.com"]
    ]

**Output:** 

    [
        ["John","john00@mail.com","john\_newyork@mail.com","johnsmith@mail.com"],
        ["Mary","mary@mail.com"],
        ["John","johnnybravo@mail.com"]
    ]

**Explanation:** The first and second John's are the same person as they have the common email "johnsmith@mail.com". The third John and Mary are different people as none of their email addresses are used by other accounts. We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], ['John', 'john00@mail.com', 'john\_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

**Example 2:**

**Input:** accounts = 

    [
        ["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],
        ["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],
        ["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],
        ["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],
        ["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]
    ]

**Output:** 

    [
        ["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],
        ["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],
        ["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],
        ["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],
        ["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]
    ]

**Constraints:**

*   `1 <= accounts.length <= 1000`
*   `2 <= accounts[i].length <= 10`
*   `1 <= accounts[i][j] <= 30`
*   `accounts[i][0]` consists of English letters.
*   `accounts[i][j] (for j > 0)` is a valid email.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.Stack;

@SuppressWarnings("java:S1149")
public class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap<>();
        Map<String, ArrayList<String>> graph = new HashMap<>();
        for (List<String> account : accounts) {
            String name = "";
            for (String email : account) {
                if (name.equals("")) {
                    name = email;
                    continue;
                }
                graph.computeIfAbsent(email, x -> new ArrayList<>()).add(account.get(1));
                graph.computeIfAbsent(account.get(1), x -> new ArrayList<>()).add(email);
                emailToName.put(email, name);
            }
        }

        Set<String> seen = new HashSet<>();
        List<List<String>> ans = new ArrayList<>();
        for (String email : graph.keySet()) {
            if (!seen.contains(email)) {
                seen.add(email);
                Stack<String> stack = new Stack<>();
                stack.push(email);
                List<String> component = new ArrayList<>();
                while (!stack.empty()) {
                    String node = stack.pop();
                    component.add(node);
                    for (String nei : graph.get(node)) {
                        if (!seen.contains(nei)) {
                            seen.add(nei);
                            stack.push(nei);
                        }
                    }
                }
                Collections.sort(component);
                component.add(0, emailToName.get(email));
                ans.add(component);
            }
        }
        return ans;
    }
}
```