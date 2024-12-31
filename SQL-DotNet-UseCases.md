# Real-World SQL Example Scenarios
### By: [Partha Sarathi Kundu](https://www.linkedin.com/in/partha-sarathi-kundu/recent-activity/articles/)

These scenarios are tailored for learning SQL when used with **C#**, **.NET Framework**, and a **RESTful API backend**. Each scenario includes an explanation of how it could be implemented in a real-world project.

---

### 1. **User Authentication: Retrieve User Details by Email**
#### Scenario:
You need to fetch user details from the database to authenticate a user logging into your RESTful API.

#### Table: `users`
| user_id | email             | password_hash           | created_at |
|---------|-------------------|-------------------------|------------|
| 1       | john@example.com  | 5f4dcc3b5aa765d61d8327  | 2024-01-10 |
| 2       | jane@example.com  | a1b2c3d4e5f67890abcd12  | 2023-12-05 |

#### Query:
```sql
SELECT 
    user_id, 
    email, 
    password_hash
FROM 
    users
WHERE 
    email = @Email;
```

#### Explanation:
- **Purpose**: Fetches user details by `email` for login validation.
- **`@Email`**: A parameter passed securely from the C# application to avoid SQL injection.
- **Implementation**: Use this query within your `.NET Framework` backend with libraries like `SqlClient` or `Entity Framework`.

---

### 2. **Fetch Paginated Data for RESTful API**
#### Scenario:
Retrieve a paginated list of products for display on the frontend.

#### Table: `products`
| product_id | product_name      | price | created_at |
|------------|-------------------|-------|------------|
| 1          | Phone Case        | 9.99  | 2023-11-01 |
| 2          | Wireless Charger  | 29.99 | 2023-11-10 |
| 3          | Laptop Stand      | 39.99 | 2023-12-01 |
| 4          | USB Hub           | 14.99 | 2023-12-05 |

#### Query:
```sql
SELECT 
    product_id, 
    product_name, 
    price
FROM 
    products
ORDER BY 
    created_at DESC
OFFSET @PageSize * (@PageNumber - 1) ROWS
FETCH NEXT @PageSize ROWS ONLY;
```

#### Explanation:
- **Purpose**: Implements pagination for large datasets.
- **Parameters**: `@PageSize` and `@PageNumber` control the number of results and the starting point.
- **Usage**: Use this query in a REST API for endpoints like `/api/products?page=1&size=10`.

---

### 3. **Insert Data After Validating Relationships**
#### Scenario:
A user submits a new order, and you need to insert it into the database.

#### Tables:
1. **`orders`**
   | order_id | user_id | order_date  | total_amount |
   |----------|---------|-------------|--------------|
   | 1        | 1       | 2024-01-15  | 99.99        |

2. **`order_items`**
   | item_id | order_id | product_id | quantity | price |
   |---------|----------|------------|----------|-------|
   | 1       | 1        | 1          | 2        | 9.99  |
   | 2       | 1        | 2          | 1        | 29.99 |

#### Query:
```sql
INSERT INTO orders (user_id, order_date, total_amount)
VALUES (@UserId, GETDATE(), @TotalAmount);

INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES (@OrderId, @ProductId, @Quantity, @Price);
```

#### Explanation:
- **First Query**: Inserts a new order into the `orders` table.
- **Second Query**: Inserts items related to the order into `order_items`.
- **Usage**: Use `SqlTransaction` in C# to ensure both inserts succeed or rollback.

---

### 4. **Update Inventory After Order Placement**
#### Scenario:
When an order is placed, reduce the inventory for each product in the order.

#### Table: `inventory`
| product_id | stock_quantity |
|------------|----------------|
| 1          | 100            |
| 2          | 50             |
| 3          | 200            |

#### Query:
```sql
UPDATE inventory
SET stock_quantity = stock_quantity - @Quantity
WHERE product_id = @ProductId;
```

#### Explanation:
- **Purpose**: Deducts `@Quantity` from the current stock for a product.
- **Usage**: Call this query after processing each item in the order.

---

### 5. **Fetch User Orders with Items**
#### Scenario:
Return a list of orders and their items for a specific user.

#### Tables:
1. **`orders`**
   | order_id | user_id | order_date  |
   |----------|---------|-------------|
   | 1        | 1       | 2024-01-15  |

