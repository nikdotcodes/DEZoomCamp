# Question 3

```sql
select count(*)
from yellow_taxi_trips
where tpep_pickup_datetime::date = '20210115'
```

# Question 4

```sql
select tpep_pickup_datetime::date as pickupDate
	, tip_amount
from yellow_taxi_trips
where DATE_PART('month', tpep_pickup_datetime) = 1
order by tip_amount DESC
LIMIT 1
```

# Question 5

```sql
SELECT du.taxizone, Count("DOLocationID") as NumberOfDO
FROM public.yellow_taxi_trips trips
	left outer join taxi_zones as pu on trips."PULocationID" = pu.locationid
	left outer join taxi_zones as du on trips."DOLocationID" = du.locationid
where pu.taxizone = 'Central Park'
	and tpep_pickup_datetime::DATE = '20210114'
GROUP BY du.taxizone
ORDER BY Count("DOLocationID") DESC
LIMIT 1
```

# Question 6

```sql
SELECT pu.taxizone || ' / ' || CASE WHEN du.taxizone = 'NA' THEN 'Unknown' ELSE du.taxizone END as pickup_dropoff_pair, avg(total_amount)
FROM public.yellow_taxi_trips trips
	left outer join taxi_zones as pu on trips."PULocationID" = pu.locationid
	left outer join taxi_zones as du on trips."DOLocationID" = du.locationid
group by pu.taxizone || ' / ' || CASE WHEN du.taxizone = 'NA' THEN 'Unknown' ELSE du.taxizone END
order by avg(total_amount) desc
limit 1
```