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
    - Provide execution permissions to the script:
        ```
        chmod +x gencsv.sh
        ```
    - Generate the input file using the script:
        ```
        ./gencsv.sh 2 8
        ```

5. Run the Docker container with a volume mounted and environment variable set:
    ```
    docker run -d --name csvserver -v "path-to/inputdata" -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest
    ```

6. Access the application using the URL:
    ```
    http://localhost:9393
    ```

# Part 2

1. Stop and remove containers using the following commands:
    ```
    docker stop csvserver
    docker rm csvserver
    ```

2. Created a `docker-compose.yaml` file.

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

2. Updated the `docker-compose.yaml` file.

3. Created a directory named `prometheus` in the solution directory. Inside this directory, created a file named `prometheus.yml` with the required content.

4. Verified Prometheus is accessible at `http://localhost:9090` on the host.

5. Run the containers using Docker Compose:
    ```
    docker-compose up -d
    ```

6. Typed `csvserver_records` in the query box of Prometheus, click on Execute, and switch to the Graph tab.

7. There is a straight line graph with the value 7.
