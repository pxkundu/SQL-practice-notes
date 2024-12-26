**10 use cases for SQL in a large data management system** designed for Warehouse Management Systems (WMS) or Material Control Systems (MCS), with data integration from **HMI**, **IoT sensors**, or tools like **Ignition scripts**:

---

### 1. **Real-Time Monitoring of Sensor Data**
#### Scenario:
You need to fetch the latest readings from IoT sensors monitoring warehouse temperature and humidity.

#### Table: `sensor_readings`
| reading_id | sensor_id | timestamp           | temperature | humidity |
|------------|-----------|---------------------|-------------|----------|
| 1          | 101       | 2024-01-01 08:00:00| 25.5        | 60       |

#### Query:
```sql
SELECT 
    sensor_id,
    temperature,
    humidity,
    timestamp
FROM 
    sensor_readings
WHERE 
    timestamp = (
        SELECT MAX(timestamp) 
        FROM sensor_readings 
        WHERE sensor_id = @SensorId
    );
```

#### Explanation:
- Retrieves the most recent reading for a specific sensor.
- Used in Ignition scripts to monitor real-time conditions on HMI dashboards.

---

### 2. **Tracking Material Movements**
#### Scenario:
Log material movements between storage bins and fetch recent activity for reporting.

#### Table: `material_movements`
| movement_id | material_id | from_bin | to_bin | moved_at           | quantity |
|-------------|-------------|----------|--------|--------------------|----------|
| 1           | 501         | A1       | B2     | 2024-01-01 10:00  | 100      |

#### Query:
```sql
SELECT 
    material_id,
    from_bin,
    to_bin,
    quantity,
    moved_at
FROM 
    material_movements
WHERE 
    moved_at >= DATEADD(DAY, -7, GETDATE());
```

#### Explanation:
- Retrieves material movements within the last 7 days.
- Used to generate weekly inventory movement reports in Ignition.

---

### 3. **Alert for Low Inventory Levels**
#### Scenario:
Generate alerts for materials that fall below their minimum stock levels.

#### Table: `inventory`
| material_id | material_name | stock_quantity | min_stock_level |
|-------------|---------------|----------------|-----------------|
| 501         | Steel Rods    | 20             | 50              |

#### Query:
```sql
SELECT 
    material_id,
    material_name,
    stock_quantity
FROM 
    inventory
WHERE 
    stock_quantity < min_stock_level;
```

#### Explanation:
- Identifies materials with stock levels below their thresholds.
- Used in Ignition scripts to trigger HMI alerts or notifications.

---

### 4. **Calculate Utilization of Storage Bins**
#### Scenario:
Determine the percentage utilization of each storage bin based on its capacity.

#### Table: `storage_bins`
| bin_id | total_capacity | current_occupancy |
|--------|----------------|--------------------|
| A1     | 1000           | 750                |

#### Query:
```sql
SELECT 
    bin_id,
    (current_occupancy * 100.0 / total_capacity) AS utilization_percentage
FROM 
    storage_bins;
```

#### Explanation:
- Calculates the utilization percentage for each bin.
- Useful for visualizing storage efficiency on HMI dashboards.

---

### 5. **Retrieve Sensor Data in Batches**
#### Scenario:
Process historical sensor data in chunks for analysis or reporting.

#### Query:
```sql
SELECT 
    sensor_id,
    timestamp,
    temperature,
    humidity
FROM 
    sensor_readings
WHERE 
    timestamp BETWEEN @StartDate AND @EndDate
ORDER BY 
    timestamp
OFFSET @BatchSize * (@BatchNumber - 1) ROWS
FETCH NEXT @BatchSize ROWS ONLY;
```

#### Explanation:
- Implements pagination to process large datasets from IoT sensors.
- Suitable for Ignition scripts handling historical data queries.

---

### 6. **Identify Idle Equipment**
#### Scenario:
Detect equipment that hasn’t logged any activity in the last 24 hours.

#### Table: `equipment_logs`
| equipment_id | last_active           |
|--------------|-----------------------|
| EQ1          | 2024-01-01 09:00:00  |

#### Query:
```sql
SELECT 
    equipment_id,
    last_active
FROM 
    equipment_logs
WHERE 
    last_active < DATEADD(HOUR, -24, GETDATE());
```

#### Explanation:
- Identifies idle equipment based on activity logs.
- Used in MCS for operational monitoring and maintenance planning.

---

### 7. **Generate Pallet Load Recommendations**
#### Scenario:
Optimize pallet loads by calculating the remaining capacity for each pallet.

#### Table: `pallets`
| pallet_id | max_capacity | current_load |
|-----------|--------------|--------------|
| P1        | 1000         | 750          |

#### Query:
```sql
SELECT 
    pallet_id,
    (max_capacity - current_load) AS remaining_capacity
FROM 
    pallets
WHERE 
    remaining_capacity > 0;
```

#### Explanation:
- Suggests pallets with available space for further loading.
- Used in MCS systems to improve warehouse packing efficiency.

---

### 8. **Generate Maintenance Schedules**
#### Scenario:
Identify equipment due for maintenance based on usage hours.

#### Table: `equipment_usage`
| equipment_id | usage_hours | maintenance_threshold |
|--------------|-------------|------------------------|
| EQ1          | 500         | 1000                  |

#### Query:
```sql
SELECT 
    equipment_id,
    usage_hours,
    maintenance_threshold
FROM 
    equipment_usage
WHERE 
    (maintenance_threshold - usage_hours) <= 100;
```

#### Explanation:
- Flags equipment nearing its maintenance threshold.
- Triggers maintenance notifications in Ignition.

---

### 9. **Analyze Throughput of Conveyor Belts**
#### Scenario:
Calculate the total throughput for each conveyor belt in the past 24 hours.

#### Table: `conveyor_throughput`
| belt_id | material_id | timestamp           | quantity |
|---------|-------------|---------------------|----------|
| B1      | 501         | 2024-01-01 10:00:00| 200      |

#### Query:
```sql
SELECT 
    belt_id,
    SUM(quantity) AS total_throughput
FROM 
    conveyor_throughput
WHERE 
    timestamp >= DATEADD(HOUR, -24, GETDATE())
GROUP BY 
    belt_id;
```

#### Explanation:
- Aggregates material quantities processed by each conveyor in the last 24 hours.
- Useful for monitoring belt performance on HMIs.

---

### 10. **Identify Frequent Errors in Sensor Data**
#### Scenario:
Analyze sensor logs for recurring errors in a specific time frame.

#### Table: `sensor_logs`
| log_id | sensor_id | error_code | timestamp           |
|--------|-----------|------------|---------------------|
| 1      | 101       | ERR01      | 2024-01-01 08:00:00|

#### Query:
```sql
SELECT 
    error_code,
    COUNT(*) AS occurrence_count
FROM 
    sensor_logs
WHERE 
    timestamp BETWEEN @StartDate AND @EndDate
GROUP BY 
    error_code
ORDER BY 
    occurrence_count DESC;
```

#### Explanation:
- Counts occurrences of each error code within a time frame.
- Helps identify recurring issues for troubleshooting.

---

These use cases demonstrate SQL’s capabilities in managing large datasets for WMS/MCS applications, integrating IoT and HMI data using Ignition-like scripting environments. 