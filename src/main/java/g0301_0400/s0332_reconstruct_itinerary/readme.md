## 332\. Reconstruct Itinerary

Hard

You are given a list of airline `tickets` where <code>tickets[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

*   For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

**Input:** tickets = \[\["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]

**Output:** ["JFK","MUC","LHR","SFO","SJC"] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

**Input:** tickets = \[\["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]

**Output:** ["JFK","ATL","JFK","SFO","ATL","SFO"]

**Explanation:**

    Another possible reconstruction is
    ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order. 

**Constraints:**

*   `1 <= tickets.length <= 300`
*   `tickets[i].length == 2`
*   <code>from<sub>i</sub>.length == 3</code>
*   <code>to<sub>i</sub>.length == 3</code>
*   <code>from<sub>i</sub></code> and <code>to<sub>i</sub></code> consist of uppercase English letters.
*   <code>from<sub>i</sub> != to<sub>i</sub></code>

## Solution

```java
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        HashMap<String, PriorityQueue<String>> map = new HashMap<>();
        LinkedList<String> ans = new LinkedList<>();
        for (List<String> ticket : tickets) {
            String src = ticket.get(0);
            String dest = ticket.get(1);
            PriorityQueue<String> pq = map.getOrDefault(src, new PriorityQueue<>());
            pq.add(dest);
            map.put(src, pq);
        }
        dfs(map, "JFK", ans);
        return ans;
    }

    private void dfs(Map<String, PriorityQueue<String>> map, String src, LinkedList<String> ans) {
        PriorityQueue<String> temp = map.get(src);
        while (temp != null && !temp.isEmpty()) {
            String nbr = temp.remove();
            dfs(map, nbr, ans);
        }
        ans.addFirst(src);
    }
}
```