[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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

    private static class TreeNode {
        private int val;
        private TreeNode left;
        private TreeNode right;

        private TreeNode(int data) {
            val = data;
            left = null;
            right = null;
        }
    }

    private Map<String, TreeNode> map = new HashMap<>();

    private TreeNode insert(TreeNode root, int val) {
        if (root == null) {
            root = new TreeNode(val);
        } else if (root.val <= val) {
            root.right = insert(root.right, val);
        } else {
            root.left = insert(root.left, val);
        }
        return root;
    }

    public void recordTweet(String name, int time) {
        TreeNode root = map.get(name);
        root = insert(root, time);
        map.put(name, root);
    }

    private int treverse(TreeNode root, int l, int r) {
        if (root == null || l >= r) {
            return 0;
        }
        if (root.val <= l) {
            int add = root.val == l ? 1 : 0;
            return add + treverse(root.right, l, r);
        }
        if (root.val >= r) {
            return treverse(root.left, l, r);
        }
        return 1 + treverse(root.left, l, r) + treverse(root.right, l, r);
    }

    public List<Integer> getTweetCountsPerFrequency(String freq, String name, int start, int end) {
        int d = 0;
        TreeNode root = map.get(name);
        List<Integer> res = new ArrayList<>();
        if (freq.equals("minute")) {
            d = 60;
        } else if (freq.equals("hour")) {
            d = 3600;
        } else {
            d = 86400;
        }
        while (start + d <= end) {
            int count = treverse(root, start, start + d);
            start = start + d;
            res.add(count);
        }
        if (start <= end) {
            int count = treverse(root, start, end + 1);
            res.add(count);
        }
        return res;
    }
}

/*
 * Your TweetCounts object will be instantiated and called as such:
 * TweetCounts obj = new TweetCounts();
 * obj.recordTweet(tweetName,time);
 * List<Integer> param_2 = obj.getTweetCountsPerFrequency(freq,tweetName,startTime,endTime);
 */
```