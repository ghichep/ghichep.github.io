---
layout: default
---

# ĐỪNG CHẠY HTTP BẰNG QUYỀN WWW-DATA

## GIỚI THIỆU

Bài viết này này nói cụ thể về cách phân quyền user chạy trên Nginx, tuy nhiên Apache thì cũng như vậy thôi. Nên chỉ nói về một cái Nginx này thôi.

Thông thường, người ta cài đặt mặt định và chạy website với một quyền duy nhất, đó chính là  ```www-data``` với rất nhiều virtual host.

Điều này thật sự nguy hiểm khi có một trong những VirtualHost bị chiếm quyền điều khiển (có thể là upload shell). Và thế là tất cả các Virtual host khác cũng cùng chung số phận.

Phương thức này gọi là Local Attack ngày xưa rất hay thường tấn công một website một cách gián tiếp thay vì tấn công trực tiếp trên lỗ hỏng cụ thể của tên miền đó.

## CÀI ĐẶT 

Bài viết này bỏ qua phần cài đặt như thế nào.

Chỉ nói rõ là cài Nginx và PHP FPM version 7.4. 

## CẤU HÌNH 

Cấu hình file nginx trong folder /etc/nginx/sites-enabled/beta.ghichep.com.conf như sau:

```
server {
        root /home/ghichepcom/beta.ghichep.com;
        # Add index.php to the list if you are using PHP
        index index.php index.html index.htm index.nginx-debian.html;

        server_name beta.ghichep.com;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

        # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/run/php/beta.ghichep.com-php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}
```

Cái file này mình trích xuất từ file default của Nginx, các bạn có thể vào đó xem. Cái này thì không có gì đặc biệt cả.

Tiếp đến là cấu hình cho PHP FPM

```
cd /etc/php/7.4/fpm/pool.d/
vim beta.ghichep.com.conf
```

Các bạn chú ý dòng thứ 4, mặc định của nó sẽ là `www`, ở đây mình đỗi nó thành socket riêng và user riêng. Nhất định không sử dụng chung socket. Nếu không sẽ bị hack sạch, cũng là phần quan trọng nhất.

Thay `www` bằng `beta.ghichep.com`, nó sẽ thành như thế này: 

```
  1 ; Start a new pool named 'www'.
  2 ; the variable $pool can be used in any directive and will be replaced by the
  3 ; pool name ('www' here)
  4 [beta.thosua.com]
```

Tiếp đến là dòng phân quyền chạy service:
```
20 ; Unix user/group of processes
 21 ; Note: The user is mandatory. If the group is not set, the default user's group
 22 ;       will be used.
 23 user = betathosua
 24 group = betathosua
 ```

 Tiếp đến là đến socket
 ```
 26 ; The address on which to accept FastCGI requests.
 27 ; Valid syntaxes are:
 28 ;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
 29 ;                            a specific port;
 30 ;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
 31 ;                            a specific port;
 32 ;   'port'                 - to listen on a TCP socket to all addresses
 33 ;                            (IPv6 and IPv4-mapped) on a specific port;
 34 ;   '/path/to/unix/socket' - to listen on a unix socket.
 35 ; Note: This value is mandatory.
 36 listen = /run/php/beta.ghichep.com-php7.4-fpm.sock
 ```

 Tiếp đến là phần set permission:
 ```
  42 ; Set permissions for unix socket, if one is used. In Linux, read/write
 43 ; permissions must be set in order to allow connections from a web server. Many
 44 ; BSD-derived systems allow connections regardless of permissions. The owner
 45 ; and group can be specified either by name or by their numeric IDs.
 46 ; Default Values: user and group are set as the running user
 47 ;                 mode is set to 0660
 48 listen.owner = betaghichep
 49 listen.group = www-data
 ```

 Rồi xong phần cấu hình file rồi. Thêm user betaghichep vào thôi:

```
 adduser ghichep
 ```

 Phân quyền lại cho home directory của user.
 
 ```
 chmod 660 -R /home/betaghichep/
 ```

 OK, như vậy là xong cả rồi. Đối với các virtual host tiếp theo, bạn đều lặp lại quá trình tạo như thế này. Vậy mỗi virtualhost sẽ được quản lý bởi một người dùng.

 Khi mà một hacker tấn công vào, họ cũng không thể tấn công sang các website khác.