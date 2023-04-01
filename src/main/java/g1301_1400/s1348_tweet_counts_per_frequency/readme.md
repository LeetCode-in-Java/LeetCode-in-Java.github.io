[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1348\. Tweet Counts Per Frequency

Medium

A social media company is trying to monitor activity on their site by analyzing the number of tweets that occur in select periods of time. These periods can be partitioned into smaller **time chunks** based on a certain frequency (every **minute**, **hour**, or **day**).

For example, the period `[10, 10000]` (in **seconds**) would be partitioned into the following **time chunks** with these frequencies:

*   Every **minute** (60-second chunks): `[10,69]`, `[70,129]`, `[130,189]`, `...`, `[9970,10000]`
*   Every **hour** (3600-second chunks): `[10,3609]`, `[3610,7209]`, `[7210,10000]`
*   Every **day** (86400-second chunks): `[10,10000]`

Notice that the last chunk may be shorter than the specified frequency's chunk size and will always end with the end time of the period (`10000` in the above example).

Design and implement an API to help the company with their analysis.

Implement the `TweetCounts` class:

*   `TweetCounts()` Initializes the `TweetCounts` object.
*   `void recordTweet(String tweetName, int time)` Stores the `tweetName` at the recorded `time` (in **seconds**).
*   `List<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime)` Returns a list of integers representing the number of tweets with `tweetName` in each **time chunk** for the given period of time `[startTime, endTime]` (in **seconds**) and frequency `freq`.
    *   `freq` is one of `"minute"`, `"hour"`, or `"day"` representing a frequency of every **minute**, **hour**, or **day** respectively.

**Example:**

**Input** ["TweetCounts","recordTweet","recordTweet","recordTweet","getTweetCountsPerFrequency","getTweetCountsPerFrequency","recordTweet","getTweetCountsPerFrequency"]

[[],["tweet3",0],["tweet3",60],["tweet3",10],["minute","tweet3",0,59],["minute","tweet3",0,60],["tweet3",120],["hour","tweet3",0,210]]

**Output:** [null,null,null,null,[2],[2,1],null,[4]]

**Explanation:** 

TweetCounts tweetCounts = new TweetCounts(); 

tweetCounts.recordTweet("tweet3", 0); // New tweet "tweet3" at time 0 

tweetCounts.recordTweet("tweet3", 60); // New tweet "tweet3" at time 60 

tweetCounts.recordTweet("tweet3", 10); // New tweet "tweet3" at time 10 

tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 59); // return [2]; chunk [0,59] had 2 tweets

tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 60); // return [2,1]; chunk [0,59] had 2 tweets, chunk [60,60] had 1 tweet 

tweetCounts.recordTweet("tweet3", 120); // New tweet "tweet3" at time 120 

tweetCounts.getTweetCountsPerFrequency("hour", "tweet3", 0, 210); // return [4]; chunk [0,210] had 4 tweets

**Constraints:**

*   <code>0 <= time, startTime, endTime <= 10<sup>9</sup></code>
*   <code>0 <= endTime - startTime <= 10<sup>4</sup></code>
*   There will be at most <code>10<sup>4</sup></code> calls **in total** to `recordTweet` and `getTweetCountsPerFrequency`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class TweetCounts {
    private static final int MINUTE = 60;
    private static final int HOUR = 3600;
    private static final int DAY = 86400;

    private final Map<String, Map<Integer, Map<Integer, Map<Integer, List<Integer>>>>> store;

    public TweetCounts() {
        this.store = new HashMap<>();
    }

    public void recordTweet(String tweetName, int time) {
        int d = time / DAY;
        int h = (time - d * DAY) / HOUR;
        int m = (time - d * DAY - h * HOUR) / MINUTE;
        Map<Integer, Map<Integer, Map<Integer, List<Integer>>>> dstore =
                store.computeIfAbsent(tweetName, k -> new HashMap<>());
        Map<Integer, Map<Integer, List<Integer>>> hstore =
                dstore.computeIfAbsent(d, k -> new HashMap<>());
        Map<Integer, List<Integer>> mstore = hstore.computeIfAbsent(h, k -> new HashMap<>());
        mstore.computeIfAbsent(m, k -> new ArrayList<>()).add(time);
    }

    public List<Integer> getTweetCountsPerFrequency(
            String freq, String tweetName, int startTime, int endTime) {
        int sfreq = convFreqToSecond(freq);
        Map<Integer, Map<Integer, Map<Integer, List<Integer>>>> dstore = store.get(tweetName);
        int[] chunks = new int[(endTime - startTime) / sfreq + 1];
        int sd = startTime / DAY;
        int ed = endTime / DAY;
        for (int d = sd; d <= ed; d++) {
            if (!dstore.containsKey(d)) {
                continue;
            }
            int sh = startTime <= (d * DAY) ? 0 : ((startTime - d * DAY) / HOUR);
            int eh = endTime > ((d + 1) * DAY) ? (DAY / HOUR) : ((endTime - d * DAY) / HOUR + 1);
            Map<Integer, Map<Integer, List<Integer>>> hstore = dstore.get(d);
            for (int h = sh; h < eh; h++) {
                if (!hstore.containsKey(h)) {
                    continue;
                }
                int sm =
                        startTime <= (d * DAY + h * HOUR)
                                ? 0
                                : ((startTime - d * DAY - h * HOUR) / MINUTE);
                int em =
                        endTime > (d * DAY + (h + 1) * HOUR)
                                ? (HOUR / MINUTE)
                                : ((endTime - d * DAY - h * HOUR) / MINUTE + 1);
                Map<Integer, List<Integer>> mstore = hstore.get(h);
                for (int m = sm; m <= em; m++) {
                    if (!mstore.containsKey(m)) {
                        continue;
                    }
                    for (Integer rc : mstore.get(m)) {
                        if (startTime <= rc && rc <= endTime) {
                            chunks[(rc - startTime) / sfreq]++;
                        }
                    }
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int chunk : chunks) {
            ans.add(chunk);
        }
        return ans;
    }

    private int convFreqToSecond(String freq) {
        switch (freq) {
            case "minute":
                return MINUTE;
            case "hour":
                return HOUR;
            case "day":
                return DAY;
            default:
                return 0;
        }
    }
}
```