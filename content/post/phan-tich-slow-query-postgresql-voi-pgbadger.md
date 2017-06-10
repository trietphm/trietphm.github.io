+++
draft = false
date = "2017-06-10T16:20:08+07:00"
title = "Phân tích slow query PostgreSQL với pgBadger"
description = "Phân tích slow query PostgreSQL với pgBadger"
+++

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/5dvag1s1vh_logo_pgbadger.png)
# Slow query là gì?

Khi các câu query chậm hơn một thời gian nhất định tùy theo bạn định nghĩa, ví dụ chậm hơn 50ms, thì các câu query đó được xem là slow query. 
Và tùy theo ứng dụng mà sẽ có định nghĩa khác nhau, một vài ví dụ:

- Bạn viết một API authentication cho cái app nho nhỏ và bạn hy vọng nó chạy càng nhanh càng tốt, tối đa là 30ms/request => Query phải < 30ms.
- Bạn viết một ứng dụng liên quan đến tiền bạc, thanh toán online. Đòi hỏi bắt buộc là phải chính xác và an toàn, chậm một chút cũng không sao => Query có thể nới ra thành < 100 ms.

Tùy theo mục đích mà có tiêu chuẩn thời gian khác nhau

# Slow query xảy ra khi nào?
Có rất nhiều trường hợp sẽ xảy ra slow query như:
- Query condition trên column thiếu index
- Query bị lock bởi các câu query, transaction,...
- Câu query không được optimize nên dẫn đến query thừa hoặc không tối ưu, sequence scan thay vì index scan,...
- Cấu hình máy không đáp ứng được nhu cầu query hiện tại.
- ...

Có khá nhiều nguyên nhân có thể xảy ra và để có thể giải quyết thì việc đầu tiên là phải biết nó có xảy ra :smile:

# Log slow query

Postgresql cho phép bạn log các câu query, statement,... và nhiều thứ khác ra.
Để log ra thì bạn có thể edit file `postgresql.config` và thay đổi theo nhu cầu, mình liệt kê một vài thông số cơ bản phục vụ cho bài viết, bạn có thể xem thêm ở đây
https://www.postgresql.org/docs/current/static/runtime-config-logging.html

- `log_directory`: Nơi lưu log, mặc định thường là `pg_log`
- `log_filename`: format name của file log
- `log_min_duration_statement `: Nếu lớn hơn thời gian này query sẽ được cho là slow query (đơn vị milisecond)

Save lại và reload (hoặc restart) PostgreSQL. Khi có slow query PostgreSQL sẽ giúp bạn ném vào log.

_**Note nhỏ:**_ Khi bạn restart PostgreSQL, các connection của client đều sẽ bị disconnect

              (╯°□°）╯︵ ┻━┻

nhưng nếu bạn reload thì không sao cả, mọi thứ vẫn bình thường

              (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧

`sudo service postgresql reload`

Khi log ra bạn sẽ thấy nhiều thông tin dạng thế này và fix dễ dàng hơn
```
593821c2.21a2 2017-06-08 00:00:41 ICT LOG:  duration: 439.218 ms  execute <unnamed>: SELECT * FROM "users"  WHERE (username = $1) LIMIT 1
593821c2.21a2 2017-06-08 00:00:41 ICT DETAIL:  parameters: $1 = 'trietphm'
```


# pgBadger
## Vì sao lại cần log analyzer?
Sau một ngày đẹp trời bạn mở file log lên và chợt nhận ra nó nặng cả GB với hằng hà sa số log query slow, với cơ man nào là các câu query dài loằng ngoằng hay những câu query ngắn ngủn như `SELECT * FROM users WHERE id = $1` mà bạn chắc chắn không thể nào chậm mà vẫn xuất hiện trong đây? Rồi có những câu query bạn cũng không biết nó đến từ server nào hay hành tinh nào? Không biết câu query nào chậm nhất để mà optimize? Không biết và không biết.

Một đống hỗn độn và gần như vô dụng!
## pgBadger là gì?
pgBadger là một PostgreSQL log analyzer nhỏ gọn được viết bằng Perl script giúp bạn phân tích và report từ PostgreSQL log file.
Sẽ giúp bạn khai thác được file hỗn độn kia, đập đá ra vàng. Sau khi xử lý pgBadge sẽ xuất ra cho bạn một file HTML (hoặc JSON) report về file log đó.

# Cài đặt
- Tải [pgBadger](https://github.com/dalibo/pgbadger/releases)
- Và make

 ```bash
        tar xzf pgbadger-9.x.tar.gz
        cd pgbadger-9.x/
        perl Makefile.PL
        make && sudo make install
```
- Trong quá trình cài đặt có thể bị lỗi và bạn cần cài thêm `perl-devel`. Cài đặt trong Centos qua rpm `sudo yum install perl-devel`

## Update format log file

Để pgBadger có thể đọc được tốt file log, bạn cần update lại một chút trong config:

-  `log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '` log ra các thông tin user, database name, application và client ip address
-  Enable các option log khác để lấy được nhiều thông tin hơn:

```
        log_checkpoints = on
        log_connections = on
        log_disconnections = on
        log_lock_waits = on
        log_temp_files = 0
        log_autovacuum_min_duration = 0
        log_error_verbosity = default
```

- Bạn có thể xem thêm trong [Document ](http://dalibo.github.io/pgbadger/)

## Parse log & export report

Cách đơn giản nhất:
```
pgbadger <file_log> <output.html>
```

Mặc định pgBadger sẽ để output là `out.html` nếu không được định nghĩa

## Report
Khi mở file `output` html lên sẽ có rất nhiều thông tin phân tích từ log file đó, một số hình ảnh:
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/kakppqn6wz_liLvC8eKeGH5Yub9IezLL--KQkAFD4qPvC0I3kbq9w3O4xDNbaESBkyMegfqg04ND0j2v7uSm1l144qLz_3Yvt0Iq-_ZMIwlJNqsu6s4bO0F1kR3dMUjedqC16uBUu85.png)
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/bumajvreio_Screen%20Shot%202017-06-10%20at%204.25.17%20PM.png)
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/j224q7vafu_Screen%20Shot%202017-06-10%20at%204.25.36%20PM.png)
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/1runvcti4u_Screen%20Shot%202017-06-10%20at%204.30.32%20PM.png)
