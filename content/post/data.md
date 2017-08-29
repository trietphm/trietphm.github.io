+++
author = ""
date = "2017-08-29T05:30:56+07:00"
description = ""
tags = []
title = "Foundation of data system"

+++

# Data system
- Ngày nay do sự phát triển rất nhanh về phần cứng nên hầu hết các ứng dụng không còn phát triển theo hướng tối ưu hóa về tốc độ xử lý của CPU (compute-intensive), công nghệ hiện tại đã đủ và dư để đáp ứng hầu hết nhu cầu, thay vào đó sự bùng nổ thông tin đã mở ra kỷ nguyên về khai thác dữ liệu.
- Các ứng dụng hầu hết đều xoay quanh vấn đề khai thác một cách triệt để data (data-intensive), với các nhu cầu phổ biến như:
 - Lưu trữ và tìm kiếm dữ liệu (database)
 - Ghi nhớ tạm các dữ liệu tốn nhiều chi phí tính toán để tăng tốc độ đọc (caches)
 - Hổ trợ tìm kiếm tối đa theo nhiều cách (search indexs)
 - Gửi message đến các processes khác nhau, và có thể xử lý bất đồng bộ (stream processing)
 - Chạy và xử lý một lượng lớn dữ liệu theo định kỳ (batch processing)

> ddia_0101.png 

Và nhìn chung hệ thống cần đáp ứng được các yêu cầu sau

## Reliability

- Đảm bảo hệ thống luôn vận hành chính xác và đáng tin cậy kể cả có lỗi phát sinh, ví dụ:
 - Thực thi được đúng chức năng như mong đợi của người dùng.
 - Có khả năng tolerate các lỗi từ người dùng hoặc lỗi phát sinh từ chính bên trong hệ thống.
 - Performance luôn ở mức đủ tốt cho các use case trong điều kiện khối lượng và độ tải của dữ liệu nằm trong khoảng đã ước lượng trước (vd ước lượng hệ thống sẽ hoạt động trơn tru khi data có dung lượng < 100GB và độ load khoảng read 1 triệu records/s, write/update/delete ~ 50k records/s).
 - Đảm bảo ngăn chặn được mọi hành vi truy cập trái phép hoặc gây nhiễu.

- Lỗi phát sinh có thể bắt nguồn từ:
 - Hardware: khả năng hư RAM, ổ cứng tuy không cao nhưng hoàn toàn có thể xảy ra
 - Software: bug luôn luôn tồn tại trong code
 - Human: theo thống kê 10-25% nguyên nhân làm hệ thống bị outages là do config sai (do người vận hành) 

## Scalability

- Thử tưởng tượng hệ thống đang hoạt động tốt với 100 users đồng thời, chuyện gì sẽ xảy ra nếu tăng lên thành 1000 users, 100000 users hay 1000000 users? Hệ thống hiện tại chưa chắc đã đáp ứng được nhưng nó cần có khả năng mở rộng để chuẩn bị cho việc đó.
 - describe load
 - describe Performance
- solution

## Maintainability

Hiểu đơn giản là làm cho hệ thống có thể control được tốt nhất, tạo ra các abstraction để giảm thiểu độ phức tạp trong việc quản lý hệ thống, dễ dàng vận hành, sửa chửa, thêm bớt tính năng. Đồng thời có thể monitor được tổng quát tình trạng hệ thống (system's health) để đưa ra các quyết định nhanh và chính xác nhất.

- Operability: được thiết kế sao cho team operation có thể dễ dàng vận hành hệ thống một cách trơn tru (monitor, track down cause of problems, keep things up to date, secure patches, perform complex maintaince tasks, Defining processes that make operations predictable and help keep the production environment stable, good practices & tool for deployment, configuration management,...)
- Simplicity: Đơn giản. Người mới khi join vào có thể hiểu được hệ thống một cách dễ dàng 
- Evolvability: hay nói cách khác là extensibility, hệ thống cho phép thay đổi dễ dàng trong tương lai để đáp ứng requirement

# 
