# Installing Lazarus/vn

From Free Pascal wiki

│ **English (en)** │

## Contents

  * 1 Tổng quan
  * 2 Trên Windows
  * 3 Trên Linux
    * 3.1 Dùng file .deb
    * 3.2 Dùng file .rpm

  
---  
  
##  Tổng quan 

Dưới đây là cách cài đặt đơn giản nhất trên Windows, Linux. (Mong mọi người đóng góp thêm cách cài đặt trên Mac, FreeBSD và các hệ điều hành khác) 

##  Trên Windows 

Tìm trong địa chỉ này [[1]](<http://sourceforge.net/projects/lazarus/files/>) phiên bản mới nhất dành cho Windows và chạy nó. Ví dụ lazarus-1.2.0-fpc-2.6.2-win32.exe 

##  Trên Linux 

Đối với người dùng Ubuntu, bạn nên dùng file .deb, đối với hệ điều hành khác, bạn nên cần nhắc xem lựa chọn nào phù hợp hơn. 

###  Dùng file .deb 

Tìm trong địa chỉ này [[2]](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20i386%20DEB/>) ba file deb, ví dụ: 

lazarus_1.2.0-0_i386.deb 

fpc-src_2.6.2-0_i386.deb 

fpc_2.6.2-0_i386.deb 

Sau khi tải về, bạn lần lượt cài chúng theo thứ tự: fpc_2.6.2-0_i386.deb, fpc-src_2.6.2-0_i386.deb, lazarus_1.2.0-0_i386.deb. 

Bạn có thể cài bằng Software center hoặc dùng gdebi như sau: 

sudo apt-get install gdebi 

sudo gdebi fpc_2.6.2-0_i386.deb 

sudo gdebi fpc-src_2.6.2-0_i386.deb 

sudo gdebi lazarus_1.2.0-0_i386.deb 

###  Dùng file .rpm

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus/vn](https://web.archive.org/web/20150323054412/https://wiki.freepascal.org/Installing_Lazarus/vn)_
