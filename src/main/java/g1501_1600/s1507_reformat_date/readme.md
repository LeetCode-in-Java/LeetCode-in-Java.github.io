## 1507\. Reformat Date

Easy

Given a `date` string in the form `Day Month Year`, where:

*   `Day` is in the set `{"1st", "2nd", "3rd", "4th", ..., "30th", "31st"}`.
*   `Month` is in the set `{"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"}`.
*   `Year` is in the range `[1900, 2100]`.

Convert the date string to the format `YYYY-MM-DD`, where:

*   `YYYY` denotes the 4 digit year.
*   `MM` denotes the 2 digit month.
*   `DD` denotes the 2 digit day.

**Example 1:**

**Input:** date = "20th Oct 2052"

**Output:** "2052-10-20"

**Example 2:**

**Input:** date = "6th Jun 1933"

**Output:** "1933-06-06"

**Example 3:**

**Input:** date = "26th May 1960"

**Output:** "1960-05-26"

**Constraints:**

*   The given dates are guaranteed to be valid, so no error handling is necessary.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String reformatDate(String date) {
        StringBuilder sb = new StringBuilder();
        Map<String, String> map = new HashMap<>();
        map.put("Jan", "01");
        map.put("Feb", "02");
        map.put("Mar", "03");
        map.put("Apr", "04");
        map.put("May", "05");
        map.put("Jun", "06");
        map.put("Jul", "07");
        map.put("Aug", "08");
        map.put("Sep", "09");
        map.put("Oct", "10");
        map.put("Nov", "11");
        map.put("Dec", "12");
        sb.append(date.substring(date.length() - 4));
        sb.append('-');
        sb.append(map.get(date.substring(date.length() - 8, date.length() - 5)));
        sb.append('-');
        if (Character.isDigit(date.charAt(1))) {
            sb.append(date, 0, 2);
        } else {
            sb.append('0');
            sb.append(date.charAt(0));
        }
        return sb.toString();
    }
}
```