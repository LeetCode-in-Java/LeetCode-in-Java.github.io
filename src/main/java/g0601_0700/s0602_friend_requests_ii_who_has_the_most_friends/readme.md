[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 602\. Friend Requests II: Who Has the Most Friends

Medium

SQL Schema

Table: `RequestAccepted`

    +----------------+---------+ 
    | Column Name    | Type    | 
    +----------------+---------+ 
    | requester_id   | int     | 
    | accepter_id    | int     | 
    | accept_date    | date    | 
    +----------------+---------+ 
    
(requester_id, accepter_id) is the primary key for this table. This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.

Write an SQL query to find the people who have the most friends and the most friends number.

The test cases are generated so that only one person has the most friends.

The query result format is in the following example.

**Example 1:**

**Input:** RequestAccepted table: 

    +--------------+-------------+-------------+ 
    | requester_id | accepter_id | accept_date | 
    +--------------+-------------+-------------+ 
    | 1            | 2           | 2016/06/03  | 
    | 1            | 3           | 2016/06/08  | 
    | 2            | 3           | 2016/06/08  | 
    | 3            | 4           | 2016/06/09  | 
    +--------------+-------------+-------------+

**Output:** 

    +----+-----+ 
    | id | num | 
    +----+-----+ 
    | 3  | 3   | 
    +----+-----+

**Explanation:** The person with id 3 is a friend of people 1, 2, and 4, so he has three friends in total, which is the most number than any others.

**Follow up:** In the real world, multiple people could have the same most number of friends. Could you find all these people in this case?

## Solution

```sql
# Write your MySQL query statement below
SELECT req AS id, COUNT(acc) AS num
FROM
((SELECT requester_id AS req, accepter_id AS acc
FROM requestaccepted)
UNION
(SELECT accepter_id AS req, requester_id AS acc
FROM requestaccepted)) t
GROUP BY req
ORDER BY num DESC
LIMIT 1
```