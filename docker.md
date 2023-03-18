# Docker

## Reference

<https://www.youtube.com/watch?v=3c-iBn73dDE&t=1s>
<https://www.youtube.com/watch?v=pTFZFxd4hOI>
<https://devdojo.com/bobbyiliev/free-introduction-to-docker-ebook>
<https://www.freecodecamp.org/news/how-to-remove-all-docker-images-a-docker-cleanup-guide/>

## Tại sao phải sử dụng docker

<https://dev.to/techworld_with_nana/what-problems-does-docker-really-solve-496a>

Giải quyết vấn đề triển khai code. Ví dụ có 3 môi trường dev, test, product, mỗi môi trường khác nhau về OS, hardware... => việc setup (setup webserver, database, packages version...) để app chạy cho từng môi trường cũng tiêu tốn nhiều thời gian. Kể cả các môi trường có giống nhau đi chăng nữa, giống như local của máy A và local của máy B cùng chung OS và hardware, thì việc setup lại từ đầu cũng không hay.

Docker sẽ đóng gói ứng dụng vào 1 container, mỗi môi trường có thể có container riêng của môi trường đó, khi deploy ta chỉ cần run docker container đấy là mọi thứ sẽ được setup. 

## Các thuật ngữ

### Container

Tưởng tượng nó như 1 cái hộp, trong đấy chứa tất cả các hệ điều hành, database, và cả code của mình. Mình có thể dễ dàng quản lý, di chuyển, share cái container này.

Về bản chất, *container* giống với *virtual machine* ở chỗ là có có thể cài app, OS tách biệt với máy tính đang chạy. Tuy nhiên, container có điểm khác so với vm đó là nó dùng chung OS kernal với hệ điều hành hiện tại của máy còn VM sẽ dùng OS kernal riêng. Do đó, container sẽ

- Nhẹ hơn, tốn ít tài nguyên hơn
- Khởi động nhanh hơn

*Container* được tạo ra từ *Docker Image*.

### Dockerfile

Chứa hướng dẫn để setup môi trường và build ra *Docker Image*

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

Để chạy docker file, dùng lệnh: `docker build -t <docker_image_name> .`. Docker image name có thể là my_app:1.0. Quá trình build docker image nó giống như lệnh `docker pull`, sau khi pull về thì chạy lệnh `docker run`

### Image

Là 1 snapshot (giống file ghost) mà khi run snapshot này sẽ cho ra *Container*, hay nói cách khác là tái tạo lại môi trường. Có thể hiểu Docker Image như là class và container như là instance của class đó.
Có thể push image này lên cloud và người khác có thể pull image này về máy mình và chạy

Tạo 1 file `.dockerignore` để ignore `node_module`

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