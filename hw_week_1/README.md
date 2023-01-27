## Downloading data
```
URL=https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-01.csv.gz
python ingest_data.py   --user=root   --password=root   --host=localhost   --port=5432   --db=ny_taxi   --table_name=green_taxi_trips   --url=${URL}
URL=https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv
python ingest_data.py   --user=root   --password=root   --host=localhost   --port=5432   --db=ny_taxi   --table_name=taxi_zone_lookup   --url=${URL}
```

How many taxi trips were totally made on January 15?
```
SELECT COUNT(1)
FROM public.green_taxi_trips t
WHERE DATE(t.lpep_pickup_datetime) = '2019-01-15'
```

Which was the day with the largest trip distance?
```
SELECT DATE(t.lpep_pickup_datetime), COUNT(1) as trip_count
FROM public.green_taxi_trips t
GROUP BY DATE(t.lpep_pickup_datetime)
ORDER BY trip_count DESC```

In 2019-01-01 how many trips had 2 and 3 passengers?
```
SELECT t.passenger_count, COUNT(1) as cnt
FROM public.green_taxi_trips t
WHERE DATE(t.lpep_pickup_datetime) = '2019-01-01'
GROUP BY t.passenger_count
HAVING t.passenger_count in (2, 3)```

For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip? We want the name of the zone, not the id.
```
SELECT  ldo."Zone", MAX(tip_amount) tips
FROM public.green_taxi_trips t
INNER JOIN public.taxi_zone_lookup lpu
	ON lpu."LocationID" = t."PULocationID"
INNER JOIN public.taxi_zone_lookup ldo
	ON ldo."LocationID" = t."DOLocationID"
WHERE lpu."Zone" = 'Astoria'
GROUP BY ldo."Zone"
ORDER BY MAX(tip_amount) DESC
LIMIT 1
```

