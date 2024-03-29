# Docker

## Reference

- <https://www.youtube.com/watch?v=3c-iBn73dDE&t=1s> (must watch for beginner)
- <https://www.youtube.com/watch?v=rgeQl_9Zw3I>
- <https://www.youtube.com/watch?v=pTFZFxd4hOI>
- <https://www.youtube.com/watch?v=pg19Z8LL06w>
- <https://devdojo.com/bobbyiliev/free-introduction-to-docker-ebook>
- <https://www.freecodecamp.org/news/how-to-remove-all-docker-images-a-docker-cleanup-guide/>

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
- `docker run -dp {HOST_PORT}:{CONTAINER_PORT} {name}:{tag}`: run image, create new container và expose container port to host, `{HOST_PORT}` tùy chọn nhưng nên giống với `{CONTAINER_PORT}`, `{CONTAINER_PORT}` xem từ `docker ps`. `{CONTAINER_PORT}` bắt buộc phải chính xác

## Image

Là 1 snapshot (giống file ghost) mà khi run snapshot này sẽ cho ra *Container*, hay nói cách khác là tái tạo lại môi trường. Có thể hiểu Docker Image như là class và container như là instance của class đó. Vậy Image này lấy từ đâu?

Docker image sẽ được lấy lấy từ `docker registries`, e.g DockerHub. Nó giống như kiểu github nhưng chỉ chứa các docker images (public và private)

Ngoài các image có sẵn, ta có thể tự tạo custom image bằng `Dockerfile`

Commands:

- `docker images`: liệt kê tất cả các images hiện có
- `docker run {name}:{tag}`: run 1 image với tag của nó, nếu nó không tìm thấy trong máy local, docker sẽ tự động pull về từ DockerHub
- `docker run -d {name}:{tag}`: `-d` means detach, which means docker runs in the background without block terminal
- `docker run -d --name {container_name} {name}:{tag}`: thêm tên cho container cho dễ nhớ

## Dockerfile

Chứa hướng dẫn để setup môi trường và build ra *Docker Image*

Tại sao cần dùng Dockerfile: Sau khi dev xong, chúng ta mong muốn deploy app lên server. Lúc này ta cần tạo custom image để deploy app như là docker container. Để tạo custom image, ta sử dụng Dockerfile

`Dockerfile => Docker image => Docker container`

Commands:

- `FROM`: khai báo rằng image này được tạo từ 1 parent image or base image. Với app js thì sẽ là node
- `COPY {files_in_host} {dest_folder_in_container}`: copy file từ host (local machine) vào container
- `WORKDIR`: working directory. Đây là thư mục để chạy các command bên dưới, thường là trùng với destination folder ở trong lệnh `COPY`. Khi khai báo `WORKDIR /app` là ta `cd` đến thư mục `app`
- `RUN`: chạy 1 command nào đó trong môi trường base image
- `CMD`: chạy command này khi Docker container start. Chỉ có 1 `CMD` trong 1 Dockerfile

```code
FROM node:18-alpine              // Download node image về

WORKDIR /app              // CD đến thư mục app

COPY package*.json ./     // Copy các file package*.json vào thư mục app. Nếu dùng yarn thì phải COPY yarn.lock

RUN npm install           // Nếu dùng yarn thì chạy yarn install

COPY . .                  // Copy tất cả code trong repo vào thư mục app. Nhớ tạo file .dockerignore để ignore node_module

EXPOSE 8080

CMD [ "npm", "start" ]    // Khi setup xong thì chạy lệnh
```

Tạo 1 file `.dockerignore` để ignore `node_module`

Để chạy Dockerfile, dùng lệnh: `docker build -t <docker_image_name> .`. `-t` hay `--tag` là đánh tag cho docker image
Docker image name có thể là my_app:1.0. Sau khi build xong chạy `docker run` như trên để tạo ra container

## Commands

- `docker ps`: liệt kê danh sách container đang chạy
- `docker ps -a`: liệt kê danh sách container đang chạy và đang dừng
- `docker ps -a | grep <text>`: Search for text

- `docker start <container_id>`: start 1 container
- `docker stop <container_id>`: stop 1 container
- `docker inspect <container_id> | grep "IPAddress"`: tìm IP của container

- `docker pull <docker_image>`: pull docker image từ docker hub.
- `docker run <docker_image>`: tạo và start 1 container từ 1 docker image, nếu image này chưa có trên local thì nó sẽ được pull về từ docker hub.
- `docker run <docker_image> -p<host_port>:<container_port>`: tạo container từ 1 docker image, bind container đó với port của host (máy của mình). Điều này cho phép 2 container có cùng port nhưng vẫn có thể cùng chạy với 2 host_port khác nhau <https://youtu.be/3c-iBn73dDE?t=3287>
- `docker image ls`: liệt kê danh sách images
- `docker build -t <docker_image_name>` .: build image từ Dockerfile. Dấu chấm ở cuối để nói với Docker là chạy Dockerfile ở thư mục hiện tại

- `docker run -it ubuntu`
- `docker-compose up`: chạy tất cả các container
- `docker-compose up -d`: chạy ngầm tất cả các container (kể cả khi tắt terminal đi thì container không bị mất đi)

Update code

- Re-build lại docker image: phải xóa docker image cũ đi rồi build lại cái mới dùng lệnh `docker build -t` ở trên
- Stop và remove container hiện tại: dùng lệnh `docker rm -f <the-container-id>` hoặc dùng Dashboard

## Docker volume

Why: If your container has the feature to store user data in database, this data will be gone when you start another container from the same image. To make it persist, we use docker volume to store our data because docker volume is separated from docker container. This docker volume is store on host file. If you are using Docker desktop, its location is `\\wsl$\docker-desktop-data\data\docker\volumes` (<https://stackoverflow.com/questions/43181654/locating-data-volumes-in-docker-desktop-windows>)

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

By using Docker compose, we set up all the docker containers in only one `.yml` file (set up name, volumes...)

An example of `docker-compose.yml`

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

After create docker-compose file, run this command: `docker compose up -d`

## Doker ARG, ENV and .env

[https://vsupalov.com/docker-arg-env-variable-guide/](https://vsupalov.com/docker-arg-env-variable-guide/)



## Optimize docker image size

<https://github.com/webuild-community/advent-of-frontend/blob/main/2022/day-23.md>

- Remove devDependencies bằng cách build multi-stage