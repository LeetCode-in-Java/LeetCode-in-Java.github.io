[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 920\. Number of Music Playlists

Hard

Your music player contains `n` different songs. You want to listen to `goal` songs (not necessarily different) during your trip. To avoid boredom, you will create a playlist so that:

*   Every song is played **at least once**.
*   A song can only be played again only if `k` other songs have been played.

Given `n`, `goal`, and `k`, return _the number of possible playlists that you can create_. Since the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, goal = 3, k = 1

**Output:** 6

**Explanation:** There are 6 possible playlists: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], and [3, 2, 1].

**Example 2:**

**Input:** n = 2, goal = 3, k = 0

**Output:** 6

**Explanation:** There are 6 possible playlists: [1, 1, 2], [1, 2, 1], [2, 1, 1], [2, 2, 1], [2, 1, 2], and [1, 2, 2].

**Example 3:**

**Input:** n = 2, goal = 3, k = 1

**Output:** 2

**Explanation:** There are 2 possible playlists: [1, 2, 1] and [2, 1, 2].

**Constraints:**

*   `0 <= k < n <= goal <= 100`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int numMusicPlaylists(int n, int l, int k) {
        long[][] dp = new long[l][n + 1];
        for (int i = 0; i < l; i++) {
            Arrays.fill(dp[i], -1);
        }
        return (int) helper(0, l, 0, n, k, dp);
    }

    private long helper(int songNumber, int l, int usedSong, int n, int k, long[][] dp) {
        if (songNumber == l) {
            return usedSong == n ? 1 : 0;
        }
        if (dp[songNumber][usedSong] != -1) {
            return dp[songNumber][usedSong];
        }
        long ans;
        if (songNumber < k) {
            ans = (n - usedSong) * helper(songNumber + 1, l, usedSong + 1, n, k, dp);
        } else if (usedSong == n) {
            ans = (usedSong - k) * helper(songNumber + 1, l, usedSong, n, k, dp);
        } else {
            ans =
                    (n - usedSong) * helper(songNumber + 1, l, usedSong + 1, n, k, dp)
                            + (usedSong - k) * helper(songNumber + 1, l, usedSong, n, k, dp);
        }
        int mod = (int) 1e9 + 7;
        dp[songNumber][usedSong] = ans % mod;
        return ans % mod;
    }
}
```