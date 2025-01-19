# Interact with Postgres

## Network:
Using Docker networks allows for more control over container communication, making it essential for larger-scale or more complex Docker environments

docker network create pg-network (run this only onetime)

### pg-database container
This container is to run the pgAdmin application, which is a web-based tool for managing and interacting with PostgreSQL databases.

docker run -d \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v c:/Users/Guanyi/DE_Zoom_Camp/Week1/ny_taxi_postgres_data:/var/lib/postgresql/data \ (first is host machine path, second is container path)
  -p 5432:5432 \
  --network pg-network \
  --name pg-database \ 
  
pg-database is the name of the container and also what you put in the pgadmin interface as the host name/address to create new server
postgres:13  The docker image being used is postgres 13 to ensure i have the postgres sql server running in side this container

docker run -p 8080:80 \
    -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
    --network pg-network  \
    --name pgadmin-2 \#(this is my new docker name)
    -d dpage/pgadmin4 

## What pgadmin-2 does: 
 This command starts a new pgAdmin container from the dpage/pgadmin4 image, configures it with an email and password for logging in, connects it to a custom Docker network (pg-network),exposes port 8080 on your local machine, and runs the container in the background (detached mode).

### Detailed explaination:
 The -p 8080:80 flag maps port 8080 on your local machine (host) to port 80 inside the container
 -d dpage/pgadmin4 :this means to run the pgadmin4 docker image from the docker hub. The pgadmin4 Docker image contains only the pgAdmin web-based  interface for managing PostgreSQL servers,it does not include or start a PostgreSQL server itself and that's why we need to use network to make two containers talk

 ### What happens next: 
  After running this command, you can open a browser and go to http://localhost:8080 to access the pgAdmin web interface. 
  Use the email and password you've set to log in and start managing your PostgreSQL databases.

# Data Ingestion

## Method 1: Direct run approach, you're running the script directly on your local machine, which could have dependencies that might not be consistent across different environments.

### parquet file:
URL="https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2021-01.parquet"
python ingest_data_parqut.py \
    --user=root \
    --password=root \
    --host=localhost \
    --port=5432 \
    --db=ny_taxi \
    --tb=yellow_taxi_trips \
    --url=${URL}

### csv file:

URL="https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv"
python ingest_data_zones.py \
    --user=root \
    --password=root \
    --host=localhost \
    --port=5432 \
    --db=ny_taxi \
    --tb=zones \
    --url=${URL}


## Method 2: Dockerization
### Dockrize the ingestion script to ingest data to the table, compare this with above direct ingestion method in my local machine

docker build -t taxi_ingest:v001 (using the content in Dockerfile):

FROM python:3.9

RUN pip install pandas
RUN pip install pgcli
RUN pip install sqlalchemy
RUN pip install psycopg2
RUN pip install  pyarrow

WORKDIR /app

COPY ingest_data_parqut.py ingest_data_parqut.py

ENTRYPOINT ["python","ingest_data_parqut.py"]

#In Git bash:

docker run -it \
  --network=pg-network \
    taxi_ingest:v001 \
    --user root \
    --password root \
    --host pg-database \
    --port 5432 \
    --db ny_taxi \
    --tb yellow_taxi_trips \
    --url "https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2021-01.parquet"

## Compare the two approach:

Use Dockerizing (taxi_ingest approach) if you need a consistent, isolated environment for running the script.It’s great if you plan to run the script across different machines or environments and want to avoid dependency issues.
Use the Direct Run Approach if you're working in a simple setup and don't mind handling dependencies manually.It's faster for quick development or testing on your local machine but might lack the robustness and portability of the Dockerized approach.
