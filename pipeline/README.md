# Summary
This is the first module of my data engineering course. I learnt how to create a data pipeline that can extract, transform, and load real-world data into a database in a reproducible and efficient way. I explored how to use Docker to isolate environments, run PostgreSQL and pgAdmin containers, and manage data safely using volumes. I also worked with Python and pandas to process large datasets in chunks, and connected everything using SQLAlchemy to automate database ingestion.




## Homework

### Question 3. Counting short trips
For the trips in November 2025 (lpep_pickup_datetime between '2025-11-01' and '2025-12-01', exclusive of the upper bound), how many trips had a trip_distance of less than or equal to 1 mile?

#### SQL QUERY 
SELECT COUNT(*)
FROM green_taxi
WHERE lpep_pickup_datetime >= '2025-11-01'
  AND lpep_pickup_datetime < '2025-12-01'
  AND trip_distance <= 1;

#### RESULT
 8,007

### Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Only consider trips with trip_distance less than 100 miles (to exclude data errors).

#### SQL QUERY
SELECT lpep_pickup_datetime
FROM green_taxi
WHERE trip_distance < 100
ORDER BY trip_distance DESC
LIMIT 1

#### RESULT
 2025-11-14



### Question 5.
Which was the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025

#### SQL QUERY 
SELECT z.Zone AS zone, SUM(g.total_amount) AS total
FROM taxi_zone_lookup z
JOIN green_taxi g
ON z.locationid = g.pulocationid
WHERE g.lpep_pickup_datetime::date = '2025-11-18'
GROUP BY z.Zone
ORDER BY total DESC
LIMIT 1;

#### RESULT 
 East Harlem North

### Question 6. 
For the passengers picked up in the zone named "East Harlem North" in November 2025, which was the drop off zone that had the largest tip?

#### SQL QUERY ANSWER
SELECT dz.zone AS dropoff_zone, MAX(g.tip_amount) AS max_tip
FROM green_taxi g
JOIN taxi_zone_lookup dz
  ON g.dolocationid = dz.locationid
JOIN taxi_zone_lookup pz
  ON g.pulocationid = pz.locationid
WHERE pz.zone = 'East Harlem North'
  AND g.lpep_pickup_datetime::date BETWEEN '2025-11-01' AND '2025-11-30'
GROUP BY dz.zone
ORDER BY max_tip DESC
LIMIT 1;

#### RESULT
Yorkville West