# DOCKER NOTE - TUYENNX

## 1. Cài đặt:

* [link trang chủ cài đặt](https://docs.docker.com/engine/install/ubuntu/)

* [link cấp quyền cho docker](https://docs.docker.com/engine/install/linux-postinstall/)

## Vòng đời :

```image --(run)-> container --(stop)-> stopped container --(commit)-> image (quay lại image lúc đầu đó) ```
## 2. Docker Image  [LINK](https://docs.docker.com/engine/reference/commandline/image/)

* ***get list images***:
  * ```docker images ``` : get list images đã tải trong máy


* ***search image trên hub***:
  * ```docker search [tên] ``` : tìm kiếm images trên docker hub
    * VD:
      * ```docker search ubuntu``` : tìm kiếm xem image ubuntu có không

* ***pull image về máy*** :
  * ```docker pull tenimage:tag``` : pull 1 image về máy
    * VD:
      * ```docker pull ubuntu:22.04``` : pull ubuntu 22.04 ve
      * ```docker pull ubuntu:latest``` : pull ubuntu phiên bản mới nhất

* ***Xóa image*** : (image đó phải ko có container nào dùng thì mới xóa đc)

  * ```docker image rm ten_image:tag_image  ||  ID_image``` : xóa 1 image
    * VD:
      * ```docker image rm hello-world``` : xóa image hello-world khoải máy


  * ```docker rmi en_image:tag_image  ||  ID_image``` : cũng là xóa 1 image
    * VD:
      * ```docker rmi hello-world```: cũng là xóa
  
  * ```docker rmi -f $(docker images -a -q)``` : xóa toàn bộ image

* ***Đưa docker image lên hub***:
  * ```docker login [OPTIONS] [SERVER]``` : để login vào docker hub
    * [OPTIONS] :
      * ```--password , -p``` : pass
      * ```--username , -u```: username
    * VD:
      ```docker login -u tuyennx -p 123456```

  * ```docker push [OPTIONS] NAME[:TAG]```:  đẩy 1 image lên docker hub
    * VD:
      ```docker push tuyennx/test_image:v1```

  ***NOTE***: khi tạo image từ container thì nên đặt tên theo dạng : ***YOUR_DOCKERHUB_NAME/docker-image-name:version***
      * Trong đó : *YOUR_DOCKERHUB_NAME* là docker hub name của 
## 3. Docker Container:  [LINK](https://docs.docker.com/engine/reference/commandline/container/)
  ``` docker container COMMAND```

  ***CHÚ Ý***: ```docker container COMMAND``` ***=*** ```docker COMMAND``` ====> ở đây mình sử dụng docker COMMAND cho ngắn và tiện

* ***get list container*** :
  * ```docker ps [OPTIONS]``` : list ra container 
    * [OPTIONS] :
      * không có option : liệt kê các container đang chay
      * ***-a*** : (--all) liệt kê tất cả các container bao gồm cả ko chạy
      * ***-s*** : (--size) hiển thị tổng kích thước tệp
    * VD:
      * ```docker ps``` : list container đang chạy
      * ```docker ps -a``` : list all container
      * ```docker ps -as``` : list all container kèm size của nó 

* Bật tắt restart container:
  * ```docker start [OPTIONS] CONTAINER [CONTAINER...]``` : bật container
    * [OPTIONS] : 
      * ***-a*** : (--attach) attach vào container
    * VD:
      * ```docker start name_container``` : khởi chạy lại container name_container
      * ```docker start -a  name_container``` : khởi chạy lại container và attach vào nó
  
  * ```docker container stop [OPTIONS] CONTAINER [CONTAINER...]``` : tắt container
    * VD:
      ```docker stop name_container``` : dừng container name_container
  * ```docker restart [OPTIONS] CONTAINER [CONTAINER...]``` : restart container
    * VD: 
      * ```docker restart my_container``` : restart my_container

* ***Xóa container*** :
  * ```docker rm [OPTIONS] CONTAINER [CONTAINER...]``` : xóa container
    * [OPTIONS] :
      * ***-f*** : (--force) : xóa container kể cả khi nó running
    * VD:
      * ```docker rm ea8``` : xóa container có id hoặc name là ea8 (container này phải stop ) 
      * ```docker rm -f ea8``` : xóa container có id hoặc name là ea8 kể cả nó đang chạy luôn 

  * ``` docker container prune [OPTIONS]``` : xóa toàn bộ container đã dừng
    * VD:
      * ```docker container prune``` : xóa toàn bộ container đã dừng


  * ```docker stop $(docker ps -a -q)```: dừng toàn bộ container

  * ```docker rm $(docker ps -a -q)```: xóa toàn bộ container


* ***Tạo container*** :
  * ```docker run [OPTIONS] IMAGE [COMMAND] [ARG...]``` : chạy một container từ image
    * [OPTIONS] :
      * ***-i*** : (--interactive) Cung cấp một con đường giúp các chương trình trong container nhận được command đã viết 
      * ***-t*** cung cấp giao diện để gõ command bên trong container
      * ***-d*** (--detach) Chạy lệnh trong nền
      * ***-e*** (--env) set biến mỗi truòng cho lệnh exec vào container
      * ***--name*** : set name cho container
      * ***-p*** : (--publish) exposed port ra ngoài : portLocal:portContainer
      * ***-P*** : (--publish-all) exposed all port ra ngoài
      * ***-v***: (--volume) share thư mục ngoài local vào docker ==>   *path_local:path_docker_container*
      * ***-w*** : (--workdir) : thư mục làm việc
    * VD:
      * ```docker run -it --name=pgadmin4 -p 5555:80 -e PGADMIN_DEFAULT_EMAIL=tuyen@gmail.com -e PGADMIN_DEFAULT_PASSWORD=123456 dpage/pgadmin4``` : 

      * ```docker run -it -v /home/nodejs/Documents/DB/mongo_setel:/data/db -p27017:27017 --name mongodb -d mongo```


* ***Truy cập vào trong container***:
  * *Docker Attach* : ``` docker attach [OPTIONS] CONTAINER``` : truy cập vào container với tiến trình đang chạy của container luôn
    * VD:
      * ```docker attach tuyennx_container``` : truy cập vào container tuyennx_contaienr
    * LƯU Ý:
      * Thoát khỏi container bằng ***CTR+C*** thì sẽ stop container
      * nếu container chạy với -it thì có thể ***CTR-p + CTR-q*** đê thoat khỏi và container vẫn chạy



  * *Docker exec* : ```docker exec [OPTIONS] CONTAINER COMMAND [ARG...]```: truy cập vào container với tiến trình khác, không liên quan đến tiến trình đang chạy
    * [OPTIONS] : 
      * ***-i*** : (--interactive) Cung cấp một con đường giúp các chương trình trong container nhận được command đã viết 
      * ***-t*** cung cấp giao diện để gõ command bên trong container
      * ***-d*** (--detach) Chạy lệnh trong nền
      * ***-e*** (--env) set biến mỗi truòng cho lệnh exec vào container
    * VD: 
      * ```docker exec -d tuyennx_ubuntu1 touch tuyennxxx.txt``` : Chạy ngầm (-d) lệnh touch đê tạo 1 file
      * ```docker exec -it tuyennx_ubuntu1 bin/bash``` : truy cập vào container để gõ lệnh (-it)
      * ```docker exec -it -e tuyen=1 -e aaa=3 tuyennx_ubuntu1 bin/bash``` truy cap vao container va khoi chay no, set env cho container


* ***Commit container thành image***:
  * ```docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]```: commit 1 container thành 1 image để đẩy lên hub
    * [OPTIONS]: 
      * ***-a*** : (--author) : tên tác giả
      * ***-m***: (--message) : commit message
    * VD:
      * ```docker commit -a tuyennx_author -m "messsage commit"  name_id_container tuyennx/test_image:v1```  : tạo image với author : tuyennx_author, ....


* ***Copy file/folders giữa local và container***:
  * 
  ```docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH    hoặc : docker cp [OPTIONS] DEST_PATH CONTAINER:SRC_PATH DEST_PATH``` : copy file/folder từ container ra local hoặc từ local sang container

  * VD:
    * ```docker cp tuyen.bash tuyen_container:./tuyennx.bash``` : copy từ local vào container tuyen_container 

    * ```docker cp tuyen_container:./tuyen.bash ./tuyen666.bash``` : copy từ container sang local

 

* ***rename container*** ;
  * ``` docker rename CONTAINER NEW_NAME``` : thay đổi name củ container
    * VD:
      * ```docker rename bff6 tuyennx_ubuntu``` : thay đổi container id hoặc name là bff6 thành tuyennx_ubuntu

* ***Hiển thị thông tin của container***:
  * ```docker inspect [OPTIONS] NAME|ID [NAME|ID...]``` : hiển thị thông tin container
    * VD:
      * ```docker inspect tuyennx_container``` : hiện thông tin của container tuyennx_container


* ***Hiển thị logs của container*** :
  * ```docker logs [OPTIONS] CONTAINER``` : list ra logs của container
  * [OPTIONS] :
    * ***-n*** : (--tail) : số lượng dòng muốn logs ra
    * ***--since*** : hiện logs kể từ truyền vào là timestamp ( 2013-01-02T13:23:37Z) hoặc quan hệ (42m  (trong 42 phút trước))
    * ***--until*** : hiện logs trước mốc thời gian truyền vào là timestamp ( 2013-01-02T13:23:37Z) hoặc quan hệ (42m  (trong 42 phút trước))
  * VD:
    * ```docker logs -n 3 tuyennx_container```: show 3 dong logs của container tuyennx_container
    * ```docker logs --tail 3 --since 42m  tuyennx_container``` : show 3 dòng logs của container 42 phút trước
    * ```docker logs -f --until 2s test``` : show logs trước 2s
    * ```docker logs --tail 3 --until 2022-06-17T03:47:56.201Z tuyennx_container``` : show 3 dongf logs trước ngày truyền vào




***
## NOTE:
#### 1. Docker mongodb : [LINK](https://hub.docker.com/_/mongo)
* ***Docker mongodb khong pass*** : 
```bash
docker run -it -v /home/nodejs/Documents/DB/mongo_test:/data/db -p27017:27017 --name mongodb -d mongo
```

* 
#### 2. docker postgresql [LINK](https://hub.docker.com/_/postgres)
* ***Docker postgresql pass***:
```bash
docker run -d \
    --name postgresdb -p 5432:5432 \
    -e POSTGRES_USER=useradmin -e POSTGRES_PASSWORD=123456 \
    -e POSTGRES_DB=db_name \
    -e PGDATA=/var/lib/postgresql/data/pgdata \
    -v /var/DATAPG:/var/lib/postgresql/data \
    postgres
```

#### 3. docker influxdb [LINK](https://hub.docker.com/_/influxdb)
```bash
docker run --name=influxdb -d -p 8086:8086 \
     -e INFLUXDB_HTTP_AUTH_ENABLED=true \
      -e INFLUXDB_DB=db0 \
      -e INFLUXDB_ADMIN_USER=admin -e INFLUXDB_ADMIN_PASSWORD=123456 \
      -e INFLUXDB_USER=user_name -e INFLUXDB_USER_PASSWORD=123456 \
      -e INFLUXDB_READ_USER=user_read -e INFLUXDB_READ_USER_PASSWORD=123456 \
      -e INFLUXDB_WRITE_USER=user_write -e INFLUXDB_WRITE_USER_PASSWORD=123456 \
      -v /var/DATAINFLUX:/var/lib/influxdb \
      influxdb
```


***


# 4. Docker file
***
## Định nghĩa:
* Dockerfile là file có tên là *Dockerfile*, là 1 file dạng text để build image từ các lệnh trong nó. Nghĩa là trong đó có các lệnh, chạy các lệnh xong nó sẽ build 1 image cho chúng ta

***VD***:
```Dockerfile
FROM node:12.10.0-alpine

ARG WORK_DIR=/var/www/node
RUN mkdir -p ${WORK_DIR}
COPY . ${WORK_DIR}/
WORKDIR ${WORK_DIR}

ENV NODE_OPTIONS --max-old-space-size=800 --max-http-header-size=32768

RUN apk update && apk upgrade && apk add --no-cache bash git python g++ make
ARG NPM_AUTH_TOKEN

RUN npm install
RUN npm run build
RUN rm -fr node_modules
RUN rm -fr src
RUN npm install --production
RUN rm -f .npmrc

ENTRYPOINT  ["npm", "run", "start:prod"]
```

## Chạy dockerfile build image và run docker container:
* ***BUILD image từ dockerfile*** : [Docker Build](https://docs.docker.com/engine/reference/commandline/build/)
  * ```docker build [OPTIONS] PATH | URL | -``` :
  * VD:
    * ```sudo docker build -t hello:latest .```

* ***Run docker container từ image***:
  * ```docker run -it <tên_image>```



## Các Lệnh trong Dockerfile:
* ***FROM***: ```FROM <image>[:<tag>] [AS <name>]``` : Chỉ định image nào sẽ là image cơ sở để quá trình build image thực hiện câu lệnh tiếp
  * VD:
    * ```FROM ubuntu:16.04``` : ubuntu sẽ là image được làm cơ sở

* ***RUN*** : có 2 dạng : ```RUN <command>``` hoặc ```RUN ["executable", "param1", "param2"]``` : dùng để chạy lệnh command line, nó được thực thi trong quá trinh build image và tạo 1 layer mới để chạy các lệnh tiếp theo (từ khi build image)
  * VD:
    * ```RUN npm install``` : chạy lênh npm install

* ***CMD*** : Cũng là chạy lệnh command line nhưng thực hiện lệnh mặc định khi khởi tạo container từ image (lệnh mặc định này có thể đc ghi đè từ dòng lệnh khi khởi tạo container)
  * VD: file Dockerfile như sau:
      ```Dockerfile
      FROM ubuntu:latest
      CMD echo "Tuyennx"
      ```
      * Build image => ``` docker run -it <tên_image>```  => kết quả : Tuyennx
      * Build image => ``` docker run -it <tên_image> /bin/bash```  => kết quả ko in ra vì nó bị thay thế bởi lệnh /bin/bash

* ***ENTRYPOINT***: Giống CMD, cũng là chạy lệnh command và thực hiện lệnh khi khởi tạo container từ image (khác là lệnh này ko thể thay thế ghi đè bằng lệnh khác khi khởi tạo)
  * VD: file Dockerfile như sau:
      ```Dockerfile
      FROM ubuntu:latest
      ENTRYPOINT echo "Tuyennx"
      ```
      * Build image (```docker build -t tenImage:version .```) => ``` docker run -it <tên_image>```  => kết quả : Tuyennx
      * Build image (```docker build -t tenImage:version .```) => ``` docker run -it <tên_image> /bin/bash```  => kết quả : Tuyennx vì nó ko bị thay thế bởi lệnh /bin/bash

  * Chú ý: chúng ta có thể kết hợp ***ENTRYPOINT*** với ***CMD*** để chạy thay thế được những đối sô được ghi đè :
    * VD: 
      ```
        FROM ubuntu:latest
        ENTRYPOINT ["/bin/echo", "Hello"]
        CMD ["world"]
      ```

        * Build image (```docker build -t tenImage:version .```) => ``` docker run -it <tên_image>```  => kết quả : Hello world
        * Build image (```docker build -t tenImage:version .```) => ``` docker run -it <tên_image> TuyenNX```  => kết quả : Hello TuyenNX : vì kết CMD bị thay thế lệnh

* ***MAINTAINER*** :  ```MAINTAINER <name>``` : thêm tên tác giả 
  * VD: ```MAINTAINER tuyen <tuyennx605@gmail.com>```

* ***EXPOSE*** : ```EXPOSE <port> [<port>/<protocol>...]``` : mở cổng cho container
  * VD:
    * ```EXPOSE 2000``` : mở cổng 2000

* ***ENV***: ```ENV <key>=<value> ...``` : thiết lập biến môi trường cho container
  * VD:
    * 
    ```bash
      ENV MY_NAME="John Doe"
      ENV MY_CAT=fluffy
      ENV HTTPS_PORT=111
    ```
* ***COPY*** : Copy 1 file từ máy local vào docker image.
  * VD: ```COPY hom* /mydir/``` : copy các file bắt đầu bằng hom vào image với đường dẫn /mydir/

* ***VOLUME***: ```VOLUME ["/host/path/" "/container/path/"] ``` share thư mục ngoài local, gắn kết luôn vào docker
  * VD:
    * ```bash
      RUN echo "hello world" > /myvol/greeting
      VOLUME /myvol
      ```

* ***WORKDIR*** : ```WORKDIR /path/to/workdir``` : Thư mục làm việc của container

* ***ARG*** : ```ARG <name>[=<default value>]``` : định nghĩa biến 
  * VD: 
    * 
    ```bash
      ARG WORK_DIR=/var/www/node

      WORKDIR ${WORK_DIR}
    ```





# 5. docker-compose.yml [Link](https://docs.docker.com/compose/compose-file/)

## Chạy file:
* ***Chạy các thành phần:***
  * ```docker-compose up -d```

* ***Down các thành phần:***
  * ```docker-compose down```

* ***Xem logs***:
  * ```docker-compose logs [SERVICES]```


```yml

version: '2'
services:
  prometheus:
    image: prom/prometheus
    ports:
      - '9090:9090'
    container_name: prometheus
    restart: always
    network_mode: host
    volumes:
      - './DATA/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml'
      - ./DATA/prometheus/prometheus/:/etc/prometheus/
      - ./DATA/prometheus/prometheus_data:/prometheus
  grafana:
    image: grafana/grafana:latest
    ports:
      - '3000:3000'
    container_name: grafana
    restart: always
    network_mode: host
    depends_on:
      - prometheus
    volumes:
      - './DATA/grafana/grafana.ini:/etc/grafana/grafana.ini'
      - './DATA/grafana/grafana-data:/var/lib/grafana'
      - ./DATA/grafana/grafana/provisioning/dashboards:/etc/grafana/provisioning/dashboards
      - ./DATA/grafana/grafana/provisioning/datasources:/etc/grafana/provisioning/datasources

```


## Các thành phần: [Link](https://docs.docker.com/compose/compose-file/#services-top-level-element)

* ***version*** : khai báo về version của docker-compose sẽ sử dụng để build ( [Link](https://docs.docker.com/compose/compose-file/compose-versioning/))

* ***service*** : Là một khối chứa 1 hoặc nhiều các dịch vụ hoặc container trong ứng dụng muốn chạy hoặc cài đặt
  * ***Khối dịch vụ:*** : bắt đầu bằng tên của dịch vụ đó
    * ***image***: chỉ định image và tag được sử dụng để tạo container cho khối này

    * ***ports***: mở cổng cho container và ánh xạ vào port nào của container : ```<source>:<destination>```
      * Cú pháp:
        ```yml
          ports:
            - "8080:80"    # Mở cổng 8080 public, ánh xạ vào 80 cua container
            - "443:443"
        ```
    * ***container_name*** : đặt tên cho container
      * VD: ```container_name: name_cua_container```
    
    * ***volumes***: share thư mục ngoài local vào trong docker hoặc bind ổ đĩa : ```<source>:<destination>```
      * VD:
        ```yml
          volumes:
            - './DATA/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml'
            - ./DATA/prometheus/:/etc/prometheus/     #share thư mục ./DATA/... ở local ánh xạ vào /etc/prometheus của container
        ```
    * ***restart***: định nghĩa action khi container bị tắt
      * ACTION:
        * ***no*** : mặc định, sẽ ko làm gì cả
        * ***always***: luôn restart lại cho đến khi xóa container
        * ***on-failure***: Khởi động lại container nếu chỉ ra lỗi
        * ***unless-stopped***: Khởi động lại ko quan trọng mã thoát nhưng dừng khi service bị dừng hoặc xóa
      * VD:
        ```yml
          restart: "no"
          restart: always
          restart: on-failure
          restart: unless-stopped
        ```
    * ***networks***: Định nghĩa mạng cho service container sẽ attched vào, tham chiếu đến network ngoài cùng cấp với service lát định nghĩa
      * VD:
        ```yml
          services:
          some-service:
            networks:
              - some-network
              - other-network
        ```

    * ***depends_on***: Thể hiện sự phụ thuộc vào service nào (nghĩa là khi các service đó chạy xong thì mới chạy đến service này)
      * VD:
        ```yml
            services:
              web:
                build: .
                depends_on:
                  - db
                  - redis
              redis:
                image: redis
              db:
                image: postgres
        ```
          ====> 1. khi start thì db và redis đc tạo trước xong mới tạo dịch vụ web
                2. khi stop thì web sẽ bị stop trước db và redis
    
    * ***environment***: khai báo biến env cho container
      * VD: 
        ```yml
          environment:
            MYSQL_ROOT_PASSWORD: abc123   #Thiết lập password
        ```

















