# Holiday-Themed SQL Questions and Answers

---

### 1. **Santa wants to know which gifts weigh more than 1 kg. Can you list them?**

#### Table: `gifts`
| gift_id | gift_name      | recipient_type | weight_kg |
|---------|----------------|----------------|-----------|
| 1       | Toy Train      | good           | 2.5       |
| 2       | Chocolate Box  | good           | 0.8       |
| 3       | Teddy Bear     | good           | 1.2       |
| 4       | Board Game     | naughty        | 0.9       |

#### Solution:
```sql
SELECT 
    gift_name, 
    weight_kg
FROM 
    gifts
WHERE 
    weight_kg > 1;
```

#### Explanation:
- Filters the `gifts` table to include only rows where `weight_kg > 1`.
- Returns the name and weight of gifts that meet the condition.

#### Output:
| gift_name     | weight_kg |
|---------------|-----------|
| Toy Train     | 2.5       |
| Teddy Bear    | 1.2       |

---

### 2. **Scientists are tracking polar bears across the Arctic to monitor their migration patterns and caloric intake. Write a query to find the top 3 polar bears that have traveled the longest total distance in December 2024. Include their bear_id, bear_name, and total_distance_traveled in the results.**

#### Table: `polar_bears`
| bear_id | bear_name | age |
|---------|-----------|-----|
| 1       | Snowball  | 10  |
| 2       | Frosty    | 7   |
| 3       | Iceberg   | 15  |

#### Table: `tracking`
| tracking_id | bear_id | distance_km | date       |
|-------------|---------|-------------|------------|
| 1           | 1       | 25          | 2024-12-01 |
| 2           | 2       | 40          | 2024-12-02 |
| 3           | 1       | 30          | 2024-12-03 |
| 4           | 3       | 50          | 2024-12-04 |
| 5           | 2       | 35          | 2024-12-05 |
| 6           | 3       | 55          | 2024-12-07 |

#### Solution:
```sql
WITH BearDistances AS (
    SELECT 
        pb.bear_id,
        pb.bear_name,
        SUM(t.distance_km) AS total_distance
    FROM 
        polar_bears pb
    JOIN 
        tracking t ON pb.bear_id = t.bear_id
    WHERE 
        t.date BETWEEN '2024-12-01' AND '2024-12-31'
    GROUP BY 
        pb.bear_id, pb.bear_name
)
SELECT 
    bear_id,
    bear_name,
    total_distance
FROM 
    BearDistances
ORDER BY 
    total_distance DESC
LIMIT 3;
```

#### Explanation:
1. **`WITH BearDistances`**:
   - Joins `polar_bears` and `tracking` tables to calculate the total distance traveled for each bear in December 2024.
   - Filters the tracking data to include only dates within December.
   - Groups by `bear_id` and `bear_name` to compute the total distance.
2. **Main Query**:
   - Selects the top 3 bears with the highest `total_distance`.

#### Output:
| bear_id | bear_name | total_distance |
|---------|-----------|----------------|
| 3       | Iceberg   | 105            |
| 1       | Snowball  | 55             |
| 2       | Frosty    | 75             |

---

### 3. **The owner of a winter market wants to know which vendors have generated the highest revenue overall. For each vendor, calculate the total revenue for all their items and return a list of the top 2 vendors by total revenue. Include the vendor_name and total_revenue in your results.**

#### Table: `vendors`
| vendor_id | vendor_name      | market_location  |
|-----------|------------------|------------------|
| 1         | Cozy Crafts      | Downtown Square  |
| 2         | Sweet Treats     | Central Park     |
| 3         | Winter Warmers   | Downtown Square  |

#### Table: `sales`
| sale_id | vendor_id | item_name            | quantity_sold | price_per_unit |
|---------|-----------|----------------------|---------------|----------------|
| 1       | 1         | Knitted Scarf       | 15            | 25             |
| 2       | 2         | Hot Chocolate       | 50            | 3.5            |
| 3       | 3         | Wool Hat            | 20            | 18             |
| 4       | 1         | Handmade Ornament   | 10            | 15             |
| 5       | 2         | Gingerbread Cookie  | 30            | 5              |

