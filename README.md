# SQL-practice-notes
SQL Practice and important notes
=====================================

### 1. **Santa wants to know which gifts weigh more than 1 kg. Can you list them?**

#### Table: `gifts`
| gift_id | gift_name      | recipient_type | weight_kg |
|---------|----------------|----------------|-----------|
| 1       | Toy Train      | good           | 2.5       |
| 2       | Chocolate Box  | good           | 0.8       |
| 3       | Teddy Bear     | good           | 1.2       |
| 4       | Board Game     | naughty        | 0.9       |

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
| 4           | 2        | Elf           |
| 5           | 2        | Gift Box      |
| 6           | 3        | Reindeer      |
| 7           | 3        | Tree          |
| 8           | 4        | Snowman       |
| 9           | 4        | Santa Claus   |
| 10          | 4        | Tree          |
| 11          | 4        | Elf           |
| 12          | 4        | Gift Box      |

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

---

### 9. **The Grinch is planning out his pranks for this holiday season. Which pranks have a difficulty level of “Advanced” or “Expert"? List the prank name and location (both in descending order).**

#### Table: `grinch_pranks`
| prank_id | prank_name              | location               | difficulty |
|----------|-------------------------|------------------------|------------|
| 1        | Stealing Stockings      | Whoville              | Beginner   |
| 2        | Christmas Tree Topple   | Whoville Town Square  | Advanced   |
| 3        | Present Swap            | Cindy Lous House      | Beginner   |
| 4        | Sleigh Sabotage         | Mount Crumpit         | Expert     |
| 5        | Chimney Block           | Mayors Mansion        | Expert     |

---

### 10. **A family reunion is being planned, and the organizer wants to identify the three family members with the most children. Write a query to calculate the total number of children for each parent and rank them. Include the parent’s name and their total number of children in the result.**

#### Table: `family_members`
| member_id | name       | age |
|-----------|------------|-----|
| 1         | Alice      | 30  |
| 2         | Bob        | 58  |
| 3         | Charlie    | 33  |
| 4         | Diana      | 55  |
| 5         | Eve        | 5   |
| 6         | Frank      | 60  |
| 7         | Grace      | 32  |
| 8         | Hannah     | 8   |
| 9         | Ian        | 12  |
| 10        | Jack       | 3   |

#### Table: `parent_child_relationships`
| parent_id | child_id |
|-----------|----------|
| 2         | 1        |
| 3         | 5        |
| 4         | 1        |
| 6         | 7        |
| 6         | 8        |
| 7         | 9        |
| 7         | 10       |
| 4         | 8        |

---

### 11. **We are looking for cheap gifts at the market. Which vendors are selling items priced below $10? List the unique (i.e., remove duplicates) vendor names.**

#### Table: `vendors`
| vendor_id | vendor_name      | market_location  |
|-----------|------------------|------------------|
| 1         | Cozy Crafts      | Downtown Square  |
| 2         | Sweet Treats     | Central Park     |
| 3         | Winter Warmers   | Downtown Square  |

#### Table: `item_prices`
| item_id | vendor_id | item_name            | price_usd |
|---------|-----------|----------------------|----------|
| 1       | 1         | Knitted Scarf       | 25       |
| 2       | 2         | Hot Chocolate       | 5        |
| 3       | 2         | Gingerbread Cookie  | 3.5      |
| 4       | 3         | Wool Hat            | 18       |
| 5       | 3         | Santa Pin           | 2        |

---

### 12. **The Grinch tracked his weight every day in December to analyze how it changed daily. Write a query to return the weight change (in pounds) for each day, calculated as the difference from the previous day's weight.**

#### Table: `grinch_weight_log`
| log_id | day_of_month | weight |
|--------|--------------|--------|
| 1      | 1            | 250    |
| 2      | 2            | 248    |
| 3      | 3            | 249    |
| 4      | 4            | 247    |
| 5      | 5            | 246    |
| 6      | 6            | 248    |

---

### 13. **Santa is tracking how many presents he delivers each night leading up to Christmas. He wants a running total to see how many gifts have been delivered so far on any given night. Using the deliveries table, calculate the cumulative sum of gifts delivered, ordered by the delivery date.**

#### Table: `deliveries`
| delivery_date | gifts_delivered |
|---------------|-----------------|
| 2024-12-20    | 120             |
| 2024-12-21    | 150             |
| 2024-12-22    | 200             |
| 2024-12-23    | 300             |
| 2024-12-24    | 500             |

---

### 14. **Santa is analyzing his deliveries to determine which days had the heaviest loads. Write a query to calculate the daily total weight of gifts delivered.**

#### Table: `deliveries`
| delivery_date | gifts_delivered | weight_kg |
|---------------|-----------------|-----------|
| 2024-12-20    | 120             | 360       |
| 2024-12-21    | 150             | 450       |
| 2024-12-22    | 200             | 600       |
| 2024-12-23    | 300             | 900       |
| 2024-12-24    | 500             | 1500      |

---

### 15. **Find the beaches expected to have temperatures above 30°C on Christmas Day. Include their beach_name and temperature.**

#### Table: `beach_temperature_predictions`
| beach_name      | country       | expected_temperature_c | date       |
|------------------|--------------|-------------------------|------------|
| Bondi Beach      | Australia    | 32                      | 2024-12-24 |
| Copacabana Beach | Brazil       | 28                      | 2024-12-24 |
| Clifton Beach    | South Africa | 31                      | 2024-12-25 |
| Brighton Beach   | New Zealand  | 25                      | 2024-12-25 |

---

