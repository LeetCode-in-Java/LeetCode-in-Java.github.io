[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3808\. Find Emotionally Consistent Users

Medium

Table: `reactions`

    +--------------+---------+
    | Column Name  | Type    |
    +--------------+---------+
    | user_id      | int     |
    | content_id   | int     |
    | reaction     | varchar |
    +--------------+---------+

(user_id, content_id) is the primary key (unique value) for this table.
Each row represents a reaction given by a user to a piece of content.

Write a solution to identify **emotionally consistent users** based on the following requirements:

* For each user, count the total number of reactions they have given.
* Only include users who have reacted to **at least** `5` **different content items**.
* A user is considered **emotionally consistent** if **at least** `60%` of their reactions are of the **same type**.

Return _the result table ordered by_ `reaction_ratio` _in **descending** order and then by_ `user_id` _in **ascending** order_.

**Note:**

* `reaction_ratio` should be rounded to `2` decimal places

The result format is in the following example.

**Example:**

**Input:**

reactions table:

    +---------+------------+----------+
    | user_id | content_id | reaction |
    +---------+------------+----------+
    | 1       | 101        | like     |
    | 1       | 102        | like     |
    | 1       | 103        | like     |
    | 1       | 104        | wow      |
    | 1       | 105        | like     |
    | 2       | 201        | like     |
    | 2       | 202        | wow      |
    | 2       | 203        | sad      |
    | 2       | 204        | like     |
    | 2       | 205        | wow      |
    | 3       | 301        | love     |
    | 3       | 302        | love     |
    | 3       | 303        | love     |
    | 3       | 304        | love     |
    | 3       | 305        | love     |
    +---------+------------+----------+

**Output:**

    +---------+-------------------+----------------+
    | user_id | dominant_reaction | reaction_ratio |
    +---------+-------------------+----------------+
    | 3       | love              | 1.00           |
    | 1       | like              | 0.80           |
    +---------+-------------------+----------------+

**Explanation:**

* **User 1**:
    * Total reactions = 5
    * like appears 4 times
    * reaction_ratio = 4 / 5 = 0.80
    * Meets the 60% consistency requirement
* **User 2**:
    * Total reactions = 5
    * Most frequent reaction appears only 2 times
    * reaction_ratio = 2 / 5 = 0.40
    * Does not meet the consistency requirement
* **User 3**:
    * Total reactions = 5
    * 'love' appears 5 times
    * reaction_ratio = 5 / 5 = 1.00
    * Meets the consistency requirement

The Results table is ordered by reaction_ratio in descending order, then by user_id in ascending order.

## Solution

```sql
# Write your MySQL query statement below
WITH user_selection AS
(SELECT
    user_id,
    COUNT(reaction) AS total_reaction_count
FROM
    reactions
GROUP BY
    user_id
HAVING
    COUNT(DISTINCT content_id) >= 5
),
reaction_counts
AS
(SELECT
    user_id,
    reaction,
    COUNT(*) AS reaction_count
FROM
reactions
group by
    user_id,
    reaction
),
ranked_reactions AS (
    -- Step 2: Use a window function to find the max for each user
    SELECT
        user_id,
        reaction,
        reaction_count,
        RANK() OVER(PARTITION BY user_id ORDER BY reaction_count DESC) as rnk
    FROM reaction_counts
)
SELECT
    rc.user_id,
    rc.reaction AS dominant_reaction,
    ROUND(reaction_count / total_reaction_count, 2) AS reaction_ratio
FROM
    ranked_reactions rc
INNER JOIN
    user_selection us
ON
    rc.user_id = us.user_id
WHERE
    rc.rnk = 1
    AND ROUND(reaction_count / total_reaction_count, 2) >= 0.60
ORDER BY
    3 DESC,
    rc.user_id
```