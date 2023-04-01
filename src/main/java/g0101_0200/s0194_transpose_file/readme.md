[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

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
wordcount=$(head -1 file.txt | wc -w)
col_n=1
while [[ $col_n -le $wordcount ]]; do
	awk "{ print \$$col_n }" file.txt | paste -sd " "
	col_n=$((col_n + 1))
done
```