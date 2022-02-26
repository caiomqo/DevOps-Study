# 1 - Install Docker on Ubuntu

# 2 - Docker Commands:
	- docker pull            -> download a docker image (can be from docker hub)
	
	- docker run             -> run container (combines pull and start)
	- docker run -d redis    -> run container in detached mode

	- docker start <cont_id> -> start a container
	- docker stop  <cont_id> -> stop a container
	
	- docker ps              -> check running containers
	- docker ps -a           -> show all containers, running or not

	- docker images          -> check all docker images on my pc	

	- docker system prune    -> removes unused containers and images
	- docker system prune -a -> removes images without one container

	- docker logs <cont_id>  -> gets the logs
	
	- docker exec -it <name> /bin/bash
	- docker exec -it <name> /bin/sh

	- docker rm <id>         -> removes a container
	- docker rmi <id>        -> removes an image

## Notes:
- if you use docker run and image is not present, it will be pulled
- docker run -d -p6000:6379 --name <container_name> <image>:<version>
- docker network ls -> checks existing docker networks

## Creating a new Network:
docker network create mongo-network -> creates a network with name "mongo-network"


## Creating Mongo DB Container:
sudo docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo

## Creating Mongo Express Container:
	sudo docker run -d \
	> -p 8081:8081 \
	> -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
	> -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
	> --net mongo-network \
	> --name mongo-express \
	> -e ME_CONFIG_MONGODB_SERVER=mongodb \
	> mongo-express
