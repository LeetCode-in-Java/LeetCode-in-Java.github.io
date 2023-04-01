[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1942\. The Number of the Smallest Unoccupied Chair

Medium

There is a party where `n` friends numbered from `0` to `n - 1` are attending. There is an **infinite** number of chairs in this party that are numbered from `0` to `infinity`. When a friend arrives at the party, they sit on the unoccupied chair with the **smallest number**.

*   For example, if chairs `0`, `1`, and `5` are occupied when a friend comes, they will sit on chair number `2`.

When a friend leaves the party, their chair becomes unoccupied at the moment they leave. If another friend arrives at that same moment, they can sit in that chair.

You are given a **0-indexed** 2D integer array `times` where <code>times[i] = [arrival<sub>i</sub>, leaving<sub>i</sub>]</code>, indicating the arrival and leaving times of the <code>i<sup>th</sup></code> friend respectively, and an integer `targetFriend`. All arrival times are **distinct**.

Return _the **chair number** that the friend numbered_ `targetFriend` _will sit on_.

**Example 1:**

**Input:** times = \[\[1,4],[2,3],[4,6]], targetFriend = 1

**Output:** 1

**Explanation:** 

- Friend 0 arrives at time 1 and sits on chair 0. 

- Friend 1 arrives at time 2 and sits on chair 1. 

- Friend 1 leaves at time 3 and chair 1 becomes empty. 

- Friend 0 leaves at time 4 and chair 0 becomes empty. 

- Friend 2 arrives at time 4 and sits on chair 0. 
  
Since friend 1 sat on chair 1, we return 1.

**Example 2:**

**Input:** times = \[\[3,10],[1,5],[2,6]], targetFriend = 0

**Output:** 2

**Explanation:** 

- Friend 1 arrives at time 1 and sits on chair 0. 

- Friend 2 arrives at time 2 and sits on chair 1. 

- Friend 0 arrives at time 3 and sits on chair 2. 

- Friend 1 leaves at time 5 and chair 0 becomes empty. 

- Friend 2 leaves at time 6 and chair 1 becomes empty. 

- Friend 0 leaves at time 10 and chair 2 becomes empty. 
  
Since friend 0 sat on chair 2, we return 2.

**Constraints:**

*   `n == times.length`
*   <code>2 <= n <= 10<sup>4</sup></code>
*   `times[i].length == 2`
*   <code>1 <= arrival<sub>i</sub> < leaving<sub>i</sub> <= 10<sup>5</sup></code>
*   `0 <= targetFriend <= n - 1`
*   Each <code>arrival<sub>i</sub></code> time is **distinct**.

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public int smallestChair(int[][] times, int targetFriend) {
        PriorityQueue<Integer> minheap = new PriorityQueue<>();
        minheap.offer(0);
        Person[] all = new Person[times.length * 2];
        for (int i = 0; i < times.length; i++) {
            all[2 * i] = new Person(i, times[i][0], false, true);
            all[2 * i + 1] = new Person(i, times[i][1], true, false);
        }
        Arrays.sort(
                all,
                (a, b) -> {
                    int i = a.leave ? -1 : 1;
                    int j = b.leave ? -1 : 1;
                    return a.time == b.time ? i - j : a.time - b.time;
                });

        int[] seat = new int[times.length];
        for (int i = 0; true; i++) {
            if (all[i].arrive) {
                if (targetFriend == all[i].idx) {
                    return minheap.peek();
                }
                seat[all[i].idx] = minheap.poll();
                if (minheap.isEmpty()) {
                    minheap.offer(seat[all[i].idx] + 1);
                }
            } else {
                minheap.offer(seat[all[i].idx]);
            }
        }
    }

    private static class Person {
        boolean leave;
        boolean arrive;
        int time;
        int idx;

        Person(int idx, int time, boolean leave, boolean arrive) {
            this.time = time;
            this.leave = leave;
            this.arrive = arrive;
            this.idx = idx;
        }
    }
}
```