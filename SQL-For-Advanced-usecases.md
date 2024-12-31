# 10 Advanced SQL Use Cases for WMS/MCS Systems
### By: [Partha Sarathi Kundu](https://www.linkedin.com/in/partha-sarathi-kundu/recent-activity/articles/)

These use cases involve IoT sensors, HMI, and Ignition scripts, showcasing complex and advanced SQL logic for real-world scenarios in warehouse management and material control systems.

---

### 1. **Identify Unused Storage Bins for the Last 30 Days**
#### Scenario:
Find storage bins that haven’t been used for storing or removing inventory in the last 30 days.

#### Tables:
**`storage_bins`**
| bin_id | bin_name |
|--------|----------|
| 1      | Bin A    |
| 2      | Bin B    |
| 3      | Bin C    |

**`material_movements`**
| movement_id | material_id | from_bin | to_bin | moved_at           |
|-------------|-------------|----------|--------|--------------------|
| 1           | 101         | 1        | 2      | 2024-01-01 12:00  |
| 2           | 102         | 2        | 3      | 2023-12-01 09:00  |
| 3           | 103         | 1        | 2      | 2024-01-02 14:00  |

#### Query:
```sql
SELECT 
    sb.bin_id
FROM 
    storage_bins sb
LEFT JOIN 
    material_movements mm 
    ON sb.bin_id = mm.from_bin OR sb.bin_id = mm.to_bin
WHERE 
    (mm.moved_at IS NULL OR mm.moved_at < DATEADD(DAY, -30, GETDATE()))
GROUP BY 
    sb.bin_id
HAVING 
    COUNT(mm.movement_id) = 0;
```

#### Explanation:
- Uses `LEFT JOIN` to find bins with no movements in the last 30 days.
- Groups bins and filters them using `HAVING` to ensure zero movements.

---

### 2. **Calculate Average Conveyor Load Over Time Intervals**
#### Scenario:
Monitor conveyor belt performance by calculating average load over 10-minute intervals.

#### Table: `conveyor_loads`
| belt_id | timestamp           | load_kg |
|---------|---------------------|---------|
| 1       | 2024-01-01 08:00:00 | 200     |
| 1       | 2024-01-01 08:05:00 | 250     |
| 1       | 2024-01-01 08:15:00 | 150     |
| 2       | 2024-01-01 08:20:00 | 300     |

#### Query:
```sql
SELECT 
    belt_id,
    DATEADD(MINUTE, (DATEDIFF(MINUTE, 0, timestamp) / 10) * 10, 0) AS time_interval,
    AVG(load_kg) AS avg_load
FROM 
    conveyor_loads
GROUP BY 
    belt_id, 
    DATEADD(MINUTE, (DATEDIFF(MINUTE, 0, timestamp) / 10) * 10, 0)
ORDER BY 
    belt_id, time_interval;
```

#### Explanation:
- Groups conveyor data into 10-minute time intervals using `DATEDIFF` and `DATEADD`.
- Calculates average load for each interval.

---

### 3. **Detect Sensor Anomalies Based on Historical Patterns**
#### Scenario:
Flag sensors reporting values outside their typical range.

#### Table: `sensor_readings`
| sensor_id | timestamp           | temperature |
|-----------|---------------------|-------------|
| 1         | 2024-01-01 08:00:00 | 25          |
| 1         | 2024-01-01 09:00:00 | 27          |
| 1         | 2024-01-01 10:00:00 | 15 (anomaly)|

#### Query:
```sql
WITH SensorStats AS (
    SELECT 
        sensor_id,
        AVG(temperature) AS avg_temp,
        STDEV(temperature) AS std_temp
    FROM 
        sensor_readings
    WHERE 
        timestamp >= DATEADD(DAY, -30, GETDATE())
    GROUP BY 
        sensor_id
)
SELECT 
    sr.sensor_id,
    sr.timestamp,
    sr.temperature
FROM 
    sensor_readings sr
JOIN 
    SensorStats ss 
    ON sr.sensor_id = ss.sensor_id
WHERE 
    sr.temperature < (ss.avg_temp - 2 * ss.std_temp) 
    OR sr.temperature > (ss.avg_temp + 2 * ss.std_temp);
```

#### Explanation:
- Computes historical average and standard deviation for each sensor.
- Flags readings outside 2 standard deviations from the mean.

---

### 4. **Analyze Material Flow Between Zones**
#### Scenario:
Track the flow of materials between warehouse zones and compute total quantities moved.

#### Table: `zone_movements`
| movement_id | from_zone | to_zone | material_id | quantity |
|-------------|-----------|---------|-------------|----------|
| 1           | Zone A    | Zone B  | 101         | 50       |
| 2           | Zone B    | Zone C  | 102         | 70       |
| 3           | Zone A    | Zone C  | 103         | 40       |

#### Query:
```sql
SELECT 
    from_zone,
    to_zone,
    SUM(quantity) AS total_quantity
FROM 
    zone_movements
GROUP BY 
    from_zone, to_zone
HAVING 
    total_quantity > 0
ORDER BY 
    total_quantity DESC;
```

#### Explanation:
- Aggregates material movements between zones and sorts by total quantity.
- Filters only zones with significant flow (`HAVING total_quantity > 0`).

---

### 5. **Monitor Downtime for Equipment**
#### Scenario:
Calculate downtime for each piece of equipment based on log gaps.

