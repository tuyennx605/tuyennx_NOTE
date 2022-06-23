# LINUX COMMAND LINE NOTE - TUYENNX
***
## Trợ giúp sử dụng lệnh:
* ```man < command > ```: vd: man ls
* ```< command > --help``` : vd: ls --help

NOTE: Cú pháp dòng lệnh:

``` < command> [option] [arguments] ```

VD: ``` ls -al /root  ```  :  liệt kê các file của thư mục root ( cả thư mục ẩn )

## NOTE:

* các ký hiệu viết tắt của tập tin :
  * ```-``` : file thông thường
  * ```d``` : thư mục
  * ```l``` : liên kết
  * ```c``` : thiết bị nhập ký tự
  * ```s``` : socket mạng
  * ```b``` : Thiết bị lưu trữ

## Các lệnh list và tìm kiếm file:

* ```ls [OPTION]... [FILE]...``` : liệt kê thông tin của file và folder
  [OPTION] :
    * ***-l*** : hiển thị tệp hoặc thư mục, kích thước, ngày, thời gian đã sửa đổi, tên tệp hoặc tên thư mục và chủ sở hữu
    * ***-a*** : liệt kê các file ẩn
    * ***-h*** : kết hợp với -l để hiển thị kích thước ở định dạng có thể đọc được của con người
    * ***-S*** : (Sort Size) sắp xếp file theo kích thước giảm dần
    * ***-t*** : sắp xếp file theo thời gian

  ===> ```ls -lah``` : liệt kê tất cả các file cả file ẩn

* ```find [path...] [expression]``` : tìm kiếm file hoặc folder
  * ```find ./tuyennx -name config.txt``` : tìm file hoặc folder đúng là config.txt trong thư mục ./tuyennx
  * ```find ./tuyennx -iname config.txt``` : tìm file hoặc folder config.txt ko phân biệt hoa thường trong tuyenn
  * ```find /tuyennx -type f -name "*.php"``` chỉ tìm file có đuôi là .php
  * ```find /tuyennx -type d -name config``` : chỉ tìm folder config


* ```grep [OPTION]... PATTERNS [FILE]...``` : tìm kiếm trong file 

  * [option] :
    * ***-i*** : Tìm không phân biệt hoa thường
    * ***-l*** : hiển thị danh sách file
    * ***-n*** : thêm sô thứ tự dòng
    * ***-v*** : in ra các dòng không chứa chũô cần tìm
    * ***-c*** : tổng số dòng chứa chuỗi cần tìm
  * VD:
    * ```grep -in tuyen tuyennx.test.txt``` : tìm từ "tuyen" trong file tuyennx.test.txt và không phân biệt hoa thường, in ra dòng của nó
    * ```grep -in '[0-9]' tuyennx.test.txt``` : tìm theo regex trong file tuyennx.test.txt
 

* ```df [OPTION]... [path]...``` : tóm tắt sử dụng không gian đĩa
  * [OPTION] :
    * ***-a*** (all) : list ra tất cả 
    * ***-h*** : để hiện size theo dạng G, M, K...
  * VD:
    * ```df -h``` : hiện không gian đĩa format size dạng G, M, K


## Các lệnh với Thư mục:

* ```mkdir [option1] [option2] < path_name> [<path_name> <path_name>]``` : Tạo thư mục mới 
    
    vd : mkdir tuyennx/test
        mkdir tuyennx/test1 tuyenn/test2 tuyennx/test3
    [option] 

      * ***-p*** : tạo cả thư mục cha khi thư mục cha chưa có
      * ***-m*** : set luôn quyền cho folder đó
    ==> ``` mkdir -p -m 777 tuyennx/test3 ```

* ``` pwd ``` : xem đường dẫn hiện tại đang đứng
* ``` mv <path_name_old> <path_name_new> ``` : đổi tên thư mục. 

    ===> ``` mv tuyennx/test3 tuyennx/testxxx ```

* ```rmdir <path_name>``` : xóa thư mục nếu nó rỗng, nếu ko rỗng thì ko xóa

  ===> ```rmdir tuyennx/test1```


