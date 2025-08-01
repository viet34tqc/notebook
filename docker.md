# Docker

## Reference

- <https://www.youtube.com/watch?v=3c-iBn73dDE&t=1s> (must watch for beginner)
- <https://www.youtube.com/watch?v=rgeQl_9Zw3I>
- <https://www.youtube.com/watch?v=pTFZFxd4hOI>
- <https://www.youtube.com/watch?v=pg19Z8LL06w>
- <https://devdojo.com/bobbyiliev/free-introduction-to-docker-ebook>
- <https://www.freecodecamp.org/news/how-to-remove-all-docker-images-a-docker-cleanup-guide/>

## Best practice

- <https://dev.to/idsulik/dockerfile-best-practices-how-to-create-efficient-containers-4p8o>

## Tại sao phải sử dụng docker

<https://dev.to/techworld_with_nana/what-problems-does-docker-really-solve-496a>

Giải quyết vấn đề triển khai code. Ví dụ có 2 máy khác nhau về OS, hardware... => việc setup (setup webserver, database, packages version...) để app chạy cho từng môi trường tiêu tốn nhiều thời gian. Kể cả các môi trường có giống nhau đi chăng nữa thì việc setup lại từ đầu cũng không hiệu quả.

Docker sẽ đóng gói ứng dụng vào 1 container, các môi trường có thể dùng chung container hoặc có container riêng của môi trường đó, khi deploy ta chỉ cần run docker container đấy là mọi thứ sẽ được setup chứ không còn phải setup từ đầu nữa 

## Container

Tưởng tượng nó như 1 cái hộp, trong đấy chứa tất cả các hệ điều hành, database, và cả code của mình. Mình có thể dễ dàng quản lý, di chuyển, share cái container này.

Về bản chất, *container* giống với *virtual machine* ở chỗ là có có thể cài app, OS tách biệt với máy tính đang chạy. Tuy nhiên, container có điểm khác so với vm đó là nó dùng chung OS kernal với hệ điều hành hiện tại của máy còn VM sẽ dùng OS kernal riêng. Do đó, container sẽ

- Nhẹ hơn, tốn ít tài nguyên hơn
- Khởi động nhanh hơn

*Container* được tạo ra từ *Docker Image*.

Làm thế nào để access Docker container?
Docker container được chạy trong 1 isolated Docker network, nên ta phải expose container ra ngoài máy local (host). Quá trình này gọi là `port binding`

Commands:

- `docker ps`: list các container đang chạy
- `docker ps -a`: list các container đang chạy và đang bị stop
- `docker rm -f <id>`: remove container

## Image

Là 1 snapshot (giống file ghost) mà khi run snapshot này sẽ cho ra *Container*, hay nói cách khác là tái tạo lại môi trường. Có thể hiểu Docker Image như là class và container như là instance của class đó. Vậy Image này lấy từ đâu?

Docker image sẽ được lấy lấy từ `docker registries`, e.g DockerHub. Nó giống như kiểu github nhưng chỉ chứa các docker images (public và private)

Ngoài các image có sẵn, ta có thể tự tạo custom image bằng `Dockerfile`

## Dockerfile

Chứa hướng dẫn để setup môi trường và build ra *Docker Image*

Tại sao cần dùng Dockerfile: Sau khi dev xong, chúng ta mong muốn deploy app lên server. Lúc này ta cần tạo custom image để deploy app như là docker container. Để tạo custom image, ta sử dụng Dockerfile

`Dockerfile => Docker image => Docker container`

Commands:

- `FROM`: set the base image. In the js app, this shoud be nodejs
- `WORKDIR`: set the working directory.
- `COPY {files_in_host} {dest_folder_in_container}`: copy file from host (local machine) into working directory of container
- `RUN`: Run a command during the build process of docker image
- `CMD`: Run a command when container is started from the image. There is only one `CMD` in a Dockerfile

