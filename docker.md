# Docker

## Reference

<https://www.youtube.com/watch?v=pTFZFxd4hOI>

## Tại sao phải sử dụng docker

Có thể dev trên local và đồng bộ môi trường trên máy local với các máy khác (reproducing environment). Các máy khác không phải setup môi trường phức tạp để chạy code của mình

## Các thuật ngữ

### Container

Tưởng tượng nó như 1 cái hộp, trong đấy chứa tất cả các hệ điều hành, database, và cả code của mình. Mình có thể dễ dàng quản lý, di chuyển, share cái container này.

Về bản chất, *container* giống với *virtual machine* ở chỗ là có có thể cài app, OS tách biệt với máy tính đang chạy. Tuy nhiên, container có điểm khác so với vm:

- Nhẹ hơn, tốn ít tài nguyên hơn
- Khởi động nhanh hơn

### Dockerfile

Chứa hướng dẫn setup môi trường và được dùng để build ra *Docker Image*

```code
FROM node:12              // Download node image về

WORKDIR /app              // CD đến thư mục app

COPY package*.json ./     // Copy các file package*.json vào thư mục app. Nếu dùng yarn thì phải COPY package.json yarn.lock để

RUN npm install           // Nếu dùng yarn thì chạy yarn install

COPY . .                  // Copy tất cả code trong repo vào thư mục app. Nhớ tạo file .dockerignore để ignore node_module

ENV PORT=8080

EXPOSE 8080

CMD [ "npm", "start" ]    // Khi setup xong thì chạy lệnh
```

### Image

Là 1 snapshot (giống file ghost) mà khi run snapshot này sẽ cho ra *Container*, hay nói cách khác là tái tạo lại môi trường
Có thể push image này lên cloud và người khác có thể pull image này về máy mình và chạy

Tạo 1 file `.dockerignore` để ignore `node_module`

## Commands

- `docker ps`: liệt kê danh sách container đang chạy
- `docker ps -a`: liệt kê danh sách container đang chạy và đang dừng
- `docker image ls`: liệt kê danh sách images
- `docker build -t <docker_image>` .: build image từ Dockerfile. Dấu chấm ở cuối để nói với Docker là chạy Dockerfile ở thư mục hiện tại
- `docker run <docker_image>`: chạy 1 docker image, nếu image này chưa có trên local thì nó sẽ được pull về từ docker hub
  ```
  docker run -it ubuntu
  ```
- `docker-compose up`: chạy tất cả các container
- `docker inspect <container_id> | grep "IPAddress"`: tìm IP của container

Update code
- Build lại docker image
- Stop và remove container hiện tại: dùng lệnh `docker rm -f <the-container-id>` hoặc dùng Dashboard