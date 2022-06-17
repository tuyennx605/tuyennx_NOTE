# DOCKER

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



## 3. Docker Container:  [LINK](https://docs.docker.com/engine/reference/commandline/container/)
  ``` docker container COMMAND```

  ***CHÚ Ý***: ```docker container COMMAND``` ***=*** ```docker COMMAND``` ====> ở đây mình sử dụng docker COMMAND cho ngắn và tiện

* ***get list container*** :
  * ```docker ps [OPTIONS]``` : list ra container 
    * [OPTIONS] :
      * không có option : liệt kê các container đang chay
      * ***-a*** : liệt kê tất cả các container bao gồm cả ko chạy
      * ***-s*** : hiển thị tổng kích thước tệp
    * VD:
      * ```docker ps``` : list container đang chạy
      * ```docker ps -a``` : list all container
      * ```docker ps -as``` : list all container kèm size của nó 

* Bật tắt restart container:
  * ```docker container start [OPTIONS] CONTAINER [CONTAINER...]``` : bật container
    * [OPTIONS] : 
      * ***-a*** : attach vào container
    * VD:
      * ```docker start name_container``` : khởi chạy lại container name_container
      * ```docker start -a  name_container``` : khởi chạy lại container và attach vào nó
  
  * ```docker container stop [OPTIONS] CONTAINER [CONTAINER...]``` : tắt container
    * VD:
      ```docker stop name_container``` : dừng container name_container
  * ```docker restart [OPTIONS] CONTAINER [CONTAINER...]``` : restart container
    * VD: 
      * ```docker restart my_container``` : restart my_container


* ***Tạo container*** :
  * ```docker run [OPTIONS] IMAGE [COMMAND] [ARG...]``` : chạy một container từ image
    * [OPTIONS] :
      * ***-i*** : Cung cấp một con đường giúp các chương trình trong container nhận được command đã viết 
      * ***-t*** cung cấp giao diện để gõ command bên trong container
      * ***-d*** Chạy lệnh trong nền
      * ***-e*** set biến mỗi truòng cho lệnh exec vào container
      * ***--name*** : set name cho container
      * ***-p*** : exposed port ra ngoài : portLocal:portContainer
      * ***-P*** : exposed all port ra ngoài
      * ***-v***: share thư mục ngoài local vào docker ==>   *path_local:path_docker_container*

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
      * ***-i*** : Cung cấp một con đường giúp các chương trình trong container nhận được command đã viết 
      * ***-t*** cung cấp giao diện để gõ command bên trong container
      * ***-d*** Chạy lệnh trong nền
      * ***-e*** set biến mỗi truòng cho lệnh exec vào container
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



































