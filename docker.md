# Docker

## Reference

<https://www.youtube.com/watch?v=3c-iBn73dDE&t=1s> (must watch for beginner)
<https://www.youtube.com/watch?v=pTFZFxd4hOI>
<https://www.youtube.com/watch?v=pg19Z8LL06w>
<https://devdojo.com/bobbyiliev/free-introduction-to-docker-ebook>
<https://www.freecodecamp.org/news/how-to-remove-all-docker-images-a-docker-cleanup-guide/>

## Tại sao phải sử dụng docker

<https://dev.to/techworld_with_nana/what-problems-does-docker-really-solve-496a>

Giải quyết vấn đề triển khai code. Ví dụ có 2 máy khác nhau về OS, hardware... => việc setup (setup webserver, database, packages version...) để app chạy cho từng môi trường tiêu tốn nhiều thời gian. Kể cả các môi trường có giống nhau đi chăng nữa thì việc setup lại từ đầu cũng không hiệu quả.

Docker sẽ đóng gói ứng dụng vào 1 container, các môi trường có thể dùng chung container hoặc có container riêng của môi trường đó, khi deploy ta chỉ cần run docker container đấy là mọi thứ sẽ được setup chứ không còn phải setup từ đầu nữa 

## Các thuật ngữ

### Container

Tưởng tượng nó như 1 cái hộp, trong đấy chứa tất cả các hệ điều hành, database, và cả code của mình. Mình có thể dễ dàng quản lý, di chuyển, share cái container này.

Về bản chất, *container* giống với *virtual machine* ở chỗ là có có thể cài app, OS tách biệt với máy tính đang chạy. Tuy nhiên, container có điểm khác so với vm đó là nó dùng chung OS kernal với hệ điều hành hiện tại của máy còn VM sẽ dùng OS kernal riêng. Do đó, container sẽ

- Nhẹ hơn, tốn ít tài nguyên hơn
- Khởi động nhanh hơn

*Container* được tạo ra từ *Docker Image*.

Làm thế nào để access Docker container?
Docker container được chạy trong 1 isolated Docker network, nên ta phải expose container ra ngoài máy local (host). Quá trình này gọi là `port binding`

Commands:

- `docker ps`: list các container đang chạy
- `docker ps -a`: list các container đang chạy và stop
- `docker run -d -p {HOST_PORT}:{CONTAINER_PORT} {name}:{tag}`: run image, create new container và expose container port to host, `{HOST_PORT}` tùy chọn nhưng nên giống với `{CONTAINER_PORT}`, `{CONTAINER_PORT}` xem từ `docker ps`. `{CONTAINER_PORT}` bắt buộc phải chính xác

### Image

Là 1 snapshot (giống file ghost) mà khi run snapshot này sẽ cho ra *Container*, hay nói cách khác là tái tạo lại môi trường. Có thể hiểu Docker Image như là class và container như là instance của class đó. Vậy Image này lấy từ đâu?

Docker image sẽ được lấy lấy từ `docker registries`, e.g DockerHub. Nó giống như kiểu github nhưng chỉ chứa các docker images (public và private)

Ngoài các image có sẵn, ta có thể tự tạo custom image bằng `Dockerfile`

Commands:

- `docker images`: liệt kê tất cả các images hiện có
- `docker run {name}:{tag}`: run 1 image với tag của nó, nếu nó không tìm thấy trong máy local, docker sẽ tự động pull về từ DockerHub
- `docker run -d {name}:{tag}`: `-d` means detach, which means docker runs in the background without block terminal
- `docker run -d --name {container_name} {name}:{tag}`: thêm tên cho container cho dễ nhớ

### Dockerfile

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
FROM node:12              // Download node image về

WORKDIR /app              // CD đến thư mục app

COPY package*.json ./     // Copy các file package*.json vào thư mục app. Nếu dùng yarn thì phải COPY yarn.lock

RUN npm install           // Nếu dùng yarn thì chạy yarn install

COPY . .                  // Copy tất cả code trong repo vào thư mục app. Nhớ tạo file .dockerignore để ignore node_module

ENV PORT=8080

EXPOSE 8080

CMD [ "npm", "start" ]    // Khi setup xong thì chạy lệnh
```

Tạo 1 file `.dockerignore` để ignore `node_module`

Để chạy docker file, dùng lệnh: `docker build -t <docker_image_name> .`. `-t` hay `--tag` là đánh tag cho docker image
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

Update code

- Re-build lại docker image: phải xóa docker image cũ đi rồi build lại cái mới dùng lệnh `docker build -t` ở trên
- Stop và remove container hiện tại: dùng lệnh `docker rm -f <the-container-id>` hoặc dùng Dashboard

## Docker compose file

1 file `.yaml` chứa hướng dẫn để build ra *Docker Container*, dùng để chạy nhiều docker file cùng lúc

Để chạy file dùng lênh: `docker-compose -f <yaml_file> up`

## Optimize docker image size

<https://github.com/webuild-community/advent-of-frontend/blob/main/2022/day-23.md>

- Remove devDependencies bằng cách build multi-stage