[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 195\. Tenth Line

Easy

Given a text file `file.txt`, print just the 10th line of the file.

**Example:**

Assume that `file.txt` has the following content:

    Line 1
    Line 2
    Line 3
    Line 4
    Line 5
    Line 6
    Line 7
    Line 8
    Line 9
    Line 10 

Your script should output the tenth line, which is:

    Line 10 

**Note:**
1\. If the file contains less than 10 lines, what should you output?
2\. There's at least three different solutions. Try to explore all possibilities.

## Solution

```bash
# Read from the file file.txt and output the tenth line to stdout.
sed -n 10p file.txt
```