#### Solution:
```sql
WITH VendorRevenue AS (
    SELECT 
        v.vendor_name,
        SUM(s.quantity_sold * s.price_per_unit) AS total_revenue
    FROM 
        vendors v
    JOIN 
        sales s ON v.vendor_id = s.vendor_id
    GROUP BY 
        v.vendor_name
)
SELECT 
    vendor_name,
    total_revenue
FROM 
    VendorRevenue
ORDER BY 
    total_revenue DESC
LIMIT 2;
```

#### Explanation:
1. **`WITH VendorRevenue`**:
   - Calculates total revenue for each vendor by summing up the product of `quantity_sold` and `price_per_unit`.
   - Groups by `vendor_name` to compute revenue for each vendor.
2. **Main Query**:
   - Selects the top 2 vendors with the highest `total_revenue`.

#### Output:
| vendor_name     | total_revenue |
|-----------------|---------------|
| Cozy Crafts     | 525           |
| Sweet Treats    | 335           |

---

### 4. **You are managing inventory in Santa's workshop. Which gifts are meant for "good" recipients? List the gift name and its weight.**

#### Table: `gifts`
| gift_id | gift_name      | recipient_type | weight_kg |
|---------|----------------|----------------|-----------|
| 1       | Toy Train      | good           | 2.5       |
| 2       | Lumps of Coal  | naughty        | 1.5       |
| 3       | Teddy Bear     | good           | 1.2       |
| 4       | Chocolate Bar  | good           | 0.3       |
| 5       | Board Game     | naughty        | 1.8       |

#### Solution:
```sql
SELECT 
    gift_name, 
    weight_kg
FROM 
    gifts
WHERE 
    recipient_type = 'good';
```

#### Explanation:
- Filters the `gifts` table to include only rows where `recipient_type = 'good'`.
- Returns the name and weight of the gifts meant for "good" recipients.

#### Output:
| gift_name      | weight_kg |
|----------------|-----------|
| Toy Train      | 2.5       |
| Teddy Bear     | 1.2       |
| Chocolate Bar  | 0.3       |

---

### 5. **A collector wants to identify the top 3 snow globes with the highest number of figurines. Write a query to rank them and include their globe_name, number of figurines, and material.**

#### Table: `snow_globes`
| globe_id | globe_name         | volume_cm3 | material |
|----------|--------------------|------------|----------|
| 1        | Winter Wonderland  | 500        | Glass    |
| 2        | Santas Workshop    | 300        | Plastic  |
| 3        | Frozen Forest      | 400        | Glass    |
| 4        | Holiday Village    | 600        | Glass    |

#### Table: `figurines`
| figurine_id | globe_id | figurine_type |
|-------------|----------|---------------|
| 1           | 1        | Snowman       |
| 2           | 1        | Tree          |
| 3           | 2        | Santa Claus   |
| 4           | 2        | Elf


---

#### Solution:
```sql
WITH GlobeFigures AS (
    SELECT 
        sg.globe_name,
        sg.material,
        COUNT(f.figurine_id) AS figurine_count
    FROM 
        snow_globes sg
    LEFT JOIN 
        figurines f ON sg.globe_id = f.globe_id
    GROUP BY 
        sg.globe_name, sg.material
)
SELECT 
    globe_name,
    material,
    figurine_count
FROM 
    GlobeFigures
ORDER BY 
    figurine_count DESC
LIMIT 3;
```

#### Explanation:
1. **`WITH GlobeFigures`**:
   - Joins `snow_globes` and `figurines` to count the number of figurines for each globe.
   - Groups the results by `globe_name` and `material` to calculate the figurine count for each globe.
2. **Main Query**:
   - Sorts globes by the number of figurines in descending order.
   - Limits the results to the top 3.

#### Output:
| globe_name         | material | figurine_count |
|--------------------|----------|----------------|
| Holiday Village    | Glass    | 5              |
| Santas Workshop    | Plastic  | 3              |
| Winter Wonderland  | Glass    | 2              |

---

[Go for next set of solutions](/SQL-6-8.md)