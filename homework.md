Question 3. Count records
How many taxi trips were totally made on September 18th 2019?

Tip: started and finished on 2019-09-18.

Remember that lpep_pickup_datetime and lpep_dropoff_datetime columns are in the format timestamp (date and hour+min+sec) and not in date.

15767
15612 <---
15859
89009

Answer: select count(1) from green_taxi_trips where lpep_pickup_datetime >= '2019-09-18 00:00:00' and lpep_dropoff_da
 tetime < '2019-09-19 00:00:00'

Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.

Tip: For every trip on a single day, we only care about the trip with the longest distance.

2019-09-18
2019-09-16
2019-09-26 <---
2019-09-21

Answer: select lpep_pickup_datetime :: date from green_taxi_trips where trip_distance >= (select max(trip_distance) f
 rom green_taxi_trips)

Question 5. Three biggest pick up Boroughs
Consider lpep_pickup_datetime in '2019-09-18' and ignoring Borough has Unknown

Which were the 3 pick up Boroughs that had a sum of total_amount superior to 50000?

"Brooklyn" "Manhattan" "Queens"  <---
"Bronx" "Brooklyn" "Manhattan"
"Bronx" "Manhattan" "Queens"
"Brooklyn" "Queens" "Staten Island"

Answer: select zones."Borough", sum(green_taxi_trips.total_amount) from green_taxi_trips,zones where green_taxi_trips
 ."PULocationID" = zones."LocationID" and cast(green_taxi_trips.lpep_pickup_datetime as date) = '2019-09-18' group by zones."Borough"
  having sum(green_taxi_trips.total_amount) > 50000

Question 6. Largest tip
For the passengers picked up in September 2019 in the zone name Astoria which was the drop off zone that had the largest tip? We want the name of the zone, not the id.

Note: it's not a typo, it's tip , not trip

Central Park
Jamaica
JFK Airport   <---
Long Island City/Queens Plaza

Answer: select zdo."Zone" from green_taxi_trips t, zones zpu, zones zdo where t."PULocationID"=zpu."LocationID" and t
 ."DOLocationID"=zdo."LocationID" and TO_CHAR(t.lpep_pickup_datetime,'YYYY-MM')='2019-09' and zpu."Zone"='Astoria' and t.tip_amount >
 = (select max(t.tip_amount) from green_taxi_trips t, zones zpu where t."PULocationID"=zpu."LocationID" and TO_CHAR(t.lpep_pickup_dat
 etime,'YYYY-MM')='2019-09' and zpu."Zone"='Astoria')

