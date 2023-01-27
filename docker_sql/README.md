### Test
    Basic test
    `docker run hello-world`
    Run bash in intractive mode
    `docker run -it ubuntu bash`
    Run docker image with python 
    `docker run -it python:3.9`
    Run docker image with python but start with bash

    For exit press `ctrl + d`

### Custom docker image
    `docker build -t test:pandas .`
    
    Dockerfile:
    ```
    FROM python:3.9.1

    RUN pip install pandas

    WORKDIR /app
    COPY pipeline.py pipeline.py

    ENTRYPOINT [ "python", "pipeline.py" ]
    ```

    pipeline.py:
    ```
    import sys
    import pandas as pd

    print(sys.argv)

    day = sys.argv[1]

    print(f'Job finished succesfully for day = {day}!')
    ```

### Running Postgres with Docker
``` bash
docker run -it \
-e POSTGRES_USER=root \
-e POSTGRES_PASSWORD=admin \
-e POSTRGRES_DB=ny_taxi \
-v dtc_postgres_volume_local:/var/lib/postgresql/data \
-p 5432:5432 \
postgres:13
```
###


Mount new volueme:
`docker volume create --name dtc_postgres_volume_local -d local dtc_postgres_volume_local`

Connect to postgres DB:
`pgcli -h localhost -p 5432 -u root -d ny_taxi`

Strart jupiter notebook:
`jupyter notebook --notebook-dir="C:\projects\de_zoomcamp"`

### NY Trips Dataset

Dataset:

* https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
* https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf


### pgAdmin

Running pgAdmin

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4
```

### Running Postgres and pgAdmin together

Create a network

```bash
docker network create pg-network
```

Run Postgres (change the path)

```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v dtc_postgres_volume_local:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Run pgAdmin

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin-2 \
  dpage/pgadmin4
```


