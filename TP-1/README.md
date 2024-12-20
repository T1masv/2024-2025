# TP-1 Build and Deploy pipeline #

The main objective of this work is to know how to build an application based on a Docker container, deploy it and manage it in its execution environment using Jenkins and learn about collaborative working using Git.

## Requirements ##

* Jenkins (jenkins/jenkins:lts-jdk17) with Docker Pipeline plugin.

## Instructions for carrying out the work ##

### 1 - Retrieve the source code repository locally on your virtual machine with the command ###

```console
git clone https://github.com/ST2DCE/2024-2025.git
```

### 2 - Prepare Jenkins ###

* Add/Register credentials for git (source code) repository and docker registry.
  * Git : <https://github.com/ST2DCE/2024-2025.git>
    * username: ST2DCE
    * password: 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
  * Docker Hub Repos : efrei2023/backend, efrei2023/frontend
    * username: efrei2023
    * password: efrei2023
* Create a network for docker infrastructure:

   ```console
     docker network create --driver bridge efrei
   ```

### Git repository structure ###

* backend : A PostgreSQL database consisting of one table with some students (see backend/database.sql).

* frontend : The webapp that retrieves the books from the database and shows them in a HTML page. This webapp is build using Flask (a Python Microframework).

## Work to be done ##

### 1. Building the backend and frontend ###

* Build by using a Dockerfile
  * Analyze the contents of the 'backend/Dockerfile' and 'frontend/Dockerfile files

  * Go to the 'backend' directory and type the command :

   ```console
     docker build -t backend-$NAME:YEAR_OF_BIRTH .
   ```

  * Go to the 'frontend' directory and type the command :

   ```console
     docker build -t frontend-$NAME:YEAR_OF_BIRTH .
   ```

### 2. Running the backend and frontend ###

* Run the 'backend' container with the following  command :

   ```console
     docker run -d --net=efrei -e POSTGRES_PASSWORD=postgres --name backend backend-$NAME:YEAR_OF_BIRTH
   ```

* Run the 'frontend' container with the following  command :

   ```console
     docker run -d --net=efrei  -p 8081:80 -e POSTGRES_PASSWORD=postgres --name frontend frontend-$NAME:YEAR_OF_BIRTH
   ```

To make sure your container is up and running, check the logs with the command:

   ```console
     docker logs frontend
     docker logs backend
   ```

Open your browser and go to <http://localhost:8081>. You will now see the books from the database displayed by the webapp.

### 3. Automation with jenkins : use your virtual machine as jenkins slave ###

Repeat steps 1 and 2 using a Jenkins pipeline. You can use the example file: Jenkins.build
Modify the database to display only your first and last name.

### 4. Deployment orchestration : docker-compose ###

To run the containers you had to execute commands. It would be easier if we only had to execute one command. We can achieve this by using Docker Compose.

Analyse the docker-compose.yml file to run the webapp (frontend) and the database (backend).

* Run `docker-compose build' to build the containers specified in the docker-compose file.

* Run docker-compose up to start the containers (add -d to run them in the background. Then run docker-compose stop when done.)

* Browse to <http://localhost:8081> to see the response from the web app.
