[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3617\. Find Students with Study Spiral Pattern

Hard

Table: `students`

    +--------------+---------+
    | Column Name  | Type    |
    +--------------+---------+
    | student_id   | int     |
    | student_name | varchar |
    | major        | varchar |
    +--------------+---------+
    student_id is the unique identifier for this table.
    Each row contains information about a student and their academic major. 

Table: `study_sessions`

    +---------------+---------+
    | Column Name   | Type    |
    +---------------+---------+
    | session_id    | int     |
    | student_id    | int     |
    | subject       | varchar |
    | session_date  | date    |
    | hours_studied | decimal |
    +---------------+---------+
    session_id is the unique identifier for this table.
    Each row represents a study session by a student for a specific subject. 

Write a solution to find students who follow the **Study Spiral Pattern** - students who consistently study multiple subjects in a rotating cycle.

*   A Study Spiral Pattern means a student studies at least `3` **different subjects** in a repeating sequence
*   The pattern must repeat for **at least** `2` **complete cycles** (minimum `6` study sessions)
*   Sessions must be **consecutive dates** with no gaps longer than `2` days between sessions
*   Calculate the **cycle length** (number of different subjects in the pattern)
*   Calculate the **total study hours** across all sessions in the pattern
*   Only include students with cycle length of **at least** `3` **subjects**

Return _the result table ordered by cycle length in **descending** order, then by total study hours in **descending** order_.

The result format is in the following example.

**Example:**

**Input:**

students table:

| student_id | student_name | major             |
|------------|--------------|-------------------|
| 1          | Alice Chen   | Computer Science  |
| 2          | Bob Johnson  | Mathematics       |
| 3          | Carol Davis  | Physics           |
| 4          | David Wilson | Chemistry         |
| 5          | Emma Brown   | Biology           |

study\_sessions table:

| session_id | student_id | subject    | session_date | hours_studied  |
|------------|------------|------------|--------------|----------------|
| 1          | 1          | Math       | 2023-10-01   | 2.5            |
| 2          | 1          | Physics    | 2023-10-02   | 3.0            |
| 3          | 1          | Chemistry  | 2023-10-03   | 2.0            |
| 4          | 1          | Math       | 2023-10-04   | 2.5            |
| 5          | 1          | Physics    | 2023-10-05   | 3.0            |
| 6          | 1          | Chemistry  | 2023-10-06   | 2.0            |
| 7          | 2          | Algebra    | 2023-10-01   | 4.0            |
| 8          | 2          | Calculus   | 2023-10-02   | 3.5            |
| 9          | 2          | Statistics | 2023-10-03   | 2.5            |
| 10         | 2          | Geometry   | 2023-10-04   | 3.0            |
| 11         | 2          | Algebra    | 2023-10-05   | 4.0            |
| 12         | 2          | Calculus   | 2023-10-06   | 3.5            |
| 13         | 2          | Statistics | 2023-10-07   | 2.5            |
| 14         | 2          | Geometry   | 2023-10-08   | 3.0            |
| 15         | 3          | Biology    | 2023-10-01   | 2.0            |
| 16         | 3          | Chemistry  | 2023-10-02   | 2.5            |
| 17         | 3          | Biology    | 2023-10-03   | 2.0            |
| 18         | 3          | Chemistry  | 2023-10-04   | 2.5            |
| 19         | 4          | Organic    | 2023-10-01   | 3.0            |
| 20         | 4          | Physical   | 2023-10-05   | 2.5            |

**Output:**

| student_id | student_name | major             | cycle_length | total_study_hours |
|------------|--------------|-------------------|--------------|-------------------|
| 2          | Bob Johnson  | Mathematics       | 4            | 26.0              |
| 1          | Alice Chen   | Computer Science  | 3            | 15.0              |

**Explanation:**

*   **Alice Chen (student\_id = 1):**
    *   Study sequence: Math → Physics → Chemistry → Math → Physics → Chemistry
    *   Pattern: 3 subjects (Math, Physics, Chemistry) repeating for 2 complete cycles
    *   Consecutive dates: Oct 1-6 with no gaps > 2 days
    *   Cycle length: 3 subjects
    *   Total hours: 2.5 + 3.0 + 2.0 + 2.5 + 3.0 + 2.0 = 15.0 hours
*   **Bob Johnson (student\_id = 2):**
    *   Study sequence: Algebra → Calculus → Statistics → Geometry → Algebra → Calculus → Statistics → Geometry
    *   Pattern: 4 subjects (Algebra, Calculus, Statistics, Geometry) repeating for 2 complete cycles
    *   Consecutive dates: Oct 1-8 with no gaps > 2 days
    *   Cycle length: 4 subjects
    *   Total hours: 4.0 + 3.5 + 2.5 + 3.0 + 4.0 + 3.5 + 2.5 + 3.0 = 26.0 hours
*   **Students not included:**
    *   Carol Davis (student\_id = 3): Only 2 subjects (Biology, Chemistry) - doesn't meet minimum 3 subjects requirement
    *   David Wilson (student\_id = 4): Only 2 study sessions with a 4-day gap - doesn't meet consecutive dates requirement
    *   Emma Brown (student\_id = 5): No study sessions recorded

The result table is ordered by cycle\_length in descending order, then by total\_study\_hours in descending order.

## Solution

```sql
# Write your MySQL query statement below
WITH studentstudysummary AS (
    SELECT
        student_id,
        SUM(hours_studied) AS total_study_hours,
        COUNT(DISTINCT subject) AS cycle_length
    FROM
        study_sessions
    GROUP BY
        student_id
    HAVING
        COUNT(DISTINCT subject) >= 3
),
rankedstudysessionswithgaps AS (
    SELECT
        ss.student_id,
        ss.subject,
        ss.session_date,
        DATEDIFF(
            LEAD(ss.session_date, 1, ss.session_date)
                OVER (PARTITION BY ss.student_id ORDER BY ss.session_date),
            ss.session_date
        ) AS gap_to_next_session,
        ROW_NUMBER() OVER (PARTITION BY ss.student_id ORDER BY ss.session_date) AS rn,
        sss.total_study_hours,
        sss.cycle_length
    FROM
        study_sessions ss
        INNER JOIN studentstudysummary sss
            ON ss.student_id = sss.student_id
),
cyclicstudents AS (
    SELECT
        rss1.student_id,
        rss1.cycle_length,
        rss1.total_study_hours
    FROM
        rankedstudysessionswithgaps rss1
        INNER JOIN rankedstudysessionswithgaps rss2
            ON rss1.student_id = rss2.student_id
            AND rss2.rn = rss1.rn + rss1.cycle_length
            AND rss1.subject = rss2.subject
    WHERE
        rss1.gap_to_next_session < 3
        AND rss2.gap_to_next_session < 3
    GROUP BY
        rss1.student_id,
        rss1.cycle_length,
        rss1.total_study_hours
    HAVING
        COUNT(DISTINCT rss1.subject) >= 3
)
SELECT
    s.student_id,
    s.student_name,
    s.major,
    cs.cycle_length,
    cs.total_study_hours
FROM
    cyclicstudents cs
    INNER JOIN students s
        ON cs.student_id = s.student_id
ORDER BY
    cs.cycle_length DESC,
    cs.total_study_hours DESC;
```