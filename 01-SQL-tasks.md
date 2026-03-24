# 🗄️ SQL —  Practice Tasks (Multi-Table Real-World Schema)

> **Goal:** Master SQL queries on a real-world e-commerce database with 6 related tables.
> **Level:** Fresher → Intermediate | **Tool:** MySQL / PostgreSQL / SQLite (any)
> **Recommended Free Tool:** [https://sqliteonline.com](https://sqliteonline.com) (no install needed)

---

## 📋 Progress Tracker

| Task | Topic | Status | 
|------|-------|--------|
| Task 1 | CREATE Tables & INSERT Data | ⬜ |
| Task 2 | Basic SELECT Queries | ⬜ |
| Task 3 | WHERE, AND, OR, IN, BETWEEN | ⬜ |
| Task 4 | Aggregate Functions & GROUP BY | ⬜ |
| Task 5 | JOINS (INNER, LEFT, RIGHT) | ⬜ |
| Task 6 | Multi-Table JOINs (4-6 tables) | ⬜ |
| Task 7 | Subqueries | ⬜ |
| Task 8 | Window Functions | ⬜ |
| Task 9 | CTEs & Views | ⬜ |
| Task 10 | Complex Business Queries | ⬜ |

---

## 🏗️ DATABASE SCHEMA — Create All Tables First

> **Run all CREATE + INSERT statements before starting tasks.**
> This is a simplified e-commerce system with 6 tables.

---

### Table 1: customers

```sql
CREATE TABLE customers (
    customer_id   VARCHAR(10) PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email         VARCHAR(100) UNIQUE,
    phone         VARCHAR(15),
    city          VARCHAR(50),
    state         VARCHAR(50),
    tier          VARCHAR(20) CHECK (tier IN ('Bronze','Silver','Gold','Platinum')),
    registered_on DATE,
    is_active     BOOLEAN DEFAULT TRUE
);

INSERT INTO customers VALUES
('C001','Ravi Kumar','ravi@gmail.com','9876543210','Mumbai','Maharashtra','Gold','2022-01-15',TRUE),
('C002','Priya Sharma','priya@gmail.com','9876543211','Delhi','Delhi','Platinum','2021-06-20',TRUE),
('C003','Amit Singh','amit@gmail.com','9876543212','Bangalore','Karnataka','Silver','2023-03-10',TRUE),
('C004','Sneha Patel','sneha@gmail.com','9876543213','Chennai','Tamil Nadu','Bronze','2022-11-05',TRUE),
('C005','Karan Mehta','karan@gmail.com','9876543214','Hyderabad','Telangana','Gold','2021-09-18',TRUE),
('C006','Divya Nair','divya@gmail.com','9876543215','Pune','Maharashtra','Silver','2023-01-22',TRUE),
('C007','Rohit Gupta','rohit@gmail.com','9876543216','Kolkata','West Bengal','Bronze','2022-07-30',TRUE),
('C008','Anita Joshi','anita@gmail.com','9876543217','Ahmedabad','Gujarat','Platinum','2020-12-01',TRUE),
('C009','Vikram Das','vikram@gmail.com','9876543218','Jaipur','Rajasthan','Silver','2023-05-14',FALSE),
('C010','Meena Iyer','meena@gmail.com','9876543219','Chennai','Tamil Nadu','Gold','2022-04-25',TRUE);
```

---

### Table 2: products

```sql
CREATE TABLE products (
    product_id   VARCHAR(10) PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category     VARCHAR(50),
    brand        VARCHAR(50),
    cost_price   DECIMAL(10,2),
    sell_price   DECIMAL(10,2),
    stock_qty    INT DEFAULT 0,
    warranty_yrs INT
);

INSERT INTO products VALUES
('P001','Laptop Pro 15','Electronics','Dell',55000.00,75000.00,50,2),
('P002','Samsung Galaxy S23','Electronics','Samsung',18000.00,25000.00,120,1),
('P003','iPad Air 5','Electronics','Apple',28000.00,38000.00,80,1),
('P004','Sony Headphones WH1000','Accessories','Sony',2500.00,4500.00,200,1),
('P005','HP Wireless Mouse','Accessories','HP',500.00,900.00,500,1),
('P006','Dell Monitor 27"','Electronics','Dell',18000.00,26000.00,40,2),
('P007','Logitech Keyboard','Accessories','Logitech',800.00,1500.00,300,1),
('P008','iPhone 15','Electronics','Apple',60000.00,80000.00,60,1),
('P009','Samsung Tab S9','Electronics','Samsung',25000.00,35000.00,70,1),
('P010','Bose Earbuds','Accessories','Bose',4000.00,6500.00,150,1);
```

---

### Table 3: salespersons

```sql
CREATE TABLE salespersons (
    salesperson_id   VARCHAR(10) PRIMARY KEY,
    salesperson_name VARCHAR(100) NOT NULL,
    region           VARCHAR(50),
    email            VARCHAR(100),
    hire_date        DATE,
    monthly_target   DECIMAL(12,2)
);

INSERT INTO salespersons VALUES
('SP01','Amit Verma','North','amit.v@company.com','2021-03-01',500000.00),
('SP02','Priya Rao','South','priya.r@company.com','2020-07-15',600000.00),
('SP03','Ravi Tiwari','East','ravi.t@company.com','2022-01-10',450000.00),
('SP04','Sneha Kulkarni','West','sneha.k@company.com','2021-11-20',550000.00),
('SP05','Karan Bose','North','karan.b@company.com','2023-02-05',400000.00);
```

---

### Table 4: orders

```sql
CREATE TABLE orders (
    order_id      VARCHAR(10) PRIMARY KEY,
    customer_id   VARCHAR(10) REFERENCES customers(customer_id),
    salesperson_id VARCHAR(10) REFERENCES salespersons(salesperson_id),
    order_date    DATE NOT NULL,
    delivery_date DATE,
    status        VARCHAR(20) CHECK (status IN ('Pending','Shipped','Delivered','Cancelled','Returned')),
    payment_mode  VARCHAR(20),
    total_amount  DECIMAL(12,2)
);

INSERT INTO orders VALUES
('O001','C001','SP01','2024-01-05','2024-01-10','Delivered','UPI',75000.00),
('O002','C002','SP02','2024-01-08',NULL,'Pending','Card',28500.00),
('O003','C003','SP03','2024-01-12','2024-01-17','Delivered','Cash',38000.00),
('O004','C001','SP01','2024-01-15','2024-01-20','Delivered','UPI',25500.00),
('O005','C004','SP04','2024-01-18',NULL,'Shipped','Card',4500.00),
('O006','C005','SP02','2024-01-22','2024-01-27','Delivered','UPI',80000.00),
('O007','C002','SP01','2024-02-01','2024-02-06','Delivered','Card',4500.00),
('O008','C006','SP03','2024-02-05',NULL,'Pending','UPI',35000.00),
('O009','C003','SP04','2024-02-10','2024-02-15','Delivered','Cash',900.00),
('O010','C007','SP05','2024-02-14','2024-02-19','Delivered','UPI',26000.00),
('O011','C008','SP02','2024-02-20',NULL,'Shipped','Card',82000.00),
('O012','C001','SP03','2024-03-01','2024-03-06','Delivered','UPI',1500.00),
('O013','C009','SP04','2024-03-05','2024-03-10','Returned','Card',38000.00),
('O014','C010','SP01','2024-03-12',NULL,'Pending','UPI',6500.00),
('O015','C005','SP05','2024-03-18','2024-03-23','Delivered','Card',35000.00),
('O016','C002','SP02','2024-03-22','2024-03-27','Delivered','UPI',80000.00),
('O017','C004','SP03','2024-03-25',NULL,'Cancelled','Cash',25000.00),
('O018','C008','SP01','2024-04-02','2024-04-07','Delivered','Card',75000.00),
('O019','C006','SP04','2024-04-08','2024-04-13','Delivered','UPI',900.00),
('O020','C010','SP05','2024-04-15',NULL,'Shipped','Card',6500.00);
```

---

### Table 5: order_items

```sql
CREATE TABLE order_items (
    item_id      INT AUTO_INCREMENT PRIMARY KEY,
    order_id     VARCHAR(10) REFERENCES orders(order_id),
    product_id   VARCHAR(10) REFERENCES products(product_id),
    quantity     INT NOT NULL,
    unit_price   DECIMAL(10,2),
    discount_pct DECIMAL(5,2) DEFAULT 0
);

INSERT INTO order_items (order_id, product_id, quantity, unit_price, discount_pct) VALUES
('O001','P001',1,75000.00,0),
('O002','P002',1,25000.00,0),('O002','P004',1,4500.00,10),
('O003','P003',1,38000.00,0),
('O004','P002',1,25000.00,0),('O004','P005',1,900.00,0),
('O005','P004',1,4500.00,0),
('O006','P001',1,75000.00,0),('O006','P007',2,1500.00,5),
('O007','P010',1,6500.00,0),
('O008','P006',1,26000.00,0),('O008','P007',5,1500.00,5),
('O009','P005',1,900.00,0),
('O010','P002',1,25000.00,0),('O010','P005',1,900.00,0),
('O011','P008',1,80000.00,0),('O011','P004',1,4500.00,5),
('O012','P007',1,1500.00,0),
('O013','P003',1,38000.00,0),
('O014','P010',1,6500.00,0),
('O015','P009',1,35000.00,0),
('O016','P008',1,80000.00,0),
('O017','P002',1,25000.00,0),
('O018','P001',1,75000.00,0),
('O019','P005',1,900.00,0),
('O020','P010',1,6500.00,0);
```

---

### Table 6: returns

```sql
CREATE TABLE returns (
    return_id   VARCHAR(10) PRIMARY KEY,
    order_id    VARCHAR(10) REFERENCES orders(order_id),
    return_date DATE,
    reason      VARCHAR(200),
    refund_amt  DECIMAL(10,2),
    status      VARCHAR(20) CHECK (status IN ('Approved','Pending','Rejected'))
);

INSERT INTO returns VALUES
('R001','O013','2024-03-15','Product defective',38000.00,'Approved'),
('R002','O017','2024-04-01','Wrong item delivered',25000.00,'Pending');
```

---

### 🔗 Relationships Summary

```
customers ──< orders >── salespersons
              orders ──< order_items >── products
              orders ──< returns
```

| Relationship | Type | FK |
|---|---|---|
| customers → orders | One-to-Many | orders.customer_id |
| salespersons → orders | One-to-Many | orders.salesperson_id |
| orders → order_items | One-to-Many | order_items.order_id |
| products → order_items | One-to-Many | order_items.product_id |
| orders → returns | One-to-One | returns.order_id |

---

## Task 1 — CREATE Tables & INSERT Data

**Task:** Run all the CREATE TABLE and INSERT INTO statements above.

### Verify your setup:
```sql
-- Run each and confirm row counts
SELECT COUNT(*) FROM customers;       -- Expected: 10
SELECT COUNT(*) FROM products;        -- Expected: 10
SELECT COUNT(*) FROM salespersons;    -- Expected: 5
SELECT COUNT(*) FROM orders;          -- Expected: 20
SELECT COUNT(*) FROM order_items;     -- Expected: 26
SELECT COUNT(*) FROM returns;         -- Expected: 2

-- Verify relationships
SELECT o.order_id, c.customer_name, s.salesperson_name
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN salespersons s ON o.salesperson_id = s.salesperson_id
LIMIT 5;
```

---

## Task 2 — Basic SELECT Queries

### Questions
```sql
-- Q1. Show all customers with their city and tier (all columns)

-- Q2. Show only customer_name, email, tier — sorted alphabetically by name

-- Q3. Show all products with their profit margin
--     (profit = sell_price - cost_price)

-- Q4. List all distinct cities customers are from

-- Q5. Show all orders placed in January 2024

-- Q6. Display product_name and a column "price_label":
--     Use CASE WHEN: >50000 = 'Premium', >20000 = 'Mid-range', else 'Budget'

-- Q7. Show all salespersons sorted by monthly_target (highest first)

-- Q8. List all products where stock_qty is less than 60

-- Q9. Display order_id, total_amount, and a column "tax_amount" = 18% of total_amount

-- Q10. Show customers who registered in 2022 or 2023
```

---

## Task 3 — WHERE, AND, OR, IN, BETWEEN, LIKE

### Questions
```sql
-- Q1. Find all customers in Maharashtra or Tamil Nadu

-- Q2. Find all products with sell_price between ₹10,000 and ₹50,000

-- Q3. Find all orders that are either 'Pending' OR 'Shipped'

-- Q4. Find customers whose name starts with 'R' or ends with 'a'

-- Q5. Find products where brand is one of: Dell, Apple, Samsung (use IN)

-- Q6. Find all orders where total_amount > 50000 AND payment_mode = 'UPI'

-- Q7. Find customers whose email contains 'gmail'

-- Q8. Find orders placed between 2024-02-01 and 2024-02-28

-- Q9. Find products where cost_price is NOT between 1000 and 10000

-- Q10. Find all active customers (is_active = TRUE) who are NOT in Bronze tier
```

---

## Task 4 — Aggregate Functions & GROUP BY

### Questions
```sql
-- Q1. Total revenue from all delivered orders

-- Q2. Count of orders by status

-- Q3. Average order value by payment_mode

-- Q4. Total and average sell_price by product category

-- Q5. Which salesperson has the most orders? (count)

-- Q6. Monthly revenue trend: total amount per month (use MONTH/YEAR functions)

-- Q7. How many customers per tier?

-- Q8. Find categories where average sell_price > 20000
--     (use HAVING clause)

-- Q9. Total quantity sold per product (from order_items)

-- Q10. Salesperson performance: total orders, total revenue, avg order value
--      Only show salespersons with more than 3 orders
```

---

## Task 5 — JOINS (2 Tables)

### Questions
```sql
-- Q1. INNER JOIN: Show order_id, customer_name, total_amount for all orders

-- Q2. LEFT JOIN: Show ALL customers and their order count
--     (include customers with 0 orders)

-- Q3. INNER JOIN: Show order_id, product_name, quantity, unit_price from order_items

-- Q4. LEFT JOIN orders with returns — show which orders have been returned

-- Q5. INNER JOIN: Show customer_name, order_date, total_amount, salesperson_name
--     for all delivered orders

-- Q6. Find customers who have NEVER placed an order
--     (LEFT JOIN + WHERE order_id IS NULL)

-- Q7. Show product_name, brand, quantity sold — for all items in order_items

-- Q8. INNER JOIN orders with salespersons:
--     Show salesperson_name, region, order_id, total_amount
--     Sorted by total_amount descending

-- Q9. Show all products that have NEVER been ordered
--     (LEFT JOIN products with order_items, filter NULLs)

-- Q10. Find the most expensive order per customer
--      (JOIN customers + orders, GROUP BY customer)
```

---

## Task 6 — Multi-Table JOINs (4–6 Tables)

> **This is the most important task for BA interviews!**

### Questions
```sql
-- Q1. JOIN 4 tables: orders + customers + salespersons + order_items
--     Show: customer_name, salesperson_name, region, order_date, product count per order

-- Q2. JOIN 5 tables: Add products to Q1
--     Show: customer_name, product_name, quantity, unit_price, salesperson_name, region

-- Q3. Full picture query (all 6 tables):
--     Show returned orders with:
--     customer_name, product_name, salesperson_name, region,
--     return_reason, refund_amount, return_status

-- Q4. Revenue by region (JOIN orders + salespersons + order_items + products):
--     Region | Total Orders | Total Revenue | Avg Order Value

-- Q5. Customer purchase history (JOIN 4 tables):
--     customer_name | tier | product_name | category | order_date | amount_paid
--     For Platinum and Gold customers only

-- Q6. Salesperson performance report (JOIN 3 tables):
--     salesperson_name | region | orders_count | total_revenue
--     | target | achievement_pct (revenue/target*100)
--     ORDER BY achievement_pct DESC

-- Q7. Product profit report (JOIN order_items + products + orders):
--     product_name | total_qty_sold | total_revenue | total_cost
--     | total_profit | profit_margin_pct
--     For DELIVERED orders only

-- Q8. City-wise sales (JOIN customers + orders + order_items):
--     city | state | total_orders | total_revenue | top_product

-- Q9. Monthly salesperson leaderboard (JOIN 3 tables):
--     Month | Rank | salesperson_name | region | total_revenue

-- Q10. Complete order invoice view (all 6 tables):
--      order_id | customer_name | city | salesperson_name | product_name
--      | quantity | unit_price | discount_pct | line_total | order_status
```

### Hint for Multi-Table JOINs
```sql
-- Pattern for 5-table join
SELECT
    c.customer_name,
    s.salesperson_name,
    s.region,
    p.product_name,
    oi.quantity,
    oi.unit_price,
    o.order_date,
    o.status
FROM orders o
JOIN customers c      ON o.customer_id = c.customer_id
JOIN salespersons s   ON o.salesperson_id = s.salesperson_id
JOIN order_items oi   ON o.order_id = oi.order_id
JOIN products p       ON oi.product_id = p.product_id
WHERE o.status = 'Delivered'
ORDER BY o.order_date DESC;
```

---

## Task 7 — Subqueries

### Questions
```sql
-- Q1. Find customers who have placed orders with total_amount > average order value
--     (subquery in WHERE)

-- Q2. Show products that cost more than the average sell_price in their category

-- Q3. Find the customer who spent the most money overall

-- Q4. List salespersons whose total revenue > the company average revenue per salesperson

-- Q5. Find all orders where total_amount is in the top 5 highest

-- Q6. Show customers who have ordered EVERY product in the Electronics category
--     (correlated subquery or EXISTS)

-- Q7. Find products that were ordered in January but NOT in February
--     (use NOT IN with subquery)

-- Q8. For each order, show if total_amount is above/below/equal to
--     that customer's average spend (correlated subquery)

-- Q9. Find the 2nd highest total_amount order (no LIMIT — use subquery)

-- Q10. List customers whose last order was more than 60 days ago
```

---

## Task 8 — Window Functions

> **Window functions are interview favorites — practice these!**

### Questions
```sql
-- Q1. Rank orders by total_amount (RANK vs DENSE_RANK — understand the difference)

-- Q2. ROW_NUMBER() — Assign row numbers to orders sorted by date

-- Q3. For each customer, rank their orders by amount (PARTITION BY customer_id)

-- Q4. RUNNING TOTAL of revenue by order_date (SUM OVER ORDER BY date)

-- Q5. LAG() — Show each order's amount AND the previous order's amount (same customer)

-- Q6. LEAD() — Show current and next order date gap per customer

-- Q7. NTILE(4) — Divide orders into 4 quartiles by amount
--     Label them: Q1 (lowest 25%) to Q4 (top 25%)

-- Q8. For each salesperson, show:
--     order_id | order_amount | salesperson_total | % contribution to total

-- Q9. FIRST_VALUE / LAST_VALUE — For each region, show
--     the first and last order placed (by date)

-- Q10. Moving average: 3-order rolling average of total_amount (by order_date)
```

### Hint
```sql
-- Ranking
SELECT order_id, total_amount,
       RANK() OVER (ORDER BY total_amount DESC) as rank_by_amount,
       DENSE_RANK() OVER (ORDER BY total_amount DESC) as dense_rank
FROM orders;

-- Partition by customer
SELECT order_id, customer_id, total_amount,
       RANK() OVER (PARTITION BY customer_id ORDER BY total_amount DESC) as rank_in_customer
FROM orders;

-- Running total
SELECT order_date, total_amount,
       SUM(total_amount) OVER (ORDER BY order_date) as running_total
FROM orders;
```

---

## Task 9 — CTEs & Views

### Questions
```sql
-- Q1. CTE: Calculate total revenue per customer, then filter top 5
WITH customer_revenue AS (
    SELECT customer_id, SUM(total_amount) as total_spent
    FROM orders
    WHERE status = 'Delivered'
    GROUP BY customer_id
)
-- Write the outer query here

-- Q2. CTE: Find salesperson monthly achievement vs target

-- Q3. Multiple CTEs: (a) revenue by product, (b) cost by product
--     Join them to find profit per product

-- Q4. Recursive CTE (bonus): Generate a date series from 2024-01-01 to 2024-04-30

-- Q5. CREATE VIEW "v_order_summary":
--     order_id, customer_name, salesperson_name, region,
--     product_name, total_amount, status

-- Q6. CREATE VIEW "v_salesperson_kpi":
--     salesperson_name | region | total_orders | total_revenue
--     | target | achievement_pct

-- Q7. CREATE VIEW "v_customer_360":
--     customer_id | customer_name | tier | city | total_orders
--     | total_spent | last_order_date | days_since_last_order

-- Q8. Use your view v_order_summary to:
--     Find all Platinum customers who ordered Electronics

-- Q9. CTE + Window Function:
--     Rank products by revenue within each category using a CTE

-- Q10. Update your v_salesperson_kpi view to also include
--      number of returned orders per salesperson
```

---

## Task 10 — Complex Business Queries

> **These mimic actual BA interview questions from product/tech companies.**

### Questions
```sql
-- Q1. RETENTION: Which customers placed orders in BOTH January and February 2024?

-- Q2. CHURN: Find customers who ordered in Jan but NOT in Feb or Mar
--     (potential churned customers)

-- Q3. COHORT: Group customers by their registration year,
--     show average lifetime spend per cohort

-- Q4. FUNNEL: Show count of orders at each stage:
--     Pending → Shipped → Delivered (and Cancelled/Returned)

-- Q5. TOP N PER GROUP: Show TOP 2 best-selling products per category
--     (by revenue) — use window functions

-- Q6. YEAR-OVER-YEAR: Monthly revenue with:
--     current month revenue | prev month revenue | MoM growth %

-- Q7. BASKET ANALYSIS: Which 2 products are most often bought in the same order?
--     (self-join order_items)

-- Q8. CUSTOMER LIFETIME VALUE:
--     customer_name | total_orders | total_spent | avg_order_value
--     | months_since_first_order | monthly_avg_spend

-- Q9. DEAD STOCK: Find products not ordered in the last 30 days
--     with stock_qty > 50 (potential overstock)

-- Q10. EXECUTIVE SUMMARY QUERY:
--      In ONE query using CTEs, produce:
--      Total Revenue | Total Orders | Avg Order Value |
--      Top Customer | Top Product | Top Region |
--      Delivery Rate (% delivered) | Return Rate (% returned)
```

---

## 📚 Resources

| Resource | Link |
|---|---|
| Practice SQL Online | [https://sqliteonline.com](https://sqliteonline.com) |
| LeetCode SQL | [https://leetcode.com/studyplan/top-sql-50](https://leetcode.com/studyplan/top-sql-50) |
| HackerRank SQL | [https://www.hackerrank.com/domains/sql](https://www.hackerrank.com/domains/sql) |
| Mode SQL Tutorial | [https://mode.com/sql-tutorial](https://mode.com/sql-tutorial) |
| W3Schools SQL | [https://www.w3schools.com/sql](https://www.w3schools.com/sql) |

---

*Update your status column daily. Mark ✅ when done, 🔄 when in progress.*
