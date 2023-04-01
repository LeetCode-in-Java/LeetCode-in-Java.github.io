[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 535\. Encode and Decode TinyURL

Medium

> Note: This is a companion problem to the [System Design](https://leetcode.com/discuss/interview-question/system-design/) problem: [Design TinyURL](https://leetcode.com/discuss/interview-question/124658/Design-a-URL-Shortener-(-TinyURL-)-System/).

TinyURL is a URL shortening service where you enter a URL such as `https://leetcode.com/problems/design-tinyurl` and it returns a short URL such as `http://tinyurl.com/4e9iAk`. Design a class to encode a URL and decode a tiny URL.

There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

Implement the `Solution` class:

*   `Solution()` Initializes the object of the system.
*   `String encode(String longUrl)` Returns a tiny URL for the given `longUrl`.
*   `String decode(String shortUrl)` Returns the original long URL for the given `shortUrl`. It is guaranteed that the given `shortUrl` was encoded by the same object.

**Example 1:**

**Input:** url = "https://leetcode.com/problems/design-tinyurl"

**Output:** "https://leetcode.com/problems/design-tinyurl"

**Explanation:**

    Solution obj = new Solution();
    string tiny = obj.encode(url); // returns the encoded tiny url.
    string ans = obj.decode(tiny); // returns the original url after deconding it. 

**Constraints:**

*   <code>1 <= url.length <= 10<sup>4</sup></code>
*   `url` is guranteed to be a valid URL.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Codec {
    private Map<String, String> map = new HashMap<>();
    private int n = 0;

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        n++;
        String ans = "http://tinyurl.com/";
        ans += Integer.toString(n);
        map.put(ans, longUrl);
        return ans;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return map.get(shortUrl);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```