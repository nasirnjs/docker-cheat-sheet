# 🚀Docker Cheat Sheet/CLI Commands🚀

## Let’s try some basic command 

1. This command is used to check the installed Docker version on your system.\
`sudo docker version`
2. Display system-wide information.\
`sudo docker info`
3. This command checks the status of the Docker service using systemd.\
`sudo systemctl status docker`
4. This command displays a list of currently running Docker containers.\
`sudo docker ps`
5. This command shows all containers, including those that are not currently running.\
`sudo docker ps -a`
6. This command lists all the Docker images that are available on your system.\
`sudo docker images`
7. This command pulls the latest version of the Nginx.\
`sudo docker run nginx`
8. The correct command to list running containers.\
`docker container list`\
*Additionally, using **sudo** might be necessary depending on your system's configuration.*
 ---

## Managing Docker Services and Sockets with Systemd

Docker service can still be activated through the Docker socket (`docker.socket`) even if you stop the Docker service (`docker.service`). The Docker socket allows communication with the Docker daemon and is used for Docker API access.

1. This command checks the status of the Docker service.\
`sudo systemctl status docker`
2. This command stops the Docker service.\
`sudo systemctl stop docker`
3. This command checks the status of the Docker service again.\
`sudo systemctl status docker`
4. This command stops the Docker socket.\
`sudo systemctl stop docker.socket`
5. This command checks the status of the Docker service once again.\
`sudo systemctl status docker`
---

## The lifecycle of a Docker container
The lifecycle of a Docker container involves creation, running, stopping, and removal. Containers are created from Docker images, run as isolated instances, can be stopped or paused, and can be removed when no longer needed.

<img src="image/Container_life.png" alt="Lifecycle of Docker Container" width="800"/>

Image: Lifecycle of a Docker container

**Example**

1. docker create:\
Purpose: Creates a new container but does not start it.\
Example:\
Creates a new container named "my-container" using the Nginx image but does not start it.\
`docker create --name my-container nginx`

2. docker start:\
Purpose: Starts one or more stopped containers.\
Example:\
Starts the container named "my-container" that was previously created but stopped.\
`docker start my-container`

3. docker run:\
Purpose: Creates and starts a new container in a single command.\
Example:\
This command combines the process of creating and starting a container named "my-container" using the Nginx image in a single step.\
`docker run --name my-container nginx`

4. docker pause:\
Purpose: Temporarily pause the processes in the running container.\
Example:
    ```bash
    docker pause my-container
    docker ps --filter "name=my-container"
    docker unpause my-container
    docker ps --filter "name=my-container"
    ```
5. docker stop:\
Purpose: The docker stop command is used to stop a running container.\
Example:\
This command stop a container named "my-container".\
`docker stop my-container`

6. docker rm:\
Purpose: The docker rm command is used to remove a container.\
Example:\
Want to remove container named "my-container".\
`docker rm my-container`

---

## Why a Docker container exits!!!
Docker containers are designed to run a specific command or process and exit when that command or process completes. The default behavior is to start the specified command or process inside the container and stop when that command or process finishes execution.\
Docker containers run as long as the process inside the container is active.\
Example:

Default Shell Behavior (Container Stops Immediately).\
`docker run busybox`

Run an Interactive Terminal (Container Remains Running).\
`docker run -it busybox`

Run a Command to Keep Container Running (e.g., Sleep). In the third example, the sleep 3600 command will keep the container running for 3600 seconds (1 hour), and then it will exit.\
`docker run busybox sleep 3600`

---
## Docker Container Lifecycle

Run a container from an image.\
`docker run nginx`

List running containers.\
`docker ps`

List all containers (running and stopped).\
`docker ps -a`

Run a command inside a running container.\
`docker exec my-nginx ls /usr/share/nginx/html`

Display detailed information about a container.\
`docker inspect my-nginx`

Stop a running container via name or ID.\
`docker stop my-nginx`

stop all running container.\
`docker container stop $(docker container ps -q)`

Start a stopped container vi name or ID.\
`docker start 24952546f818`

Restart a container.\
`docker restart 24952546f818`

Restart after 30 second.\
`docker restart -t 30 6d144a1d546b` 

Remove a stopped container.\
`docker rm 24952546f818`

Remove multiple containers.\
`docker rm 24952546f818 6d144a1d546b`

Remove all containers.\
`docker rm $(docker ps -aq)`

Temporary pause a Docker container.\
`docker pause  24952546f818`

