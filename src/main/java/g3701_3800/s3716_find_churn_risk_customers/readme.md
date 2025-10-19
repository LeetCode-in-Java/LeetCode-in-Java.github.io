[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3716\. Find Churn Risk Customers

Medium

Table: `subscription_events`

    +------------------+---------+
    | Column Name      | Type    |
    +------------------+---------+
    | event_id         | int     |
    | user_id          | int     |
    | event_date       | date    |
    | event_type       | varchar |
    | plan_name        | varchar |
    | monthly_amount   | decimal |
    +------------------+---------+
    event_id is the unique identifier for this table.
    event_type can be start, upgrade, downgrade, or cancel.
    plan_name can be basic, standard, premium, or NULL (when event_type is cancel).
    monthly_amount represents the monthly subscription cost after this event.
    For cancel events, monthly_amount is 0. 

Write a solution to **Find Churn Risk Customers** - users who show warning signs before churning. A user is considered **churn risk customer** if they meet ALL the following criteria:

*   Currently have an **active subscription** (their last event is not cancel).
*   Have performed **at least one** downgrade in their subscription history.
*   Their **current plan revenue** is less than `50%` of their historical maximum plan revenue.
*   Have been a subscriber for **at least** `60` days.

Return _the result table ordered by_ `days_as_subscriber` _in **descending** order, then by_ `user_id` _in **ascending** order_.

The result format is in the following example.

**Example:**

**Input:**

subscription\_events table:

    +----------+---------+------------+------------+-----------+----------------+
    | event_id | user_id | event_date | event_type | plan_name | monthly_amount |
    +----------+---------+------------+------------+-----------+----------------+
    | 1        | 501     | 2024-01-01 | start      | premium   | 29.99          |
    | 2        | 501     | 2024-02-15 | downgrade  | standard  | 19.99          |
    | 3        | 501     | 2024-03-20 | downgrade  | basic     | 9.99           |
    | 4        | 502     | 2024-01-05 | start      | standard  | 19.99          |
    | 5        | 502     | 2024-02-10 | upgrade    | premium   | 29.99          |
    | 6        | 502     | 2024-03-15 | downgrade  | basic     | 9.99           |
    | 7        | 503     | 2024-01-10 | start      | basic     | 9.99           |
    | 8        | 503     | 2024-02-20 | upgrade    | standard  | 19.99          |
    | 9        | 503     | 2024-03-25 | upgrade    | premium   | 29.99          |
    | 10       | 504     | 2024-01-15 | start      | premium   | 29.99          |
    | 11       | 504     | 2024-03-01 | downgrade  | standard  | 19.99          |
    | 12       | 504     | 2024-03-30 | cancel     | NULL      | 0.00           |
    | 13       | 505     | 2024-02-01 | start      | basic     | 9.99           |
    | 14       | 505     | 2024-02-28 | upgrade    | standard  | 19.99          |
    | 15       | 506     | 2024-01-20 | start      | premium   | 29.99          |
    | 16       | 506     | 2024-03-10 | downgrade  | basic     | 9.99           |
    +----------+---------+------------+------------+-----------+----------------+

**Output:**

    +---------+--------------+------------------------+-----------------------+--------------------+
    | user_id | current_plan | current_monthly_amount | max_historical_amount | days_as_subscriber |
    +---------+--------------+------------------------+-----------------------+--------------------+
    | 501     | basic        | 9.99                   | 29.99                 | 79                 |
    | 502     | basic        | 9.99                   | 29.99                 | 69                 |
    +---------+--------------+------------------------+-----------------------+--------------------+

**Explanation:**

*   **User 501**:
    *   Currently active: Last event is downgrade to basic (not cancelled)
    *   Has downgrades: Yes, 2 downgrades in history
    *   Current revenue (9.99) vs max (29.99): 9.99/29.99 = 33.3% (less than 50%)
    *   Days as subscriber: Jan 1 to Mar 20 = 79 days (at least 60)
    *   Result: **Churn Risk Customer**
*   **User 502**:
    *   Currently active: Last event is downgrade to basic (not cancelled)
    *   Has downgrades: Yes, 1 downgrade in history
    *   Current revenue (9.99) vs max (29.99): 9.99/29.99 = 33.3% (less than 50%)
    *   Days as subscriber: Jan 5 to Mar 15 = 70 days (at least 60)
    *   Result: **Churn Risk Customer**
*   **User 503**:
    *   Currently active: Last event is upgrade to premium (not cancelled)
    *   Has downgrades: No downgrades in history
    *   Result: **Not at-risk** (no downgrade history)
*   **User 504**:
    *   Currently active: Last event is cancel
    *   Result: **Not at-risk** (subscription cancelled)
*   **User 505**:
    *   Currently active: Last event is 'upgrade' to standard (not cancelled)
    *   Has downgrades: No downgrades in history
    *   Result: **Not at-risk** (no downgrade history)
*   **User 506**:
    *   Currently active: Last event is downgrade to basic (not cancelled)
    *   Has downgrades: Yes, 1 downgrade in history
    *   Current revenue (9.99) vs max (29.99): 9.99/29.99 = 33.3% (less than 50%)
    *   Days as subscriber: Jan 20 to Mar 10 = 50 days (less than 60)
    *   Result: **Not at-risk** (insufficient subscription duration)

Result table is ordered by days\_as\_subscriber DESC, then user\_id ASC.

**Note:** days\_as\_subscriber is calculated from the first event date to the last event date for each user.

## Solution

```sql
# Write your MySQL query statement below
WITH UserStats AS (
    SELECT 
        user_id, 
        MIN(CASE WHEN event_type = 'start' THEN event_date END) AS start_date,
        MAX(event_date) AS last_event,
        MAX(monthly_amount) AS max_revenue,
        SUM(CASE WHEN event_type = 'downgrade' THEN 1 ELSE 0 END) AS downgrade_count
    FROM subscription_events
    GROUP BY user_id
)
SELECT
    us.user_id,
    se.plan_name AS current_plan,
    se.monthly_amount AS current_monthly_amount,
    us.max_revenue AS max_historical_amount,
    TIMESTAMPDIFF(DAY, us.start_date, us.last_event) AS days_as_subscriber
FROM UserStats us
JOIN subscription_events se
    ON us.user_id = se.user_id
   AND us.last_event = se.event_date
WHERE se.event_type <> 'cancel'
  AND us.downgrade_count > 0
  AND se.monthly_amount * 2 < us.max_revenue
  AND TIMESTAMPDIFF(DAY, us.start_date, us.last_event) > 60
ORDER BY
    TIMESTAMPDIFF(DAY, us.start_date, us.last_event) DESC,
    us.user_id ASC;
```