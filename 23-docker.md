# Docker
* **Dockerhub** is a public repository of docker images from which the docker containers can be built.
* **Dockerfile** is going to contain all the necessary steps to create our own custom image. Sample Dockerfile can be found [here](https://github.com/masif2002/fastapi/blob/master/Dockerfile).
    * All custom images need to start off with a base image
    * When an image is built from a Dockerfile, the result of each step in the Dockerfile is cached.
* .dockerignore file is created and the files to be excluded are added here line by line
## Docker compose
_docker-compose.yml_ file is created, which is going to contain the instructions for starting the docker containers. The docker containers are started with the `docker-compose up -d` command.
* There are two ways to represent an array in a  yaml file. For example, 
    * ```
      ports: [5432, 6748]
      ```  
    * ```
      ports:
        - 5432
        - 6748
      ```
* When it comes to data within a container, the data doesn't persist as the container goes down. So, to not lose the data we need to create something called a **_volume_** in docker to store the data.
* As the changes we make locally will not be reflected once the container is up and running, we create a so called _bind mount_ to do so.  
  * When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine.
  * So when you make changes locally, it is also done to the files in the container aswell
  ```
  volumes:
  - ./:/usr/src/app/:ro
  ```
  * Here, the current directory in the local machine is referenced to the `/usr/src/app/` path in the container
  * `ro` refers to read-only. So the files in the container cannot be changed explicitly. 

> Sample docker-compose file can be found [here](https://github.com/masif2002/fastapi/blob/master/docker-compose-dev.yml).  
## Docker commands
```
docker build -t <image_name> <Dockerfile_path>
```
* This command is used to build docker images
* If the Dockerfile is placed in the root directory of the project, we can specify only a `.` after the name of the image
---
```
docker image ls
```
* This command displays the list of docker images
---
```
docker-compose up -d
```
* This command is used to start the docker containers according to the instructions given in the *docker-compose.yml* file
  * If you have a custom file name you need to add a `-f` flag and specify the filename of the docker-compose file.
* The `-d` flag is used to run the command in background (d for detach)
* The `--build` flag can be added if we need to recreate the image before creating the container, if any changes has been made.
---
```
docker-compose down
```
* This command is used to stop the running docker containers
---
```
docker ps
```
* Displays the list of running containers
* Adding a `-a` flag will display all the containers
---
```
docker logs <container_name>
```
* Displays the logs for the specific container
---
```
docker exec --interactive <container_name> bash
```
* This command opens up the bash terminal inside the specified container.
* However `docker exec` is also used to execute any commands in a running container.
---
```
docker push <image_name>
```
* This command is used to push the docker image onto DockerHub
* However, there are three preqrequisites before the command can run
  * A repository with the given name must already exist in DockerHub
  * You need to be logged in from the docker CLI
  * The name of the image must be in the specific format
    ```
    <dockerhub_username>/<image_name>
    ```
---
```
docker login
```
* Used to login into Dockerhub
---
```
docker image tag <existing_image_name> <name_of_new_image>
```
* A new image with a new name is created from an existing image.  
  * Basically used for renaming the image if you're dumb enough to not think of a proper name while creating the image.