2. **`order_items`**
   | item_id | order_id | product_id | quantity | price |
   |---------|----------|------------|----------|-------|
   | 1       | 1        | 1          | 2        | 9.99  |
   | 2       | 1        | 2          | 1        | 29.99 |

#### Query:
```sql
SELECT 
    o.order_id,
    o.order_date,
    oi.product_id,
    oi.quantity,
    oi.price
FROM 
    orders o
JOIN 
    order_items oi ON o.order_id = oi.order_id
WHERE 
    o.user_id = @UserId;
```

#### Explanation:
- **Purpose**: Retrieves a user’s order history.
- **Usage**: This query is ideal for endpoints like `/api/users/{id}/orders`.

---

### 6. **Search Products by Keyword**
#### Scenario:
Allow users to search for products using keywords.

#### Query:
```sql
SELECT 
    product_id,
    product_name,
    price
FROM 
    products
WHERE 
    product_name LIKE '%' + @Keyword + '%';
```

#### Explanation:
- **Purpose**: Performs a partial match search on the product name.
- **Usage**: Use this query for search endpoints like `/api/products?search=case`.

---

### 7. **Aggregate Data for Analytics**
#### Scenario:
Calculate the total revenue generated in the past month.

#### Query:
```sql
SELECT 
    SUM(oi.quantity * oi.price) AS total_revenue
FROM 
    orders o
JOIN 
    order_items oi ON o.order_id = oi.order_id
WHERE 
    o.order_date >= DATEADD(MONTH, -1, GETDATE());
```

#### Explanation:
- **Purpose**: Aggregates revenue data for analytics.
- **Usage**: Use this for admin dashboards in your API.

---

### 8. **Monitor Failed Login Attempts**
#### Scenario:
Log failed login attempts and fetch recent failures.

#### Table: `login_attempts`
| attempt_id | user_id | attempt_time        | success |
|------------|---------|---------------------|---------|
| 1          | 1       | 2024-01-01 08:00:00| 0       |
| 2          | 1       | 2024-01-01 08:05:00| 0       |
| 3          | 2       | 2024-01-01 09:00:00| 1       |

#### Query:
```sql
SELECT 
    user_id, 
    COUNT(*) AS failed_attempts
FROM 
    login_attempts
WHERE 
    success = 0 AND attempt_time >= DATEADD(DAY, -7, GETDATE())
GROUP BY 
    user_id;
```

#### Explanation:
- **Purpose**: Monitors failed logins over the past 7 days.
- **Usage**: Use this for security reports in your RESTful API.

---

### 9. **Soft Delete for RESTful API**
#### Scenario:
Mark a product as deleted without removing it from the database.

#### Table: `products`
| product_id | product_name | is_deleted |
|------------|--------------|------------|
| 1          | Phone Case   | 0          |
| 2          | USB Hub      | 0          |

#### Query:
```sql
UPDATE products
SET is_deleted = 1
WHERE product_id = @ProductId;
```

#### Explanation:
- **Purpose**: Updates the `is_deleted` flag instead of deleting the product.
- **Usage**: Use this for endpoints like `DELETE /api/products/{id}`.

---

### 10. **Complex Filtering for Products**
#### Scenario:
Fetch products based on price range, category, and availability.

#### Table: `products`
| product_id | product_name    | price | category_id | is_deleted |
|------------|-----------------|-------|-------------|------------|
| 1          | Phone Case      | 9.99  | 1           | 0          |
| 2          | Wireless Charger| 29.99 | 1           | 0          |
| 3          | Laptop Stand    | 39.99 | 2           | 0          |
| 4          | USB Hub         | 14.99 | 2           | 1          |

#### Query:
```sql
SELECT 
    product_id,
    product_name,
    price
FROM 
    products
WHERE 
    price BETWEEN @MinPrice AND @MaxPrice
    AND category_id = @CategoryId
    AND is_deleted = 0;
```

#### Explanation:
- **Purpose**: Applies multiple filters to return products meeting all criteria.
- **Usage**: Use this query for advanced search endpoints like `/api/products?minPrice=10&maxPrice=50&category=1`.

---

These scenarios illustrate practical uses of SQL in projects involving **C#**, **.NET Framework**, and **RESTful APIs**.
