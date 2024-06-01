# Part 1

1. Run the CSV server Docker container in detached mode:
    ```
    docker run -d --name csvserver infracloudio/csvserver:latest
    ```

2. Check the running Docker containers:
    ```
    docker ps
    ```

3. View the logs of the CSV server container:
    ```
    docker logs csvserver
    ```

4. If step 1 fails due to the input file not being present, follow these steps:
    - Create a script named `gencsv.sh` to generate the input file.
5. Add this shell script to create inputfile
   ```#!/bin/bash

        start=$1
        end=$2

        if [ -z "$start" ] || [ -z "$end" ]; then
        echo "Usage: $0 <start> <end>"
        exit 1
        fi

        file="inputFile"
        rm -f $file

        for ((i=$start; i<=$end; i++))
        do
        echo "$i, $((RANDOM % 1000))" >> $file
        done
   ```
    - Provide execution permissions to the script:
        ```
        chmod +x gencsv.sh
        ```
    - Generate the input file using the script:
        ```
        ./gencsv.sh 2 8
        ```
6. Again re-run the container by using the command
  ```
      docker run -d --name csvserver infracloudio/csvserver:latest
  ```
7.check whether docker is csvserver is running or not using command 
   ```
   docker ps -a
   ```  
8.check logs as well if not running using 
   ```
   docker logs csvserver
   ```   
9.if you again got this error 
```
error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory
```
10. you can copy the file into the container after it starts:
```
docker cp inputFile csvserver:/csvserver/inputdata
```
11. now again run the container and check whether its running or not ,this time it resolves theissues.will move to next step.
12. Run the Docker container with a volume mounted and environment variable set:
    ```
    docker run -d --name csvserver -v "path-to/inputdata" -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest
    ```

13. Access the application using the URL:
    ```
    http://localhost:9393
    ```

# Part 2

1. Stop and remove containers using the following commands:
    ```
    docker stop csvserver
    docker rm csvserver
    ```

2. Created a `docker-compose.yaml` file and add this script.
```
version: '3.8'

services:
  csvserver:
    image: infracloudio/csvserver:latest
    container_name: csvserver
    volumes:
      - ./inputFile:/csvserver/inputdata
    env_file:
      - csvserver.env
    ports:
      - "9300:9300"
```

3. Created an environment variable file named `csvserver.env`.

4. Ensure the application can be run with Docker Compose:
    ```
    docker-compose up -d
    ```

5. Saved the changes and push to GitHub.

# Part 3

1. Deleted any containers running from the last part:
    ```
    docker-compose down
    ```

2. Updated the `docker-compose.yaml` file by Attaching Prometheus container (prom/prometheus:v2.45.2) to the docker-compose.yaml form part II.
```
  prometheus:
    image: prom/prometheus:v2.45.2
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
```

3. created a file named `prometheus.yml` with the required content in solution directory.

4. Verified Prometheus is accessible at `http://localhost:9090` on the host.

5. Run the containers using Docker Compose:
    ```
    docker-compose up -d
    ```

6. Typed `csvserver_records` in the query box of Prometheus, click on Execute, and switch to the Graph tab.

7. There is a straight line graph with the value 7.
