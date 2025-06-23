[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3586\. Find COVID Recovery Patients

Medium

Table: `patients`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | patient_id  | int     |
    | patient_name| varchar |
    | age         | int     |
    +-------------+---------+
    patient_id is the unique identifier for this table.
    Each row contains information about a patient.

Table: `covid_tests`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | test_id     | int     |
    | patient_id  | int     |
    | test_date   | date    |
    | result      | varchar |
    +-------------+---------+
    test_id is the unique identifier for this table.
    Each row represents a COVID test result. The result can be Positive, Negative, or Inconclusive. 

Write a solution to find patients who have **recovered from COVID** - patients who tested positive but later tested negative.

*   A patient is considered recovered if they have **at least one** **Positive** test followed by at least one **Negative** test on a **later date**
*   Calculate the **recovery time** in days as the **difference** between the **first positive test** and the **first negative test** after that **positive test**
*   **Only include** patients who have both positive and negative test results

Return _the result table ordered by_ `recovery_time` _in **ascending** order, then by_ `patient_name` _in **ascending** order_.

The result format is in the following example.

**Example:**

**Input:**

patients table:

    +------------+--------------+-----+
    | patient_id | patient_name | age |
    +------------+--------------+-----+
    | 1          | Alice Smith  | 28  |
    | 2          | Bob Johnson  | 35  |
    | 3          | Carol Davis  | 42  |
    | 4          | David Wilson | 31  |
    | 5          | Emma Brown   | 29  |
    +------------+--------------+-----+ 

covid\_tests table:

    +---------+------------+------------+--------------+
    | test_id | patient_id | test_date  | result       |
    |---------|------------|------------|--------------|
    | 1       | 1          | 2023-01-15 | Positive     |
    | 2       | 1          | 2023-01-25 | Negative     |
    | 3       | 2          | 2023-02-01 | Positive     |
    | 4       | 2          | 2023-02-05 | Inconclusive |
    | 5       | 2          | 2023-02-12 | Negative     |
    | 6       | 3          | 2023-01-20 | Negative     |
    | 7       | 3          | 2023-02-10 | Positive     |
    | 8       | 3          | 2023-02-20 | Negative     |
    | 9       | 4          | 2023-01-10 | Positive     |
    | 10      | 4          | 2023-01-18 | Positive     |
    | 11      | 5          | 2023-02-15 | Negative     |
    | 12      | 5          | 2023-02-20 | Negative     |
    +---------+------------+------------+--------------+

**Output:**

    +------------+--------------+-----+---------------+
    | patient_id | patient_name | age | recovery_time |
    |------------|--------------|-----|---------------|
    | 1          | Alice Smith  | 28  | 10            |
    | 3          | Carol Davis  | 42  | 10            |
    | 2          | Bob Johnson  | 35  | 11            |
    +------------+--------------+-----+---------------+

**Explanation:**

*   **Alice Smith (patient\_id = 1):**
    *   First positive test: 2023-01-15
    *   First negative test after positive: 2023-01-25
    *   Recovery time: 25 - 15 = 10 days
*   **Bob Johnson (patient\_id = 2):**
    *   First positive test: 2023-02-01
    *   Inconclusive test on 2023-02-05 (ignored for recovery calculation)
    *   First negative test after positive: 2023-02-12
    *   Recovery time: 12 - 1 = 11 days
*   **Carol Davis (patient\_id = 3):**
    *   Had negative test on 2023-01-20 (before positive test)
    *   First positive test: 2023-02-10
    *   First negative test after positive: 2023-02-20
    *   Recovery time: 20 - 10 = 10 days
*   **Patients not included:**
    *   David Wilson (patient\_id = 4): Only has positive tests, no negative test after positive
    *   Emma Brown (patient\_id = 5): Only has negative tests, never tested positive

Output table is ordered by recovery\_time in ascending order, and then by patient\_name in ascending order.

## Solution

```sql
# Write your MySQL query statement below
-- mysql
-- SELECT 
--     p.patient_id,
--     p.patient_name,
--     p.age,
--     DATEDIFF(
--         min(neg.test_date),
--         min(pos.test_date)
--     ) AS recovery_time
-- FROM 
--     patients p
--     JOIN covid_tests pos ON
--         p.patient_id = pos.patient_id AND pos.result = 'Positive'
--     JOIN covid_tests neg ON 
--         p.patient_id = neg.patient_id AND neg.result = 'Negative'
-- WHERE 
--     neg.test_date > pos.test_date
-- GROUP BY 
--     p.patient_id, p.patient_name, p.age
-- ORDER BY 
--     recovery_time, p.patient_name;
select 
    p.patient_id,
    p.patient_name,
    p.age,
    datediff(
        day, 
        pos.first_pos_date, 
        neg.first_neg_date
    ) as recovery_time
from 
    patients p
    join (
        select patient_id, min(test_date) as first_pos_date 
        from covid_tests 
        where result = 'Positive'
        group by patient_id
    ) pos on p.patient_id = pos.patient_id
    join (
        select 
            c1.patient_id, 
            min(c1.test_date) as first_neg_date
        from 
            covid_tests c1
            join (
                select patient_id, min(test_date) as first_pos_date 
                from covid_tests 
                where result = 'Positive'
                group by patient_id
            ) p2 on c1.patient_id = p2.patient_id
        where 
            c1.result = 'Negative'
            and c1.test_date > p2.first_pos_date
        group by c1.patient_id
    ) neg on p.patient_id = neg.patient_id
order by 
    recovery_time ASC, p.patient_name ASC;
```