#### Table: `equipment_logs`
| equipment_id | timestamp           |
|--------------|---------------------|
| EQ1          | 2024-01-01 08:00:00|
| EQ1          | 2024-01-01 09:00:00|
| EQ1          | 2024-01-01 12:00:00|

#### Query:
```sql
WITH LogGaps AS (
    SELECT 
        equipment_id,
        LAG(timestamp) OVER (PARTITION BY equipment_id ORDER BY timestamp) AS prev_time,
        timestamp AS curr_time,
        DATEDIFF(MINUTE, LAG(timestamp) OVER (PARTITION BY equipment_id ORDER BY timestamp), timestamp) AS downtime_minutes
    FROM 
        equipment_logs
)
SELECT 
    equipment_id,
    SUM(downtime_minutes) AS total_downtime
FROM 
    LogGaps
WHERE 
    downtime_minutes > 5
GROUP BY 
    equipment_id;
```

#### Explanation:
- Uses `LAG` to calculate time gaps between consecutive logs.
- Aggregates downtime exceeding 5 minutes for each equipment.

---

### 6. **Track Expiring Inventory Items**
#### Scenario:
Find inventory items expiring within the next 7 days, grouped by their storage location.

#### Table: `inventory_items`
| item_id | bin_id | expiry_date |
|---------|--------|------------|
| 1       | 1      | 2024-01-05 |
| 2       | 2      | 2024-01-03 |
| 3       | 3      | 2024-01-10 |

#### Query:
```sql
SELECT 
    bin_id,
    COUNT(item_id) AS expiring_items
FROM 
    inventory_items
WHERE 
    expiry_date BETWEEN GETDATE() AND DATEADD(DAY, 7, GETDATE())
GROUP BY 
    bin_id;
```

#### Explanation:
- Counts items in each bin that are set to expire within 7 days.
- Helps prevent inventory wastage.

---

### 7. **Track the Fastest-Moving Materials**
#### Scenario:
Identify materials that had the highest number of movements in the last 30 days.

#### Table: `material_movements`
| movement_id | material_id | moved_at           |
|-------------|-------------|--------------------|
| 1           | 101         | 2024-01-01 12:00  |
| 2           | 101         | 2024-01-02 14:00  |
| 3           | 102         | 2024-01-01 13:00  |

#### Query:
```sql
SELECT 
    material_id,
    COUNT(movement_id) AS movement_count
FROM 
    material_movements
WHERE 
    moved_at >= DATEADD(DAY, -30, GETDATE())
GROUP BY 
    material_id
ORDER BY 
    movement_count DESC
LIMIT 5;
```

#### Explanation:
- Counts material movements and sorts by the number of movements.
- Highlights the top 5 most frequently moved materials.

---

### 8. **Find Overloaded Storage Bins**
#### Scenario:
List bins where the total weight exceeds 90% of the bin's capacity.

#### Table: `storage_bins`
| bin_id | max_capacity | current_load |
|--------|--------------|--------------|
| A1     | 1000         | 950          |
| A2     | 800          | 600          |
| A3     | 500          | 450          |

#### Query:
```sql
SELECT 
    bin_id,
    current_load,
    max_capacity,
    (current_load * 100.0 / max_capacity) AS load_percentage
FROM 
    storage_bins
WHERE 
    current_load > (max_capacity * 0.9);
```

#### Explanation:
- Calculates and filters bins exceeding 90% of their capacity.

---

### 9. **Analyze Sensor Failures by Location**
#### Scenario:
Group and count sensor failures by location and sensor type.

#### Tables:
**`sensor_logs`**
| log_id | sensor_id | error_code |
|--------|-----------|------------|
| 1      | 1         | ERR01      |
| 2      | 2         | ERR02      |

**`sensors`**
| sensor_id | location      | sensor_type |
|-----------|---------------|-------------|
| 1         | Zone A        | Temperature |
| 2         | Zone B        | Humidity    |

#### Query:
```sql
SELECT 
    s.location,
    s.sensor_type,
    COUNT(sl.error_code) AS failure_count
FROM 
    sensors s
JOIN 
    sensor_logs sl ON s.sensor_id = sl.sensor_id
WHERE 
    sl.error_code IS NOT NULL
GROUP BY 
    s.location, s.sensor_type
ORDER BY 
    failure_count DESC;
```

#### Explanation:
- Joins sensor metadata with error logs to analyze failure trends.

---

### 10. **Calculate Inventory Turnover Rate**
#### Scenario:
Determine how frequently materials are moving in and out of the warehouse.

#### Table: `material_movements`
| movement_id | material_id | movement_type | quantity |
|-------------|-------------|---------------|----------|
| 1           | 101         | IN            | 100      |
| 2           | 101         | OUT           | 50       |
| 3           | 102         | IN            | 200      |
| 4           | 102         | OUT           | 150      |

#### Query:
```sql
SELECT 
    material_id,
    (SUM(CASE WHEN movement_type = 'OUT' THEN quantity ELSE 0 END) /
     NULLIF(SUM(CASE WHEN movement_type = 'IN' THEN quantity ELSE 0 END), 0)) AS turnover_rate
FROM 
    material_movements
GROUP BY 
    material_id;
```

#### Explanation:
- Computes turnover rate as the ratio of `OUT` to `IN` movements.
- Uses `NULLIF` to avoid division by zero errors.

---

These examples demonstrate how SQL can solve tricky problems in WMS/MCS systems while integrating IoT and HMI data.
Feel free to ask if you'd like me to elaborate on any of these scenarios or if you have.