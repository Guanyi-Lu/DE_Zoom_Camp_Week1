# DEZoomcamp - Week 1: Docker, PostgreSQL, and Terraform

## Question 1. Understanding Docker First Run

Run Docker with the `python:3.12.8` image in an interactive mode, using the entrypoint `bash`.

**What's the version of pip in the image?**

- 24.3.1
- 24.2.1
- 23.3.1
- 23.2.1

```bash
$ docker run -it python:3.12.8

import pip

print(pip.__version__)
```


## Question 2 

Understanding Docker Networking and Docker Compose

Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?
```yaml
services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin  

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```

it should be db:5432. use localhost:8080 to open the pgadmin interface, then create a new server, name should be db,posts should be 5432.
pgAdmin (inside Docker) connects to db:5432 (service name & container port).
Your local machine connects using localhost:5433 (host port).

## Question 3:

```bash
docker-compose up
```
open a new git bash window run below:
```
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-10.csv.gz"
python ingest_data_green_taxi.py \
    --user=root \
    --password=root \
    --host=localhost \
    --port=5432 \
    --db=ny_taxi \
    --table_name=green_taxi_trips \
    --url=${URL}
```

login to postgres:

```sql
SELECT  
  CASE 
    WHEN trip_distance <= 1 THEN 'Up to 1 mile'
    WHEN trip_distance > 1 AND trip_distance <= 3 THEN 'between 1 and 3'
    WHEN trip_distance > 3 AND trip_distance <= 7 THEN 'between 3 and 7'
    WHEN trip_distance > 7 AND trip_distance <= 10 THEN 'between 7 and 10'
    WHEN trip_distance > 10 THEN 'more than 10 miles'
  END AS total_trips,
  COUNT(*)
FROM public.green_taxi_trips
WHERE CAST(lpep_dropoff_datetime AS DATE) >= '2019-10-01' 
  AND CAST(lpep_dropoff_datetime AS DATE) < '2019-11-01'
GROUP BY total_trips
```

## Question 4
```sql
SELECT 
  lpep_pickup_datetime, 
  MAX(trip_distance) AS maximum
FROM public.green_taxi_trips
GROUP BY lpep_pickup_datetime
ORDER BY maximum DESC
```



## Question 5:
```sql

SELECT 
  SUM(g.total_amount) AS total_money,
  zpu."Zone",
  zpu.service_zone
FROM public.green_taxi_trips g
  LEFT JOIN public.zones zpu
    ON g."PULocationID" = zpu."LocationID"
  LEFT JOIN public.zones zdo
    ON g."DOLocationID" = zdo."LocationID"
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-10-18'
GROUP BY zpu."Zone", zpu.service_zone
HAVING SUM(g.total_amount) > 13000
```

## Question 6:

```sql
SELECT 
  MAX(g.tip_amount) AS max_tip,
  zdo."Zone",
  zdo.service_zone
FROM public.green_taxi_trips g
  LEFT JOIN public.zones zpu
    ON g."PULocationID" = zpu."LocationID"
  LEFT JOIN public.zones zdo
    ON g."DOLocationID" = zdo."LocationID"
WHERE CAST(lpep_pickup_datetime AS DATE) >= '2019-10-01'
  AND CAST(lpep_pickup_datetime AS DATE) <= '2019-10-31'
  AND zpu."Zone" = 'East Harlem North'
GROUP BY zdo."Zone", zdo.service_zone
ORDER BY max_tip DESC
```

## Question 7. Terraform Workflow

Which of the following sequences, **respectively**, describes the workflow for: 
1. Downloading the provider plugins and setting up backend,
2. Generating proposed changes and auto-executing the plan
3. Remove all resources managed by terraform`

Answer: terraform init, terraform apply -auto-approve, terraform destroy