Unpause a Container.\
`docker unpause 24952546f818`

## Container Naming and Identification:
Run a container with Name.\
`docker run --name my-container nginx`

Rename a container.\
`docker rename my-nginx new-nginx`

Get the container ID by its name.\
`docker inspect --format='{{.Id}}' nginx`


## Container Logs and Stats:
Display the logs of a container.\
`docker logs <containerID>`

Display a live stream of container resource usage statistics.\
`docker stats <containerID>`

Check specific container log runtime.\
`docker exec 266 cat /var/log/nginx/access.log`

## Container Interaction:
Attach to a running container (not recommended for long-term use).\
`docker attach <containerID>`

Run an interactive shell inside a container.\
`docker container exec -it b71f15d33b60 /bin/bash`\
`docker container run -it ubuntu`

Check container host Name.\
`docker container exec b71f15d hostname`	

Single container resources consumptions.\
`docker container top nginx 5ac4`

Multiple containers stats by name and ID.\
`docker container stats nginx 5ac4`	

Inspecting Container Information.\
`docker inspect <container_id_or_name>`

Copy from container to host.\
`docker cp <container_id_or_name>:<container_path> <host_path>`

Copy from host to container.\
`docker cp <host_path> <container_id_or_name>:<container_path>`

Remove all stopped containers.\
`docker container prune`

## Build a simple Docker Image

**Here's a basic Dockerfile for a Python "Hello, World!" application.**

**Steps 1** Create a `app.py` file and add print("Hello, World!")

`echo "print('Hello, World!')" > app.py`

**Steps 2** Create a `Dockerfile` and write Dockerize Python `Hello, World!` application.
```bash
# Use an official Python runtime as a parent image
FROM python:3.8-slim
# Set the working directory in the container
WORKDIR /app
# Copy the current directory contents into the container at /app
COPY . /app
# Run app.py when the container launches
CMD ["python", "app.py"]
```
**Steps 3** Build and run docker image.\
`docker build -t nasirnjs:hello-python:001`\
`docker run nasirnjs:hello-python:001`

## Docker Images and Tag:
List of all image.\
`docker image ls`

Pull an image from a registry.\
`docker pull nginx`

Show detailed information about an image.\
`docker image inspect nginx`

<img src="image/docker-image-tag.png" alt="Docker Image Taging explaination" width="800"/>

Tag the existing Image with the New Name.\
`docker tag old-image:old-tag new-image:new-tag`

Remove an image.\
`docker image rm my-image:tag`

Remove all unused images.\
`docker image prune -a`

Remove all images.\
`docker rmi $(docker images -q)`

## Authenticating to Registries
Docker and containerization, a registry is a service that stores and distributes Docker images. Docker images can be stored in public or private registries.\
Here's an overview of public and private registries:

**Public Registry**
- Definition: Public storage service for Docker images, accessible to anyone.
- Example: Docker Hub (hub.docker.com).
- Use Cases: Sharing open-source or community-based images.
  
**Private Registry**
- Definition: Secure storage service for Docker images, requires authentication.
- Example: Self-hosted or third-party private registries.
- Use Cases: Storing proprietary or sensitive images, access control.

Log in to Docker Hub.\
`docker login`

Check local Docker images list.\
`docker image list`

Push Docker Image to Dockerhub Private Repo.\
`docker push nasirnjs:hello-python:001`

Log in to a Private Registry.\
`docker login registry.example.com`

Docker CLI configuration settings, including authentication credentials for Docker registries.\
`cat ~/.docker/config.json `

 Display information about disk usage related to Docker.\
 `docker system df`


## Docker Image Layer
In Docker, images are composed of multiple layers. A docker container image is created using a dockerfile. Every line in a dockerfile will create a layer.\
If you make changes to your Dockerfile and rebuild the image, Docker can reuse cached layers to speed up the process, only rebuilding the layers affected by the changes.  Caching plays a significant role in optimizing the build process.\
Let's explore both concepts with examples:

