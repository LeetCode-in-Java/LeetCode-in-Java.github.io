## 599\. Minimum Index Sum of Two Lists

Easy

Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their **common interest** with the **least list index sum**. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

**Example 1:**

**Input:** list1 = ["Shogun","Tapioca Express","Burger King","KFC"], list2 = ["Piatti","The Grill at Torrey Pines","Hungry Hunter Steakhouse","Shogun"]

**Output:** ["Shogun"]

**Explanation:** The only restaurant they both like is "Shogun". 

**Example 2:**

**Input:** list1 = ["Shogun","Tapioca Express","Burger King","KFC"], list2 = ["KFC","Shogun","Burger King"]

**Output:** ["Shogun"]

**Explanation:** The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1). 

**Constraints:**

*   `1 <= list1.length, list2.length <= 1000`
*   `1 <= list1[i].length, list2[i].length <= 30`
*   `list1[i]` and `list2[i]` consist of spaces `' '` and English letters.
*   All the stings of `list1` are **unique**.
*   All the stings of `list2` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        int min = 1000000;
        Map<String, Integer> hm = new HashMap<>();
        List<String> result = new ArrayList<>();
        fillMap(list1, hm);
        // find min value
        for (int i = 0; i < list2.length; i++) {
            if (hm.containsKey(list2[i])) {
                int value = hm.get(list2[i]) + i;
                // a new min value was found
                if (value < min) {
                    min = value;
                    // Clean the arraylist
                    result.clear();
                    // add new min value
                    result.add(list2[i]);
                } else if (value == min) {
                    result.add(list2[i]);
                }
            }
        }
        return result.toArray(new String[result.size()]);
    }

    public void fillMap(String[] a, Map<String, Integer> hm) {
        for (int i = 0; i < a.length; i++) {
            hm.put(a[i], i);
        }
    }
}
```