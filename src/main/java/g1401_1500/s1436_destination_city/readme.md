[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1436\. Destination City

Easy

You are given the array `paths`, where <code>paths[i] = [cityA<sub>i</sub>, cityB<sub>i</sub>]</code> means there exists a direct path going from <code>cityA<sub>i</sub></code> to <code>cityB<sub>i</sub></code>. _Return the destination city, that is, the city without any path outgoing to another city._

It is guaranteed that the graph of paths forms a line without any loop, therefore, there will be exactly one destination city.

**Example 1:**

**Input:** paths = \[\["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]

**Output:** "Sao Paulo"

**Explanation:** Starting at "London" city you will reach "Sao Paulo" city which is the destination city. Your trip consist of: "London" -> "New York" -> "Lima" -> "Sao Paulo".

**Example 2:**

**Input:** paths = \[\["B","C"],["D","B"],["C","A"]]

**Output:** "A"

**Explanation:** All possible trips are: 

"D" -> "B" -> "C" -> "A". 

"B" -> "C" -> "A". 

"C" -> "A". 

"A". Clearly the destination city is "A".

**Example 3:**

**Input:** paths = \[\["A","Z"]]

**Output:** "Z"

**Constraints:**

*   `1 <= paths.length <= 100`
*   `paths[i].length == 2`
*   <code>1 <= cityA<sub>i</sub>.length, cityB<sub>i</sub>.length <= 10</code>
*   <code>cityA<sub>i</sub> != cityB<sub>i</sub></code>
*   All strings consist of lowercase and uppercase English letters and the space character.

## Solution

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public String destCity(List<List<String>> paths) {
        Set<String> set = new HashSet<>();
        for (List<String> strings : paths) {
            set.add(strings.get(0));
        }
        for (List<String> path : paths) {
            if (!set.contains(path.get(1))) {
                return path.get(1);
            }
        }
        return "";
    }
}
```