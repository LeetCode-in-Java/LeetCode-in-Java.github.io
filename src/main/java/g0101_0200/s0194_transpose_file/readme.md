[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 194\. Transpose File

Medium

Given a text file `file.txt`, transpose its content.

You may assume that each row has the same number of columns, and each field is separated by the `' '` character.

**Example:**

If `file.txt` has the following content:

    name age
    alice 21
    ryan 30 

Output the following:

    name alice ryan
    age 21 30

## Solution

```bash
# Read from the file file.txt and print its transposed content to stdout.
awk '
{
    for (i = 1; i <= NF; i++) {
        if (NR == 1) {
            a[i] = $i
        } else {
            a[i] = a[i] " " $i
        }
    }
}
END {
    for (i = 1; i <= NF; i++) {
        print a[i]
    }
}' file.txt
```