```
FROM node:18-alpine       // Download node image về

WORKDIR /app              // CD đến thư mục app

COPY package*.json ./     // Copy các file package*.json vào thư mục app. Nếu dùng yarn thì phải COPY yarn.lock

RUN npm install           // Nếu dùng yarn thì chạy yarn install

COPY . .                  // Copy tất cả code trong repo vào thư mục app. Nhớ tạo file .dockerignore để ignore node_module

EXPOSE 8080               // Optional. It 's kind of a documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. To publish the port when running the container, use the -p flag on docker run to publish

CMD [ "npm", "start" ]    // Khi setup xong thì chạy lệnh
```

Tạo 1 file `.dockerignore` để ignore `node_module`, file markdown...

```
node_modules
Dockerfile
docker-compose.yml
test
dist
.gitignore
.prettierrc
.git
*.md
```

## `COPY`

The copying source is the build content, which is the directory you specify when you run docker build or the `build/context` field in docker-compose file.

- `RUN` runs once during docker build, nothing happens at runtime. If you have `RUN pnpm dev` inside Dockerfile, it will run forever and you cannot start the container
- `CMD` doesn't run during build, but runs when the container starts.

### `ARG`, `ENV` in Dockerfile

[https://vsupalov.com/docker-arg-env-variable-guide/](https://vsupalov.com/docker-arg-env-variable-guide/)

- `ARG` 
  - Defines build-time variables that you can pass to the Docker build process.
  - is only available during the build of a Docker image
- `ENV` 
  - Sets environment variables that are available to the running container.
  - is available during the build of a Docker image and also when we run the container. So its variable in build process and running container can be different.
  
**Why we use `ENV` in Dockerfile**

By default, you don't need to declare `ENV` variables in Dockerfile cause it's only being used when running container. However, setting `ENV` in Dockerfile can have some benefits:

- provide default values for `ENV` during build process, using both of `ARG` and `ENV`. By this way, you can ensure that an `ENV` value is not empty when no values are provided during runtime.

```bash
ARG name
ENV env_name $name
```

- you want to do something in a shell script defined in Dockerfile, like all your command to run in Docker is actually or is started by a shell script.

```bash
# Dockerfile
FROM alpine:latest

ENV MY_VAR John

COPY setup.sh .

RUN chmod +x /setup.sh
CMD ["/setup.sh"]

# setup.sh
#!/bin/sh
echo "The value of MY_VAR is: $MY_VAR"
```

When build, it will print 'The value of MY_VAR is: John'

Remember that this wont work if you are using multi-stage build, like this

```bash
FROM alpine:latest

ENV MY_VAR John

FROM ubuntu
RUN apt-get update
RUN apt-get install nginx -y

COPY setup.sh .

RUN chmod +x /setup.sh
CMD ["/setup.sh"]
```

### How to build and run a Dockerfile

- First you need to build the image by running: `docker build -t ${docker_image_name} .` (`.` means the current directory). To find your images, run `docker images`
  - `-t` or `--tag`: setting name for the docker image. It could be `my_app:1.0` or smt else.
  - `.`: the current directory
- Then run your image to build container: `docker run -dp ${HOST_PORT}:${CONTAINER_PORT} ${CONTAINER_NAME}`: 
  - `-d`: run in the background (optional)
  - `-p`:  means port mapping, port on host machine is map to port in the container. Then all the requests that are made to the host port can be redirected into the Docker container.
  - `HOST` is the operating system in which the Docker client is running. `${HOST_PORT}` is the port that help us to connect to the `${CONTAINER_PORT}`. When we access the `${HOST_PORT}`, it will be redirected to the `${CONTAINER_PORT}`
  - `${CONTAINER_PORT}` is the port that the container exposes
  
### Passing args value when build image

`docker build --build-arg VERSION=2.0 -t myimage .`

### Passing environment variables'value when running container

- Using `-e`: `docker run -e MY_VAR=my_value my_image`
- Using `.env` file: `docker run --env-file env.list my_image`

### Passing environment variables'value when running docker compose

- Define `ENV` in Dockerfile.
- Using `environment` attribute:

You can define the value directly

```bash
services:
  webapp:
    environment:
      POSTGRES_USER: postgres
```

Or you can use interpolation to dynamically passing variable when you run your container using docker compose. This value is from a '.env' file in the container directory or from your host machine that runs docker or when you run cli: `docker compose run -e POSTGRES_USER=abc webapp`

```bash
environment:
  POSTGRES_USER: ${POSTGRES_USER}
```

- Using `env_file` attribute to set multiple variables

```bash
services:
  webapp:
    env_file: ".env"
```

### Dockerfile layer caching

Every command in Dockerfile is a layer

When build an image, the builder reruns all the layers and attempts to reuse layers from earlier builds. If a layer of an image is unchanged, then the builder picks it up from the build cache. If a layer has changed since the last build, that layer, and all layers that follow, must be rebuilt. 

Therefore, we typically copy the package*.json file first and run the installation. Then we copy the remaining files from 'src' folder. By this way, we can utilize the cache mechanism because unlike 'src' folder, packages change less frequently and we can cached the process of installing them.

```
FROM node:18-alpine    
WORKDIR /app           

COPY package*.json ./  

RUN npm install        // We copy and install

COPY . .               

// Other commands...
```

## Docker networking

### How host connect to container, port mapping, host port, container port

- Host machine connects to container

By default, Docker uses `bridge` network, that means the port in container isn't exposed to the host and we cannot access the container using container's port and host's domain or ip. For example, let's say the container's port is 80 and domain of host is localhost, we cannot connect to the container via 'http://localhost:80'. 

Of course, we can use `host` network to make the container port the same as host port, then we can access the container via their port directly without mapping. However, using port mapping brings offers many benefits:

- Isolation: By nature, containers are designed to be isolated from the host system. If you want to access a web server inside a Docker container from your browser, you should map the container's web server port to a port on the host
- Avoid port conflicts: If the default port used by the service inside the container is already in use on the host, you can map it to a different port on the host.
- Run multiple container that has the same container port: we might have 2 web services that runs on port 3000. To access them all, we need to map their port to 2 different host port

### How containers connects to each other

Unlike the situation where host machine connects to container, in Docker environment, containers use **container name** to connect to each other directly. Docker sets up a internal DNS system where service names in the `docker-compose` file can be used as hostname

Let's say we have 3 services: client (port: 5173), server (port: 3000) and redis (port: 6379). Client needs to connect to server and server needs to connect to redis. 
The client-side code runs in the browser on host machine, not inside a Docker container. So when it makes requests to `localhost:3000`, it's using your host machine's networking, not Docker's internal networking. Client can also connect to redis using 'localhost:6379' if it wants

However, 'server' and 'redis' are isolated and can only communicate with each other using their service names (like `server:3000` or `redis:6379` within the Docker network).

Here's a simplified view of what's happening:

- Browser on Host runs `localhost:3000` -> Docker Port Mapping -> Server Container
- Server Container runs `redis:6379` -> Redis Container (within Docker network)

If you were running your client in a Docker container (instead of serving it via nginx and accessing it through the browser), and you wanted it to connect to the server container, you would indeed use server:3000 in your client code.

**NOTE**: if service A connects to multiple ports in service B, you need to expose all of them. For example, you backend service has 2 port: 3000 for Rest API and 8000 for Websocket, you need to expose both 3000 and 8000 in `docker-compose` file.

## Common commands

- `docker ps`: liệt kê danh sách container đang chạy
- `docker ps -a`: liệt kê danh sách container đang chạy và đang dừng
- `docker ps -a | grep <text>`: Search for text

- `docker start <container_id>`: start 1 container
- `docker stop <container_id>`: stop 1 container
- `docker inspect <container_id> | grep "IPAddress"`: tìm IP của container

- `docker pull <docker_image>`: pull docker image từ docker hub.
- `docker run <docker_image>`: tạo và start 1 container từ 1 docker image, nếu image này chưa có trên local thì nó sẽ được pull về từ docker hub.
- `docker run -p <host_port>:<container_port> <docker_image>`: tạo container từ 1 docker image, bind container đó với port của host (máy của mình). Điều này cho phép 2 container có cùng port nhưng vẫn có thể cùng chạy với 2 host_port khác nhau <https://youtu.be/3c-iBn73dDE?t=328-p <host_port>:<container_port> 7>
- `docker image ls`: liệt kê danh sách images
- `docker build -t <docker_image_name>` .: build image từ Dockerfile. Dấu chấm ở cuối để nói với Docker là chạy Dockerfile ở thư mục hiện tại

- `docker run -it ubuntu`
- `docker-compose up`: chạy tất cả các container
- `docker-compose up -d`: chạy ngầm tất cả các container (kể cả khi tắt terminal đi thì container không bị mất đi)

**Clean Docker storage**

- `docker system prune`.
- `docker system prune -a`: cleaner

We can replace `docker system` with `docker volume` or `docker container`, etc.

Update code

- Re-build lại docker image: phải xóa docker image cũ đi rồi build lại cái mới dùng lệnh `docker build -t` ở trên
- Stop và remove container hiện tại: dùng lệnh `docker rm -f <the-container-id>` hoặc dùng Dashboard

## Docker volume

To Synchronize files between host and docker container

Why: If your container has the feature to store user data in database, this data will be gone when you start another container from the same image. To make it persist, we use docker volume to store our data because docker volume is separated from docker container. This docker volume is store on host machine. If you are using Docker desktop, its location is `\\wsl$\docker-desktop-data\data\docker\volumes` (<https://stackoverflow.com/questions/43181654/locating-data-volumes-in-docker-desktop-windows>)

How:
We create a volume, then mount that volume to the directory where we store db in container. When we run container, do something and update db, our volume will be synchronized. So, if we create another container using this volume, the db still remains and is ready to use.

Commands: 

```bash
# First, create a volume
docker volume create todo-db

# Run the container using volume
# `todo-db` is the name of volume
# `etc/todos` is where container store the db, we set this location in the code
docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
```

## How Docker works

<https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc69f4fe6-2606-47c4-b093-3de6914d942b_1602x1536.jpeg>

There are 3 components in Docker architecture:

- Docker client
The docker client talks to the Docker daemon.

- Docker host
The Docker daemon listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.

- Docker registry
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use.

Let's take the 'docker run' command as an example.

- Docker pulls the image from the registry.
- Docker creates a new container.
- Docker allocates a read-write filesystem to the container.
- Docker creates a network interface to connect the container to the default network.
- Docker starts the container.

## Docker compose

Why:

- In practice, our app will have multiple containers, like: container for FE, container for BE, container for DB. We need these containers connect to each other.
- Each container requires a single Dockerfile => hard to maintain.

**Can we remove Dockerfile, and only use Docker compose**: maybe, but most of the time you need to have seperate `Dockerfile` and `docker-compose.yml` file

- Dockerfile: this is how to build my image
- Docker-compose: this is how to run my image(s)

An example of `docker-compose.yml`. Here we are not using a Dockerfile, all the configuration are in the `docker-compose`

```bash
# Declare container
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app # similar to Dockerfile, after container created, cd to `/app` folder then run command above
    volumes: 
      - ./:/app # Mount the current folder on host machine to the `/app` folder in the container. So, when we modify codebase, the app in container is synchronized
    environment: # environment variables
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql # /var/lib/mysql is where MySQL stores data
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

# Define a volume name
volumes:
  todo-mysql-data:

```

And here is how we use `Dockerfile` in `docker-compose` (this way is preferred). When we run `docker compose up`, we are actually building the image using `Dockerfile` first. It's the same as running `docker build`. Then we run this image with other params like `ports`, `environment`, just like running `docker run`

```bash
services:
  db:
    image: postgres:13
    restart: always
    ports:
      - 5434:5432
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123
      POSTGRES_DB: nest
    volumes:
      - postgres:/var/lib/postgresql/data
  app:
    depends_on:
      - db
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 3001:3000
    environment:
      - DATABASE_URL=postgresql://postgres:123@host.docker.internal:5434/nest?schema=public
      - JWT_SECRET=super-secret
    volumes:
      - ./src:/app/src
volumes:
  postgres:
    name: nest-prisma-docker-db
```

After create docker-compose file, run this command: `docker compose up -d`. This command Starts your services in detached mode (running in the background) and build the images if they don't exist. Otherwise, it uses existing images

You can also use `docker compose up -d --build` to forces a rebuild of the images regardless of whether an image already exists. It ensures that any changes in your Dockerfile or build context are incorporated into the new image.

Using --build is particularly useful when you've updated your code or Dockerfile and want to make sure your containers are running the latest version.

### Wait for db run first

App can run already while db needs time to set up => we need to wait for db run first before running app

```yml
# POSTGRES
dev-db:
    image: postgres:13
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready pg_isready -d nest -U postgres']
      interval: 10s
      timeout: 5s
      retries: 5

# MYSQL
mysql:
    image: mysql:8.0
    healthcheck:
      test: ['CMD', 'mysqladmin', 'ping', '-h', 'localhost', '-uroot', '-ppass']
      interval: 5s
      timeout: 5s
      retries: 20
```

## How to run something when container starts

<https://chatgpt.com/c/67c4353f-0ac0-8010-aaeb-f1bf584c28a7>

## CMD

- Define a simple default behavior like starting a server.
- Can overridden by `command:` in docker-compose or `docker run` CLI.

### ENTRYPOINT

The purpose of

```Dockerfile
ENTRYPOINT ["./entrypoint.sh"]
```

is to set entrypoint.sh as the entry point for the container. This means when the container starts, it will always execute `entrypoint.sh` first before running any command specified in CMD or provided at runtime.

Why Use ENTRYPOINT?

1. Ensures a Script Runs Before Anything Else
  - `entrypoint.sh` can be used to prepare the environment before the main process starts, such as:
    - Replacing environment variables in configuration files
    - Running database migrations
    - Checking for required services (e.g., waiting for a database to be ready)
2. Allows Custom Commands While Keeping the Default Behavior
If you define:

```
ENTRYPOINT ["./entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
```

  - The container will always execute entrypoint.sh first.
  - After entrypoint.sh finishes, it will run nginx -g 'daemon off;' (from CMD).
  - You can override CMD at runtime, but ENTRYPOINT will still execute first.
3. Makes your commands more clean and can be reuseable across container

ENTRYPOINT can not be overridden by CMD, but can be overridden by `entrypoint:` or `--entrypoint`. 

### `command:` in docker-compose.yml

```
services:
  app:
    command: ["npm", "test"]
```

- Overrides CMD in Dockerfile at runtime.

### `--entrypoint` in `docker run` (CLI only)

`docker run --entrypoint "sh" my-image -c "echo Hello"`

- This has the highest precedence.
  
## Multi-statge build

<https://adventofdocker.com/posts/day-13-multistage-builds/>

```bash
# Build stage - compiles the application
FROM node:lts AS base
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Runtime stage - serves the static files
FROM nginx:mainline-alpine-slim AS runtime
COPY --from=base ./app/dist /usr/share/nginx/html # Copy app/dist folder from base stage into /usr/share/nginx/html
EXPOSE 80
```

- The build stage uses Node.js to install dependencies and build the application
- The runtime stage uses a lightweight nginx image to serve only the built static files

Another example:

```bash
# Build stage
FROM golang:latest AS builder

WORKDIR /app
COPY . .

RUN go build -o main . # Build from current directory which is /app and the output file's name is main

# Final stage
FROM alpine:3.18 # We are using the Linux alpine image which is much smaller

WORKDIR /app
COPY --from=builder /app/main .

EXPOSE 8080
CMD ["./main"]
```

Benefits of Multistage Builds:

- Smaller Image Size: Final images contain only what's necessary to run the application. It can use smaller image to run the app
- Better Security: Fewer components mean a smaller attack surface
- Faster Deployments: Smaller images are faster to push and pull
- Clean Separation: Build-time dependencies are completely separated from runtime

## Best practices for performance and security

- Optimize docker image size: <https://github.com/webuild-community/advent-of-frontend/blob/main/2022/day-23.md>
- Set memory limit and cpu limits: https://adventofdocker.com/posts/day-19-limiting-resources/
  - Always set memory limits in production
  - Use CPU limits when sharing hosts
  - Test your app with limits before deployment
- Set non-root user in the Dockerfile

```bash
# Create a new user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Switch to that user
USER appuser

# Make sure directories have correct permissions
RUN chown -R appuser /app
```

- Limit resources: Limit the impact of a DoS attack by setting resource limits, like memory and CPU <https://adventofdocker.com/posts/day-19-limiting-resources/>
