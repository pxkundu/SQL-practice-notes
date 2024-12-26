
---

### 6. **A travel agency is promoting activities for a "Summer Christmas" party. They want to identify the top 2 activities based on the average rating. Write a query to rank the activities by average rating.**

#### Table: `activities`
| activity_id | activity_name      |
|-------------|--------------------|
| 1           | Surfing Lessons    |
| 2           | Jet Skiing         |
| 3           | Sunset Yoga        |

#### Table: `activity_ratings`
| rating_id | activity_id | rating |
|-----------|-------------|--------|
| 1         | 1           | 4.7    |
| 2         | 1           | 4.8    |
| 3         | 1           | 4.9    |
| 4         | 2           | 4.6    |
| 5         | 2           | 4.7    |
| 6         | 2           | 4.8    |
| 7         | 2           | 4.9    |
| 8         | 3           | 4.8    |
| 9         | 3           | 4.7    |
| 10        | 3           | 4.9    |
| 11        | 3           | 4.8    |
| 12        | 3           | 4.9    |

#### Solution:
```sql
WITH ActivityAverages AS (
    SELECT 
        a.activity_name,
        AVG(r.rating) AS average_rating
    FROM 
        activities a
    JOIN 
        activity_ratings r ON a.activity_id = r.activity_id
    GROUP BY 
        a.activity_name
)
SELECT 
    activity_name,
    average_rating
FROM 
    ActivityAverages
ORDER BY 
    average_rating DESC
LIMIT 2;
```

#### Explanation:
1. **`WITH ActivityAverages`**:
   - Joins `activities` and `activity_ratings` to calculate the average rating for each activity.
   - Groups by `activity_name` to compute the average rating per activity.
2. **Main Query**:
   - Sorts activities by their average rating in descending order.
   - Limits the results to the top 2 activities.

#### Output:
| activity_name   | average_rating |
|-----------------|----------------|
| Sunset Yoga     | 4.82           |
| Jet Skiing      | 4.75           |

---

### 7. **Santa needs to optimize his sleigh for Christmas deliveries. Write a query to calculate the total weight of gifts for each recipient type (good or naughty) and determine what percentage of the total weight is allocated to each type. Include the recipient_type, total_weight, and weight_percentage in the result.**

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
WITH TotalWeights AS (
    SELECT 
        recipient_type,
        SUM(weight_kg) AS total_weight
    FROM 
        gifts
    GROUP BY 
        recipient_type
)
SELECT 
    recipient_type,
    total_weight,
    (total_weight * 100.0 / SUM(total_weight) OVER ()) AS weight_percentage
FROM 
    TotalWeights
ORDER BY 
    total_weight DESC;
```

#### Explanation:
1. **`WITH TotalWeights`**:
   - Groups the `gifts` table by `recipient_type` and calculates the total weight of gifts for each type.
2. **Main Query**:
   - Computes the weight percentage for each type using the total weight and the `SUM` window function.
   - Sorts results by total weight in descending order.

#### Output:
| recipient_type | total_weight | weight_percentage |
|----------------|--------------|--------------------|
| good           | 4.0          | 69.57             |
| naughty        | 3.3          | 30.43             |

---

### 8. **We are hosting a gift party and need to ensure every guest receives a gift. Using the guests and guest_gifts tables, write a query to identify the guest(s) who have not been assigned a gift (i.e., they are not listed in the guest_gifts table).**

#### Table: `guests`
| guest_id | guest_name        |
|----------|-------------------|
| 1        | Cindy Lou         |
| 2        | The Grinch        |
| 3        | Max the Dog       |
| 4        | Mayor May Who     |

#### Table: `guest_gifts`
| gift_id | guest_id | gift_name      |
|---------|----------|----------------|
| 1       | 1        | Toy Train      |
| 2       | 1        | Plush Bear     |
| 3       | 2        | Bag of Coal    |
| 4       | 2        | Sleigh Bell    |
| 5       | 3        | Dog Treats     |

#### Solution:
```sql
SELECT 
    g.guest_name
FROM 
    guests g
LEFT JOIN 
    guest_gifts gg ON g.guest_id = gg.guest_id
WHERE 
    gg.guest_id IS NULL;
```

#### Explanation:
1. **`LEFT JOIN`**:
   - Joins the `guests` table with the `guest_gifts` table to include all guests, even those without a matching record in `guest_gifts`.
2. **`WHERE gg.guest_id IS NULL`**:
   - Filters for guests who do not have a corresponding entry in `guest_gifts`, indicating they have not been assigned a gift.

#### Output:
| guest_name     |
|----------------|
| Mayor May Who  |

---

[Go for next set of solutions](/SQL-9-11.md)