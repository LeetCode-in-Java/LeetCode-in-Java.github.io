[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3626\. Find Stores with Inventory Imbalance

Medium

Table: `stores`

    +------------+----------+
    | Column Name| Type     |
    +------------+----------+
    | store_id   | int      |
    | store_name | varchar  |
    | location   | varchar  |
    +------------+----------+

    store_id is the unique identifier for this table.
    Each row contains information about a store and its location.

Table: `inventory`

    +--------------+----------+
    | Column Name  | Type     |
    +--------------+----------+
    | inventory_id | int      |
    | store_id     | int      |
    | product_name | varchar  |
    | quantity     | int      |
    | price        | decimal  |
    +--------------+----------+

    inventory_id is the unique identifier for this table.
    Each row represents the inventory of a specific product at a specific store.

Write a solution to find stores that have **inventory imbalance** - stores where the most expensive product has lower stock than the cheapest product.

*   For each store, identify the **most expensive product** (highest price) and its quantity
*   For each store, identify the **cheapest product** (lowest price) and its quantity
*   A store has inventory imbalance if the most expensive product's quantity is **less than** the cheapest product's quantity
*   Calculate the **imbalance ratio** as (cheapest\_quantity / most\_expensive\_quantity)
*   **Round** the imbalance ratio to **2** decimal places
*   Only include stores that have **at least** `3` **different products**

Return _the result table ordered by imbalance ratio in **descending** order, then by store name in **ascending** order_.

The result format is in the following example.

**Example:**

**Input:**

stores table:

    +----------+----------------+-------------+
    | store_id | store_name     | location    |
    +----------+----------------+-------------+
    | 1        | Downtown Tech  | New York    |
    | 2        | Suburb Mall    | Chicago     |
    | 3        | City Center    | Los Angeles |
    | 4        | Corner Shop    | Miami       |
    | 5        | Plaza Store    | Seattle     |
    +----------+----------------+-------------+

inventory table:

    +--------------+----------+--------------+----------+--------+
    | inventory_id | store_id | product_name | quantity | price  |
    +--------------+----------+--------------+----------+--------+
    | 1            | 1        | Laptop       | 5        | 999.99 |
    | 2            | 1        | Mouse        | 50       | 19.99  |
    | 3            | 1        | Keyboard     | 25       | 79.99  |
    | 4            | 1        | Monitor      | 15       | 299.99 |
    | 5            | 2        | Phone        | 3        | 699.99 |
    | 6            | 2        | Charger      | 100      | 25.99  |
    | 7            | 2        | Case         | 75       | 15.99  |
    | 8            | 2        | Headphones   | 20       | 149.99 |
    | 9            | 3        | Tablet       | 2        | 499.99 |
    | 10           | 3        | Stylus       | 80       | 29.99  |
    | 11           | 3        | Cover        | 60       | 39.99  |
    | 12           | 4        | Watch        | 10       | 299.99 |
    | 13           | 4        | Band         | 25       | 49.99  |
    | 14           | 5        | Camera       | 8        | 599.99 |
    | 15           | 5        | Lens         | 12       | 199.99 |
    +--------------+----------+--------------+----------+--------+

**Output:**

    +----------+----------------+-------------+------------------+--------------------+------------------+
    | store_id | store_name     | location    | most_exp_product | cheapest_product   | imbalance_ratio  |
    +----------+----------------+-------------+------------------+--------------------+------------------+
    | 3        | City Center    | Los Angeles | Tablet           | Stylus             | 40.00            |
    | 1        | Downtown Tech  | New York    | Laptop           | Mouse              | 10.00            |
    | 2        | Suburb Mall    | Chicago     | Phone            | Case               | 25.00            |
    +----------+----------------+-------------+------------------+--------------------+------------------+

**Explanation:**

*   **Downtown Tech (store\_id = 1):**
    *   Most expensive product: Laptop ($999.99) with quantity 5
    *   Cheapest product: Mouse ($19.99) with quantity 50
    *   Inventory imbalance: 5 < 50 (expensive product has lower stock)
    *   Imbalance ratio: 50 / 5 = 10.00
    *   Has 4 products (≥ 3), so qualifies
*   **Suburb Mall (store\_id = 2):**
    *   Most expensive product: Phone ($699.99) with quantity 3
    *   Cheapest product: Case ($15.99) with quantity 75
    *   Inventory imbalance: 3 < 75 (expensive product has lower stock)
    *   Imbalance ratio: 75 / 3 = 25.00
    *   Has 4 products (≥ 3), so qualifies
*   **City Center (store\_id = 3):**
    *   Most expensive product: Tablet ($499.99) with quantity 2
    *   Cheapest product: Stylus ($29.99) with quantity 80
    *   Inventory imbalance: 2 < 80 (expensive product has lower stock)
    *   Imbalance ratio: 80 / 2 = 40.00
    *   Has 3 products (≥ 3), so qualifies
*   **Stores not included:**
    *   Corner Shop (store\_id = 4): Only has 2 products (Watch, Band) - doesn't meet minimum 3 products requirement
    *   Plaza Store (store\_id = 5): Only has 2 products (Camera, Lens) - doesn't meet minimum 3 products requirement

The Results table is ordered by imbalance ratio in descending order, then by store name in ascending order

## Solution

```sql
# Write your MySQL query statement below
WITH store_product_check AS (
    SELECT 
        s.store_id,
        s.store_name,
        s.location,
        COUNT(i.inventory_id) AS store_product_ct
    FROM 
        stores s
        JOIN inventory i ON s.store_id = i.store_id
    GROUP BY 
        s.store_id,
        s.store_name,
        s.location
    HAVING 
        COUNT(i.inventory_id) >= 3
),
store_product_ranked AS (
    SELECT 
        s.store_id,
        s.store_name,
        s.location,
        i.inventory_id,
        i.product_name,
        i.quantity,
        i.price,
        ROW_NUMBER() OVER (PARTITION BY s.store_id ORDER BY i.price ASC) AS low_price_rk,
        ROW_NUMBER() OVER (PARTITION BY s.store_id ORDER BY i.price DESC) AS high_price_rk
    FROM
        stores s
        JOIN inventory i ON s.store_id = i.store_id
),
high_low_price AS (
    SELECT 
        spc.store_id,
        spc.store_name,
        spc.location,
        lp.product_name AS low_price_product_name,
        lp.quantity + 0.0 AS low_price_quantity,
        hp.product_name AS high_price_product_name,
        hp.quantity + 0.0 AS high_price_quantity
    FROM 
        store_product_check spc
        JOIN store_product_ranked lp 
            ON spc.store_id = lp.store_id AND lp.low_price_rk = 1
        JOIN store_product_ranked hp 
            ON spc.store_id = hp.store_id AND hp.high_price_rk = 1
)
SELECT 
    hlp.store_id,
    hlp.store_name,
    hlp.location,
    hlp.high_price_product_name AS most_exp_product,
    hlp.low_price_product_name AS cheapest_product,
    ROUND(hlp.low_price_quantity / hlp.high_price_quantity, 2) AS imbalance_ratio
FROM
    high_low_price hlp
WHERE
    hlp.high_price_quantity < hlp.low_price_quantity
ORDER BY
    imbalance_ratio DESC, 
    hlp.store_name ASC;
```