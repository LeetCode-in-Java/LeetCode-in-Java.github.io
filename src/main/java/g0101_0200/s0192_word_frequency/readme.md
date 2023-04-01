[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 192\. Word Frequency

Medium

Write a bash script to calculate the frequency of each word in a text file `words.txt`.

For simplicity sake, you may assume:

*   `words.txt` contains only lowercase characters and space `' '` characters.
*   Each word must consist of lowercase characters only.
*   Words are separated by one or more whitespace characters.

**Example:**

Assume that `words.txt` has the following content:

    the day is sunny the the
    the sunny is is 

Your script should output the following, sorted by descending frequency:

    the 4
    is 3
    sunny 2
    day 1 

**Note:**

*   Don't worry about handling ties, it is guaranteed that each word's frequency count is unique.
*   Could you write it in one-line using [Unix pipes](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-4.html)?

## Solution

```bash
# Read from the file words.txt and output the word frequency list to stdout.
sed -e 's/ /\n/g' words.txt | sed -e '/^$/d' | sort | uniq -c | sort -r | awk '{print $2" "$1}'
```