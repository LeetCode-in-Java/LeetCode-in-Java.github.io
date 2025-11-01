[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 71\. Simplify Path

Medium

You are given an _absolute_ path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.

The _rules_ of a Unix-style file system are as follows:

*   A single period `'.'` represents the current directory.
*   A double period `'..'` represents the previous/parent directory.
*   Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
*   Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or** **file** **name**. For example, `'...'` and `'....'` are valid directory or file names.

The simplified canonical path should follow these _rules_:

*   The path must start with a single slash `'/'`.
*   Directories within the path must be separated by exactly one slash `'/'`.
*   The path must not end with a slash `'/'`, unless it is the root directory.
*   The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.

Return the **simplified canonical path**.

**Example 1:**

**Input:** path = "/home/"

**Output:** "/home"

**Explanation:**

The trailing slash should be removed.

**Example 2:**

**Input:** path = "/home//foo/"

**Output:** "/home/foo"

**Explanation:**

Multiple consecutive slashes are replaced by a single one.

**Example 3:**

**Input:** path = "/home/user/Documents/../Pictures"

**Output:** "/home/user/Pictures"

**Explanation:**

A double period `".."` refers to the directory up a level (the parent directory).

**Example 4:**

**Input:** path = "/../"

**Output:** "/"

**Explanation:**

Going one level up from the root directory is not possible.

**Example 5:**

**Input:** path = "/.../a/../b/c/../d/./"

**Output:** "/.../b/d"

**Explanation:**

`"..."` is a valid name for a directory in this problem.

**Constraints:**

*   `1 <= path.length <= 3000`
*   `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
*   `path` is a valid absolute Unix path.

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public String simplifyPath(String path) {
        Deque<String> stk = new ArrayDeque<>();
        int start = 0;
        while (start < path.length()) {
            while (start < path.length() && path.charAt(start) == '/') {
                start++;
            }
            int end = start;
            while (end < path.length() && path.charAt(end) != '/') {
                end++;
            }
            String s = path.substring(start, end);
            if (s.equals("..")) {
                if (!stk.isEmpty()) {
                    stk.pop();
                }
            } else if (!s.equals(".") && !s.equals("")) {
                stk.push(s);
            }
            start = end + 1;
        }
        StringBuilder ans = new StringBuilder();
        while (!stk.isEmpty()) {
            ans.insert(0, stk.pop());
            ans.insert(0, "/");
        }
        return !ans.isEmpty() ? ans.toString() : "/";
    }
}
```