# SQL For Real-Time Data Processing
### By: [Partha Sarathi Kundu](https://www.linkedin.com/in/partha-sarathi-kundu/recent-activity/articles/)

10 practical examples of using SQL for real-time data processing, applicable to streaming systems, real-time dashboards, and alerting mechanisms.

---

### 1. **Real-Time Temperature Monitoring**
#### Scenario:
Fetch the latest temperature readings from IoT sensors to monitor warehouse conditions.

#### Table: `sensor_data`
| sensor_id | timestamp           | temperature |
|-----------|---------------------|-------------|
| 1         | 2024-01-01 08:00:00| 25.5        |
| 2         | 2024-01-01 08:05:00| 22.0        |
| 1         | 2024-01-01 08:10:00| 26.0        |

#### Query:
```sql
SELECT 
    sensor_id,
    MAX(timestamp) AS latest_time,
    temperature
FROM 
    sensor_data
WHERE 
    timestamp >= NOW() - INTERVAL 10 MINUTE
GROUP BY 
    sensor_id;
```
#### Explanation:
- Retrieves the most recent readings within the last 10 minutes.
- Used for real-time dashboards.

---

### 2. **Detect Anomalies in Network Traffic**
#### Scenario:
Identify spikes in traffic exceeding a threshold.

#### Table: `network_logs`
| log_id | timestamp           | traffic_mb |
|--------|---------------------|------------|
| 1      | 2024-01-01 08:00:00| 200        |
| 2      | 2024-01-01 08:05:00| 600        |
| 3      | 2024-01-01 08:10:00| 150        |

#### Query:
```sql
SELECT 
    timestamp,
    traffic_mb
FROM 
    network_logs
WHERE 
    traffic_mb > 500;
```
#### Explanation:
- Flags traffic exceeding 500 MB.
- Suitable for generating alerts.

---

### 3. **Real-Time Stock Price Updates**
#### Scenario:
Fetch the latest price of stocks for a trading dashboard.

#### Table: `stock_prices`
| stock_id | symbol | timestamp           | price |
|----------|--------|---------------------|-------|
| 1        | AAPL   | 2024-01-01 08:00:00| 150.25|
| 2        | TSLA   | 2024-01-01 08:05:00| 705.30|

#### Query:
```sql
SELECT 
    symbol,
    price
FROM 
    stock_prices
WHERE 
    timestamp = (SELECT MAX(timestamp) FROM stock_prices);
```
#### Explanation:
- Retrieves the latest stock prices for each symbol.

---

### 4. **Monitor Machine Utilization**
#### Scenario:
Calculate the average utilization of machines over the last hour.

#### Table: `machine_logs`
| machine_id | timestamp           | utilization |
|------------|---------------------|-------------|
| 1          | 2024-01-01 08:00:00| 75          |
| 2          | 2024-01-01 08:05:00| 80          |
| 1          | 2024-01-01 08:10:00| 78          |

#### Query:
```sql
SELECT 
    machine_id,
    AVG(utilization) AS avg_utilization
FROM 
    machine_logs
WHERE 
    timestamp >= NOW() - INTERVAL 1 HOUR
GROUP BY 
    machine_id;
```
#### Explanation:
- Calculates average utilization for each machine in real time.

---

### 5. **Live Order Fulfillment**
#### Scenario:
Identify orders that have not been fulfilled within the expected time.

#### Table: `orders`
| order_id | status    | created_at          |
|----------|-----------|---------------------|
| 1        | pending   | 2024-01-01 07:55:00|
| 2        | completed | 2024-01-01 08:10:00|

#### Query:
```sql
SELECT 
    order_id,
    status,
    created_at
FROM 
    orders
WHERE 
    status = 'pending' AND created_at <= NOW() - INTERVAL 15 MINUTE;
```
#### Explanation:
- Lists orders pending for more than 15 minutes for prioritization.

---

### 6. **Track Real-Time Energy Consumption**
#### Scenario:
Monitor the energy consumption of devices to detect high usage.

#### Table: `energy_logs`
| device_id | timestamp           | energy_kwh |
|-----------|---------------------|------------|
| 1         | 2024-01-01 08:00:00| 5.0        |
| 2         | 2024-01-01 08:05:00| 7.5        |

#### Query:
```sql
SELECT 
    device_id,
    SUM(energy_kwh) AS total_energy
FROM 
    energy_logs
WHERE 
    timestamp >= NOW() - INTERVAL 1 HOUR
GROUP BY 
    device_id;
```
#### Explanation:
- Tracks total energy usage for each device over the past hour.

---

### 7. **Analyze Real-Time Website Performance**
#### Scenario:
Monitor the average response time for API endpoints in real time.

#### Table: `api_logs`
| endpoint     | timestamp           | response_time |
|--------------|---------------------|---------------|
| /login       | 2024-01-01 08:00:00| 150 ms        |
| /dashboard   | 2024-01-01 08:05:00| 200 ms        |

#### Query:
```sql
SELECT 
    endpoint,
    AVG(response_time) AS avg_response_time
FROM 
    api_logs
WHERE 
    timestamp >= NOW() - INTERVAL 5 MINUTE
GROUP BY 
    endpoint;
```
#### Explanation:
- Calculates the average response time for each endpoint in the last 5 minutes.

---

### 8. **Detect Failed Payment Transactions**
#### Scenario:
Identify payment transactions that failed in real time.

#### Table: `transactions`
| transaction_id | timestamp           | status    |
|----------------|---------------------|-----------|
| 1              | 2024-01-01 08:00:00| success   |
| 2              | 2024-01-01 08:05:00| failed    |

#### Query:
```sql
SELECT 
    transaction_id,
    timestamp
FROM 
    transactions
WHERE 
    status = 'failed' AND timestamp >= NOW() - INTERVAL 1 MINUTE;
```
#### Explanation:
- Identifies transactions that failed in the last minute.

---

### 9. **Monitor Inventory Levels in Real Time**
#### Scenario:
Track inventory levels of products and flag items running low.

#### Table: `inventory`
| product_id | product_name | stock_quantity |
|------------|--------------|----------------|
| 1          | Widget A     | 10             |
| 2          | Widget B     | 50             |

#### Query:
```sql
SELECT 
    product_id,
    product_name,
    stock_quantity
FROM 
    inventory
WHERE 
    stock_quantity < 20;
```
#### Explanation:
- Flags products with stock quantities below 20.

---

### 10. **Real-Time Error Tracking**
#### Scenario:
Track error logs to detect recurring issues in the last 30 minutes.

#### Table: `error_logs`
| log_id | timestamp           | error_message |
|--------|---------------------|---------------|
| 1      | 2024-01-01 08:00:00| Timeout Error |
| 2      | 2024-01-01 08:10:00| DB Connection |

#### Query:
```sql
SELECT 
    error_message,
    COUNT(*) AS occurrence_count
FROM 
    error_logs
WHERE 
    timestamp >= NOW() - INTERVAL 30 MINUTE
GROUP BY 
    error_message
ORDER BY 
    occurrence_count DESC;
```
#### Explanation:
- Counts occurrences of error messages in the last 30 minutes.
- Helps identify frequent issues in real time.

---

These examples demonstrate how SQL can be used to process and analyze real-time data effectively.