Build Docker Image form [Here](https://github.com/nasirnjs/docker-static-site)

list of `nginx` image layer.\
`docker image inspect nginx -f '{{.RootFS.Layers}}' | awk -F' ' '{for (i=1; i<=NF; i++) print $i}'`

**Install Dive**
The dive is a command line tool for analyzing a Docker image. This tool shows image contents broken down by layer. It can used to explore image structure in order to minimize size of Docker image. [Reference](https://github.com/wagoodman/dive)

Install dive on Ubuntu
`sudo snap install dive`

Uninstall dive
`sudo apt purge --autoremove -y dive`


## Publishing Ports:

Publishing a Port.\
`docker run -d -p <host_port>:<container_port> --name my_container my_image`

Publish all exposed ports to random ports on the host.\
`docker run -d -P --name web_server nginx`

To see the actual mappings, you can use.\
`docker port my_container`


##  Difference between CMD vs ENTRYPOINT Docker!

In Docker, both `CMD` and `ENTRYPOINT` are instructions used to specify what command should be run when a container is started.\
You can write Docker CMD/ENTRYPOINT instructions in both forms:

- CMD echo "Hello World" (shell form)
- CMD ["echo", "Hello World"] (exec form)
- ENTRYPOINT echo "Hello World" (shell form)
- ENTRYPOINT ["echo", "Hello World"] (exec form)

**Docker CMD**
- Whenever we want to override executable while running the container, use `CMD`.
- We can override the value with a command-line argument.
- We can multiple CMD in a single docker file but only one will be executable while the container start.

Let’s see an example.\
`vi Dockerfile`
```bash
FROM ubuntu
RUN apt-get update
CMD ["echo", "Hello World"]
```
Build and run the docker file.\
`docker build -t ubuntu-test .`\
`docker run -it ubuntu-test`

*Here, we have passed as parameter Hello World for CMD that prints after container start. Here, we overrode the default value with Hi!*
`$ docker run -it ubuntu-test echo "Hi!"`

*In a Dockerfile, only the last CMD instruction is effective. If there are multiple CMD instructions, the last one will override the previous ones.*

**Docker Entrypoint**
- Just like with CMD, you need to specify a command and parameters.
- You cannot override the ENTRYPOINT instruction by adding command-line parameters to the docker run command.

We will also see an example.\
`vi Dockerfile`
```bash
FROM ubuntu
RUN apt-get update
ENTRYPOINT ["echo", "Hello Google"]
```
Build and run the docker file.\
`docker build -t ubuntu-test .`\
`docker run -it ubuntu-test`

It worked the same as CMD but when we have passed parameters will be see difference.\
`$ docker run -it ubuntu-test:latest "Hello from AWS"`

*we have passed parameters but the executable hasn’t overridden and also added a new parameter with the old parameter.*

**Now, let’s see how to use CMD and Entrypoint together.**

Modify our existing Dockerfile so it includes both `CMD & ENTRYPOINT`instructions.
```bash
FROM ubuntu
RUN apt-get update
ENTRYPOINT ["echo", "Hello"]
CMD ["Google World"]
```
Build a new image from the modified Dockerfile and run a container.\
`docker build -t ubuntu-test .`
`docker run -it ubuntu-test:latest`

*It will print the message `Hello Google World` message*

Let's pass parameters to the docker run command.\
`docker run -it ubuntu-test  Bangladesh`

*The output has now changed to `Hello Bangladesh`*\

**Conclusion**
- Whenever there is a chance of overriding executable while running the container using CMD otherwise uses ENTRYPOINT.
- Sometimes we don’t have to override the executable all we want to run the container, in that case, ENTRYPOINT is the best use case.
- If ENTRYPOINT is used for the executable, we can use CMD to pass default parameters. In that case, we use both together.


## Don,t Ignore .dockerignore
The ``.dockerignore` file is used by Docker to specify files and directories that should be excluded (ignored) when building a Docker image. It works similarly to the more widely known `.gitignore` file used by Git to specify files and directories that should be ignored when tracking changes.

Here's how it works:

- Default Behavior: By default, Docker includes all files and directories in the build context when building an image.
- Use of .dockerignore: If a file named .dockerignore is present in the build context, Docker reads it to determine which files and directories should be excluded from the build context.
- Exclusion Rules: The .dockerignore file follows the same rules as .gitignore. You can use wildcards and patterns to specify files and directories to be excluded.

For example, a .dockerignore file might look like this:
```
# .dockerignore

# Exclude the Dockerfile
Dockerfile

# Exclude the README.md file
README.md

# Exclude the .env file
.env

# Exclude the .git file
.git
```
Complete Example is [Here](https://github.com/nasirnjs/docker-static-site)


## docker volume:
docker cp



## Docker Restart Policy:



## Container Cleanup:
docker container prune: Remove all stopped containers.
docker container stop $(docker container ps -q): Stop all running containers.
docker container rm $(docker ps -aq): Remove all containers.


[Ref](https://docs.docker.com/engine/reference/run/)