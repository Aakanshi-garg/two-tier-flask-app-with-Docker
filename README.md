
# Two-tier Flask app with MySQL Docker Setup
This project demonstrates a two-tier architecture application using Flask (frontend/backend) and MySQL (database), containerized using Docker.
The aim is to deploy and run both tiers in isolated containers connected through a Docker network.

<img width="1165" height="293" alt="Capture" src="https://github.com/user-attachments/assets/cb6cb32e-aaf9-4147-86a0-2d14b8b7afce" />

## Prerequisites
Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup without Docker-Compose

1. Clone this repository (if you haven't already):
```
git clone https://github.com/your-username/your-repo-name.git
```
2. Navigate to the project directory:
``` 
cd your-repo-name
```
3. create a Dockerfile:
```
vim Dockerfile 
```
4. Create the docker image from the dockerfile:
```
docker build -t two-tier-app . 
```
5. Make sure you have created a network using the command:
```
docker network create two-tier -d bridge
```
6. Attach both the continers using the  same docker network, so that they can communicate with each other:
(i) MySQL Container:
```
docker run -d --name mysql --network two-tier -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=db mysql
```
(ii) Flask(Backend) Container:
```
docker run -d -p 80:5000 --name flask-app --network two-tier -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=db -e MYSQL_HOST=mysql -e MYSQL_USER=root two-tier-app 
```
7. Acccess the app:
Once both containers are running, open your browser and visit:
- **EC2 Instance-** http://<ec2-public-ip>:80
- **Local Machine-** http://localhost

### Note
- Make sure to replace placeholders (e.g., your_username, your_password, your_database) with your actual MySQL configuration.

- If you encounter issues, check Docker logs and error messages for troubleshooting.
```
docker logs <container-id>
```
- To view running containers:
```
docker ps 
```
- Ensure both containers are part of the same Docker network.

### Cleanup

To stop and remove containers and network:
```
docker stop flask-app mysql
docker rm flask-app mysql
docker network rm two-tier
```
## Setup with Docker-Compose

1. Clone this repository (if you haven't already):
```
git clone https://github.com/your-username/your-repo-name.git
```
2. Navigate to the project directory:
``` 
cd your-repo-name
```
3. create a Dockerfile:
```
vim Dockerfile 
```
4. Create the compose yaml file:
```
vim docker-compose.yaml
```
5. Start the containers using Docker Compose:
```
docker compose up 
```
7. Acccess the app:
Once both containers are running, open your browser and visit:
- **EC2 Instance-** http://<ec2-public-ip>:80
- **Local Machine-** http://localhost

### Note
- Ensure that the docker compose is installed already, if not then follow the command:
```
sudo apt-get install docker-compose-v2
```

### Cleanup
To stop and remove the docker containers and network:
```
docker compose down
docker network rm two-tier
```
### Recommendations

- **Environment Variables:** Consider using environment variables for sensitive information like database credentials.

- **Persistent Storage:** Implement Docker volumes to persist MySQL data beyond container lifecycles.
  
## Why Docker compose?
Using Docker Compose simplifies the management of multi-container applications. Instead of running and connecting each container manually, Compose allows you to define all services, networks, and environment variables in a single YAML file. This makes setup, scaling, and teardown much easier with just one command (docker compose up). It ensures consistency across environments, reduces manual errors, and helps in managing complex applications efficiently. Overall, Docker Compose provides a faster, more organized, and reproducible way to deploy multi-container setups like this Flask–MySQL project.

