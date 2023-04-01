[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 729\. My Calendar I

Medium

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **double booking**.

A **double booking** happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the `MyCalendar` class:

*   `MyCalendar()` Initializes the calendar object.
*   `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a **double booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**

**Input** 

["MyCalendar", "book", "book", "book"] 

[[], [10, 20], [15, 25], [20, 30]]

**Output:** [null, true, false, true]

**Explanation:** 

    MyCalendar myCalendar = new MyCalendar(); 
    myCalendar.book(10, 20); // return True 
    myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event. 
    myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.

**Constraints:**

*   <code>0 <= start < end <= 10<sup>9</sup></code>
*   At most `1000` calls will be made to `book`.

## Solution

```java
import java.util.TreeSet;

@SuppressWarnings("java:S1210")
public class MyCalendar {
    static class Meeting implements Comparable<Meeting> {
        public final int start;
        public final int end;

        public Meeting(int start, int end) {
            this.start = start;
            this.end = end;
        }

        @Override
        public int compareTo(Meeting anotherMeeting) {
            return this.start - anotherMeeting.start;
        }
    }

    private final TreeSet<Meeting> meetings;

    public MyCalendar() {
        meetings = new TreeSet<>();
    }

    public boolean book(int start, int end) {
        Meeting meetingToBook = new Meeting(start, end);
        Meeting prevMeeting = meetings.floor(meetingToBook);
        Meeting nextMeeting = meetings.ceiling(meetingToBook);
        if ((prevMeeting == null || prevMeeting.end <= meetingToBook.start)
                && (nextMeeting == null || meetingToBook.end <= nextMeeting.start)) {

            meetings.add(meetingToBook);
            return true;
        }
        return false;
    }
}

/*
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```