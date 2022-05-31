## 630\. Course Schedule III

Hard

There are `n` different online courses numbered from `1` to `n`. You are given an array `courses` where <code>courses[i] = [duration<sub>i</sub>, lastDay<sub>i</sub>]</code> indicate that the <code>i<sup>th</sup></code> course should be taken **continuously** for <code>duration<sub>i</sub></code> days and must be finished before or on <code>lastDay<sub>i</sub></code>.

You will start on the <code>1<sup>st</sup></code> day and you cannot take two or more courses simultaneously.

Return _the maximum number of courses that you can take_.

**Example 1:**

**Input:** courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]

**Output:** 3 Explanation: There are totally 4 courses, but you can take 3 courses at most: First, take the 1<sup>st</sup> course, it costs 100 days so you will finish it on the 100<sup>th</sup> day, and ready to take the next course on the 101<sup>st</sup> day. Second, take the 3<sup>rd</sup> course, it costs 1000 days so you will finish it on the 1100<sup>th</sup> day, and ready to take the next course on the 1101<sup>st</sup> day. Third, take the 2<sup>nd</sup> course, it costs 200 days so you will finish it on the 1300<sup>th</sup> day. The 4<sup>th</sup> course cannot be taken now, since you will finish it on the 3300<sup>th</sup> day, which exceeds the closed date.

**Example 2:**

**Input:** courses = [[1,2]]

**Output:** 1

**Example 3:**

**Input:** courses = [[3,2],[4,3]]

**Output:** 0

**Constraints:**

*   <code>1 <= courses.length <= 10<sup>4</sup></code>
*   <code>1 <= duration<sub>i</sub>, lastDay<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

@SuppressWarnings("java:S135")
public class Solution {
    public int scheduleCourse(int[][] courses) {
        // Sort the courses based on their deadline date.
        Arrays.sort(courses, (a, b) -> a[1] - b[1]);
        // Only the duration is stored. We don't care which course
        // is the longest, we only care about the total courses can
        // be taken.
        // If the question wants the course ids to be returned.
        // Consider use a Pair<Duration, CourseId> int pair.
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        // Total time consumed.
        int time = 0;
        // At the given time `course`, the overall "time limit" is
        // course[1]. All courses in pq is already 'valid'. But
        // adding this course[0] might exceed the course[1] limit.
        for (int[] course : courses) {
            // If adding this course doesn't exceed. Let's add it
            // for now. (Greedy algo). We might remove it later if
            // we have a "better" solution at that time.
            if (time + course[0] <= course[1]) {
                time += course[0];
                pq.offer(course[0]);
            } else {
                // If adding this ecxeeds the limit. We can still add it
                // if-and-only-if there are courses longer than current
                // one. If so, by removing a longer course, current shorter
                // course can fit in for sure. Although the total course
                // count is the same, the overall time consumed is shorter.
                // Which gives us more room for future courses.
                // Remove any course that is longer than current course
                // will work, but we remove the longest one with the help
                // of heap (pq).
                if (!pq.isEmpty() && pq.peek() > course[0]) {
                    time -= pq.poll();
                    time += course[0];
                    pq.offer(course[0]);
                }
                // If no course in consider (pq) is shorter than the
                // current course. It is safe to discard it.
            }
        }
        return pq.size();
    }
}
```