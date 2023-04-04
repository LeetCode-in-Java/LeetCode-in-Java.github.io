[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1629\. Slowest Key

Easy

A newly designed keypad was tested, where a tester pressed a sequence of `n` keys, one at a time.

You are given a string `keysPressed` of length `n`, where `keysPressed[i]` was the <code>i<sup>th</sup></code> key pressed in the testing sequence, and a sorted list `releaseTimes`, where `releaseTimes[i]` was the time the <code>i<sup>th</sup></code> key was released. Both arrays are **0-indexed**. The <code>0<sup>th</sup></code> key was pressed at the time `0`, and every subsequent key was pressed at the **exact** time the previous key was released.

The tester wants to know the key of the keypress that had the **longest duration**. The <code>i<sup>th</sup></code> keypress had a **duration** of `releaseTimes[i] - releaseTimes[i - 1]`, and the <code>0<sup>th</sup></code> keypress had a duration of `releaseTimes[0]`.

Note that the same key could have been pressed multiple times during the test, and these multiple presses of the same key **may not** have had the same **duration**.

_Return the key of the keypress that had the **longest duration**. If there are multiple such keypresses, return the lexicographically largest key of the keypresses._

**Example 1:**

**Input:** releaseTimes = [9,29,49,50], keysPressed = "cbcd"

**Output:** "c"

**Explanation:** The keypresses were as follows: 

Keypress for 'c' had a duration of 9 (pressed at time 0 and released at time 9). 

Keypress for 'b' had a duration of 29 - 9 = 20 (pressed at time 9 right after the release of the previous character and released at time 29). 

Keypress for 'c' had a duration of 49 - 29 = 20 (pressed at time 29 right after the release of the previous character and released at time 49). 

Keypress for 'd' had a duration of 50 - 49 = 1 (pressed at time 49 right after the release of the previous character and released at time 50). 

The longest of these was the keypress for 'b' and the second keypress for 'c', both with duration 20. 'c' is lexicographically larger than 'b', so the answer is 'c'.

**Example 2:**

**Input:** releaseTimes = [12,23,36,46,62], keysPressed = "spuda"

**Output:** "a"

**Explanation:** 

The keypresses were as follows: Keypress for 's' had a duration of 12. 

Keypress for 'p' had a duration of 23 - 12 = 11.

Keypress for 'u' had a duration of 36 - 23 = 13. 

Keypress for 'd' had a duration of 46 - 36 = 10.

Keypress for 'a' had a duration of 62 - 46 = 16. 

The longest of these was the keypress for 'a' with duration 16.

**Constraints:**

*   `releaseTimes.length == n`
*   `keysPressed.length == n`
*   `2 <= n <= 1000`
*   <code>1 <= releaseTimes[i] <= 10<sup>9</sup></code>
*   `releaseTimes[i] < releaseTimes[i+1]`
*   `keysPressed` contains only lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < releaseTimes.length; i++) {
            char c = keysPressed.charAt(i);
            int duration;
            if (i == 0) {
                duration = releaseTimes[i];
            } else {
                duration = releaseTimes[i] - releaseTimes[i - 1];
            }
            if (!map.containsKey(c)) {
                map.put(c, duration);
            } else {
                int val = map.get(c);
                if (duration > val) {
                    map.put(c, duration);
                }
            }
        }
        Map<Integer, List<Character>> map2 = new HashMap<>();
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            int duration = entry.getValue();
            if (!map2.containsKey(duration)) {
                map2.put(duration, new ArrayList<>());
            }
            map2.get(duration).add(entry.getKey());
        }
        int max = -1;
        for (Map.Entry<Integer, List<Character>> entry : map2.entrySet()) {
            List<Character> chars = entry.getValue();
            Collections.sort(chars);
            map2.put(entry.getKey(), chars);
            max = Math.max(max, entry.getKey());
        }
        return map2.get(max).get(map2.get(max).size() - 1);
    }
}
```