* ```du [option1] [option2] < path_name>``` : xem thông tin thư mục

    [option] :

      * ***-h*** : đọc được dung lượng cua folder như kilobyte, megabyte, gigabyte ...
      * ***-d N*** : đặt độ sâu N cho cây thư mục hiện ra
      * ***--time*** : thời gian cập nhập gần nhất folder (--time=access : thời gian truy cập gần nhất hoặc  --time-style=STYLE (--time-style="+%Y-%m-%d %H:%M:%S"   hoặc --time-style=iso) )

    ===> ``` du -h -d 2 tuyennx ``` : hiện thông tin đến độ sâu 2 của thư mục tuyennx



## Các lệnh quản lý file:

* ```touch <path_name>/file_name``` : Tạo file rỗng mới
  
  ===> ```touch tuyennx/file.txt```

* ``` echo "noi dung" [ >|>> ]  <path_name>/file_name``` : tạo file mới với nội dung replace ( > ) hoặc append ( >> )

  ===> ``` echo "noi dung tap tin" >> tuyennx/ten_tep.txt ``` : ghi tiếp vào file

  ===> ```echo "noi dung tap tin" > tuyennx/ten_tep.txt``` : ghi đè vào file


* ``` cp [option] [option2] source1 source2 destination ``` : copy file

  ===> ```cp A.txt B.txt``` : copy file A.txt chuyển vào file B.txt

  ===> 

* ``` mv [OPTIONS] SOURCE1 SOURCE2 SOURCE3.. DESTINATION ``` : di chuyển file hoặc thư mục

  ==> ```mv file1 /tuyennx``` : copy file1 sang folder tuyennx

  ==> ```mv file1 file2``` : copy file1 sang file mới file2

  ==> ```mv dir1 dir2```: copy thư mục dir1 thành dir2


* ```rm [OPTIONS]... FILE...``` : xóa file (xóa folder khi có option -r)

  [option] :

    * ***-f*** : (--force) : tắt thông báo để đỡ phải xác nhận yes
    * ***-d*** : giống rmdir : để xóa thư mục
    * ***-r*** : xóa lần lượt từ con đến cha trong thư mục (đệ quy xóa)

  ===> ```rm -rf folder``` : xóa folder và cả con của folder bỏ qua xác nhận
  ===> ```rm file1``` : xóa file

* ```head <option> <file>``` : hiển thị n dòng đầu trong 1 file (mặc định là 10 dòng)

  * ```head tuyennx.txt``` : hiện ra 10 dòng trong file
  * ```head -n 5 tuyennx.txt``` : hiện ra 5 dòng trong file

* ```tail [OPTION]... [FILE]...``` : hiển thị n dòng cuối của 1 file (mặc định 10 dòng)

  * ```tail -n 3 tuyennx.txt``` : hiển thị 3 dòng cuối cùng của file

## Phân quyền:
  
* Có 3 chủ thể trong cơ chế phân quyền là : USER, GROUP, OTHER
* Mỗi chủ thể có 3 quyền chính : Read (r = 4), Write (w = 2), Execute (x = 1)

===> 755 nghĩa là : User: 7=4+2+1(Read, Write, Execute), Group: 5=4+1
(Read+Execute), Other: 5=4+1 (Read+Execute). 

* ``` chmod [options] mode [mode] file1|folder file2|folder ....``` : phân quyền

  * [option] :
    * ***-R*** : (Recursive : đệ quy) : áp dụng phân quyền cho tất cả file và folder trong
    * ***-f*** : (force : có hiệu lực) : Vẫn set quyền kể cả xảy ra lỗi
    * ***-v*** : (verbose: rườm rà) : hiển thị ra những đối tượng được xử lý
  * Ví dụ:
    * ```sudo chmod -R 777 tuyennx``` : set full quyền cho tuyennx



## Cơ chế pipeline :

* ```Lệnh_1 | Lệnh_2 ... ``` : Lấy kết quả của một lệnh và truyền vào lệnh tiếp theo

  * VD: ```ls /tuyennx | tail –n 5``` : hiện ra 5 file cuối của kết quả ls (list ra file trong tuyennx)


## Gói phần mềm


## SSH server :

* ``` ssh ubuntu@ip ``` : ssh vào khi sever thêm ssh.public của máy mình vào *~/.ssh/authorized_keys*

* ``` sudo ssh -i tuyennx1.pem ubuntu@ip ``` : ssh vào server khi có file pem

* ```sudo ssh root@ip``` : ssh khi có tài khoản, mật khẩu (ở đây là root)
