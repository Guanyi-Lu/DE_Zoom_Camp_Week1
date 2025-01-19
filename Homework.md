## Question 1. Understanding docker first run
Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash.

What's the version of pip in the image?

24.3.1
24.2.1
23.3.1
23.2.1

$ docker run -it python:3.12.8
Python 3.12.8 (main, Jan 14 2025, 05:32:36) [GCC 12.2.0] on linux
import pip
print(pip.__version__)
24.3.1


## Question 2 
Understanding Docker Networking and Docker Compose

Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

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

it should be db:5432. use localhost:8080 to open the pgadmin interface, then create a new server, name should be db,posts should be 5432

## Question 3:

git bash window:docker-compose up
open a new git bash window run below:

URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-10.csv.gz"
python ingest_data_green_taxi.py \
    --user=root \
    --password=root \
    --host=localhost \
    --port=5432 \
    --db=ny_taxi \
    --table_name=green_taxi_trips \
    --url=${URL}

login to postgres:
SELECT  
case when trip_distance <=1 then 'Up to 1 mile'
when trip_distance>1 and trip_distance<=3 then 'between 1 and 3'
when trip_distance>3 and trip_distance<=7 then 'between 3 and 7'
when trip_distance>7 and trip_distance<=10 then 'between 7 and 10'
when trip_distance >10 then 'more than 10 mile'
end as total_trips,
count(*)
FROM public.green_taxi_trips
WHERE cast(lpep_dropoff_datetime as DATE )>='2019-10-01' 
AND cast(lpep_dropoff_datetime as DATE )<'2019-11-01'
group by total_trips

ctrl+c to quit docker-compose

## Question 4:

select lpep_pickup_datetime,max(trip_distance) as maxumum
from public.green_taxi_trips
group by lpep_pickup_datetime
order by maxumum desc

## Question 5:
select sum(g.total_amount) as total_money,zpu."Zone",zpu.service_zone
from public.green_taxi_trips g 
     left join public.zones zpu
     on g."PULocationID"=zpu."LocationID"
	   left join public.zones zdo
     on g."DOLocationID"=zdo."LocationID"
WHERE cast(lpep_pickup_datetime as DATE )='2019-10-18' 
group by zpu."Zone",zpu.service_zone
having SUM(g.total_amount)>13000

## Question 6:
select max(g.tip_amount) as max_tip,zdo."Zone",zdo.service_zone
from public.green_taxi_trips g 
     left join public.zones zpu
     on g."PULocationID"=zpu."LocationID"
	   left join public.zones zdo
     on g."DOLocationID"=zdo."LocationID"
WHERE cast(lpep_pickup_datetime as DATE )>='2019-10-01' 
and cast(lpep_pickup_datetime as DATE )<='2019-10-31'
and zpu."Zone" = 'East Harlem North'
group by zdo."Zone",zdo.service_zone
order by max_tip desc

## Question 7. Terraform Workflow
Which of the following sequences, respectively, describes the workflow for:

Downloading the provider plugins and setting up backend,
Generating proposed changes and auto-executing the plan
Remove all resources managed by terraform

Answer: terraform init, terraform apply -auto-approve, terraform destro