### 16. **Scientists are studying the diets of polar bears. Write a query to find the maximum amount of food (in kilograms) consumed by each polar bear in a single meal December 2024. Include the bear_name and biggest_meal_kg, and sort the results in descending order of largest meal consumed.**

#### Table: `polar_bears`
| bear_id | bear_name | age |
|---------|-----------|-----|
| 1       | Snowball  | 10  |
| 2       | Frosty    | 7   |
| 3       | Iceberg   | 15  |

#### Table: `meal_log`
| log_id | bear_id | food_type | food_weight_kg | date       |
|--------|---------|-----------|----------------|------------|
| 1      | 1       | Seal      | 30             | 2024-12-01 |
| 2      | 2       | Fish      | 15             | 2024-12-02 |
| 3      | 1       | Fish      | 10             | 2024-12-03 |
| 4      | 3       | Seal      | 25             | 2024-12-04 |
| 5      | 2       | Seal      | 20             | 2024-12-05 |
| 6      | 3       | Fish      | 18             | 2024-12-06 |

---

### 17. **Santa wants to evaluate his sleigh’s weight distribution. Write a query to calculate the total weight of gifts for each recipient.**

#### Table: `gifts`
| gift_id | gift_name         | recipient | weight_kg |
|---------|-------------------|-----------|-----------|
| 1       | Toy Train         | John      | 2.5       |
| 2       | Chocolate Box     | Alice     | 0.8       |
| 3       | Teddy Bear        | Sophia    | 1.2       |
| 4       | Board Game        | John      | 0.9       |

---

### 18. **The Grinch tracked the total calories in his holiday feasts. Write a query to find the total calories consumed in each feast, sorted by feast name.**

#### Table: `feasts`
| feast_id | feast_name          | date       |
|----------|---------------------|------------|
| 1        | Christmas Eve Feast | 2024-12-24 |
| 2        | New Year’s Feast    | 2024-12-31 |

#### Table: `feast_food`
| food_id | feast_id | food_name      | calories |
|---------|----------|----------------|----------|
| 1       | 1        | Roast Turkey   | 3500     |
| 2       | 1        | Stuffing       | 500      |
| 3       | 2        | Champagne      | 150      |
| 4       | 2        | Cheesecake     | 450      |

---

### 19. **The Grinch wants to plan pranks on those with the most valuable items. Find the top 3 gifts in terms of price-to-weight ratio (price per kg). Include gift_name and ratio.**

#### Table: `gifts`
| gift_id | gift_name       | price_usd | weight_kg |
|---------|-----------------|-----------|-----------|
| 1       | Toy Train       | 25        | 2.5       |
| 2       | Chocolate Box   | 10        | 0.8       |
| 3       | Teddy Bear      | 20        | 1.2       |
| 4       | Board Game      | 15        | 0.9       |

---

### 20. **Santa’s elves are evaluating gift nutritional value. Write a query to find the top 3 gifts with the highest calorie density (calories per gram). Include gift_name, category, and calorie_density.**

#### Table: `gift_nutrition`
| gift_id | gift_name       | category  | calories | weight_g |
|---------|-----------------|-----------|----------|----------|
| 1       | Chocolate Bar   | Sweets    | 250      | 100      |
| 2       | Candy Cane      | Sweets    | 200      | 150      |
| 3       | Fruitcake       | Baked     | 400      | 250      |
| 4       | Gingerbread Man | Baked     | 500      | 350      |

---

### 21. **Santa is preparing his list of naughty and nice children. Write a query to calculate the number of nice and naughty children in each region.**

#### Table: `children`
| child_id | child_name   | region        | status   |
|----------|-------------|---------------|----------|
| 1        | Cindy Lou   | Whoville      | nice     |
| 2        | Max the Dog | Whoville      | nice     |
| 3        | The Grinch  | Mount Crumpit | naughty  |
| 4        | Sam Who     | Whoville      | nice     |

---

### 22. **A candy store owner wants to calculate the total revenue for each candy category. Write a query to rank candy categories by their total revenue.**

#### Table: `candy_sales`
| sale_id | candy_name         | category     | quantity_sold | price_per_unit |
|---------|--------------------|--------------|---------------|----------------|
| 1       | Candy Cane         | Sweets       | 20            | 1.5            |
| 2       | Chocolate Bar      | Chocolate    | 10            | 2              |
| 3       | Lollipop           | Sweets       | 5             | 0.75           |
| 4       | Dark Chocolate     | Chocolate    | 8             | 2.5            |

---

### 23. **Find the top 3 ski resorts with the highest snowfall in December 2024. Include resort_name and snowfall_inches.**

#### Table: `ski_resorts`
| resort_id | resort_name         | location   |
|-----------|---------------------|------------|
| 1         | Snowy Peaks         | Colorado   |
| 2         | Winter Wonderland   | Utah       |
| 3         | Frozen Slopes       | Alaska     |

#### Table: `snowfall`
| snowfall_id | resort_id | snowfall_inches | date       |
|-------------|-----------|-----------------|------------|
| 1           | 1         | 60              | 2024-12-20 |
| 2           | 2         | 45              | 2024-12-21 |
| 3           | 3         | 75              | 2024-12-22 |

---

### 24. **Santa is optimizing his Christmas routes. Write a query to calculate the total gifts delivered per country.**

#### Table: `routes`
| route_id | country      | gifts_delivered |
|----------|--------------|-----------------|
| 1        | USA          | 500             |
| 2        | Canada       | 300             |
| 3        | Mexico       | 200             |

---

[All the SQL Solutions here](/SQL-1-5.md)