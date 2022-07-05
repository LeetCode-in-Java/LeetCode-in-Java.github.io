[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1729\. Find Followers Count

Easy

SQL Schema

Table: `Followers`

    +-------------+------+
    | Column Name | Type |
    +-------------+------+
    | user_id     | int  |
    | follower_id | int  |
    +-------------+------+
    (user_id, follower_id) is the primary key for this table.
    This table contains the IDs of a user and a follower in a social media app where the follower follows the user.

Write an SQL query that will, for each user, return the number of followers.

Return the result table ordered by `user_id`.

The query result format is in the following example.

**Example 1:**

**Input:**

    Followers table:
    +---------+-------------+
    | user_id | follower_id |
    +---------+-------------+
    | 0       | 1           |
    | 1       | 0           |
    | 2       | 0           |
    | 2       | 1           |
    +---------+-------------+

**Output:**

    +---------+----------------+
    | user_id | followers_count|
    +---------+----------------+
    | 0       | 1              |
    | 1       | 1              |
    | 2       | 2              |
    +---------+----------------+

**Explanation:**

The followers of 0 are {1}

The followers of 1 are {0}

The followers of 2 are {0,1}

## Solution

```sql
# Write your MySQL query statement below
select user_id, count(follower_id) as followers_count
from followers
group by user_id
order by user_id
```