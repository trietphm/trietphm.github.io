+++
author = "Triet Pham"
date = "2018-03-16T07:40:38+07:00"
description = "Tạo một newsfeed random"
tags = ["redis","random","newsfeed"]
title = "Random newsfeed"

+++

# Các thuật toán sắp xếp newsfeed

- Một trong những tính năng thường gặp của những trang tin tức/mạng xã hội là newsfeed.
- Thông thường các trang này được thiết kế để hiển thị **tin tức mới nhất** (sort theo timestamp) hoặc những **tin được ưa chuộng, đáng giá cao** (sort theo hệ thống ranking được đánh giá dựa score/view/ như hackernews, reddit) hoặc đơn giản là **theo ý thích của admin**, mình thích thì mình đưa lên top thôi (kipalog).
- Tuy nhiên, hầu hết các dạng này đều phải sort dựa trên một giá trị nhất định và quan trọng nhất cần có một lượng bài viết liên tục đủ để tránh làm cho newsfeed không bị nhàm chán, nhìn chỉ có vài bài đăng đi đăng lại.
- Ngoài ra, không như các trang tin tức hằng ngày, chỉ cần qua một đêm là đã thành tin outdated, một số trang web chia sẽ tin tức, kiến thức lại không như vậy (ví dụ chia sẽ kiến thức thiên văn, văn học, lịch sử, kỹ năng,... Hoặc chia sẽ các địa điểm, thông tin du lịch, mẹo vặt,...), đồng thời lượng bài viết cũng không nhiều bằng. Và các thuật toán newsfeed ở bên trên chỉ có thể giúp người dùng tiếp cận được một lượng thông tin rất nhỏ.
- Và đôi khi do lượng bài cập nhật không liên tục (vd khoảng 1-2 ngày mới có 1 bài), khi user vào cứ thấy có vài bài vừa được đăng gần đây và kết luận _site cùi quá, chả có gì hay_, mặc dù còn một lượng bài viết rất lớn đã được đăng trước nhưng user không được tiếp cận.
- Để giải quyết tình trạng trên, bài viết dưới sẽ mô tả về việc random dữ liệu, đồng thời thiết kế newsfeed có vẻ có *hơi hướm* newsfeed base user (nội dung xây dựng dựa trên hành vi người dùng).
- Việc random có thể sẽ mang lại ưu và nhược điểm như sau:
 - Ưu: tạo cảm giác dữ liệu phong phú, site active liên tục, mỗi lần user refresh/truy cập sẽ có được thông tin khác nhau, và mỗi lần load đều là trải nghiệm dữ liệu mới.
 - Nhược: đôi khi user thấy gì đó hay ho, quay lại xem thì lại không tìm thấy cái cũ, random mà. Nhưng đôi khi đây cũng có thể coi là feature tùy theo business của product.

# Random newsfeed
## Yêu cầu

Để dễ hình như ta sẽ thiết kế một API newsfeeds trả về danh sách địa điểm ăn uống một cách ngẫu nhiên. Giả sử mình đang có trong tay tập dữ liệu của 10000 địa điểm với nội dung khác nhau và đang nằm trong DB SQL (Mysql/Postgresql). Newsfeed sẽ giống như timeline của Facebook, tức là cứ scroll xuống thì client sẽ request lên để lấy nội dung mới về. Và cần phải đảm bảo:

- Mỗi lần reload newsfeed phải có nội dung random khác nhau giữa các lần reload.
- Dữ liệu trả về ở mỗi lần load more (scroll xuống) phải đảm bảo không trùng với dữ liệu đã trả về trước đó.

- Giả sử dữ liệu của mình có dạng đơn giản như sau:

```
eateries

+-----+------+---------+------------+
| id  | name | address | created_at |
+-----+------+---------+------------+
| int | text | text    | timestamp  |
+-----+------+---------+------------+
```

- Và format API trả về sẽ có dạng như thế này:

```
{
	"data": [
		{<eatery_1>},
		{<eatery_2>},
		{<eatery_3>},
		...
	],
	"pagination": {
		"nextUrl":"/newsfeeds?param1=x&param2=y&param3=z"
	}
}
```

Với format trên, client mỗi lần cần lấy thêm dữ liệu (load more) chỉ cần lấy `nextUrl` ra và request thẳng lên server mà không cần thêm thao tác gì khác, mọi thứ đều được quyết định ở server.

## Xây dựng
### Simple approach: Lọc phần tử trùng mỗi lần load

- Cách tiếp cận đơn giản nhất: mỗi lần load sẽ query random, đồng thời loại bỏ các địa điểm đã load trước đó, câu query sẽ có dạng như sau:

```
	SELECT * FROM eateries WHERE id NOT IN (?) ORDER BY RANDOM()
```

- Dữ liệu trong param `?` nhận vào là danh sách `id` của những địa điểm đã load trước đó.
- Câu hỏi đặt ra là làm sao biết user hiện tại đã load những địa điểm nào rồi để bỏ qua? Cách đơn giản nhất là lưu thẳng danh sách địa điểm vào trong `nextUrl` và khi client request lên thì lọc ra. Ví dụ:
 - Lần load đầu tiên đã lấy ra 3 điểm có id là `[1, 2, 3]`.
 - Trong response trả về cho user sẽ là

 ```
	"pagination": {
		"nextUrl":"/newsfeeds?ignoreEatery=1,2,3"
	}
 ```

 - Lần tới khi client request với `nextUrl` trên, ta sẽ có danh sách `id` cần loại ra, câu SQL của lần load kế tiếp sẽ là:
 ```
	SELECT * FROM eateries WHERE id NOT IN (1,2,3) ORDER BY RANDOM() LIMIT 10
 ```
- **Vấn đề**:
 - URL có giới hạn về số lượng ký tự, đâu đó tầm [2000](https://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers) characters tùy theo browsers, nên chắc chắn không thể lưu được 10000 địa điểm.
 - Giả sử bằng cách nào đó có thể truy xuất được danh sách id cần loại ra ở trên, ví dụ ta sẽ cache tạm ở đâu đó trong memcache/redis, lúc nào cần thì đọc từ cache ra, sau đó query đến SQL database, khi response data về thì bổ sung thêm id mới vào cache. Có vẻ ổn nhưng thực tế sẽ phát sinh các vấn đề như sau:
   - Server round trip: IDs phải luân chuyển từ cache -> application -> SQL Database -> Update to cache
   - Query sử dụng điều kiện `NOT IN` sẽ không đảm bảo để sử dụng index (ví như ở trên `id` là primary key = đã indexed) nhưng db vẫn sẽ sequences scan, nguyên nhân vì planners thường sẽ dựa vào khá nhiều thông tin để quyết định xem có nên dùng index không, index nào, optimize thế nào để có performance tốt nhất và có thể sẽ đưa ra quyết định không chính xác, xem thêm [Indexes and NOT Equal](https://richardfoote.wordpress.com/2008/08/13/indexes-and-not-equal-not-now-john/).
   - Dưới đây là thông tin sau khi explain ra với data sample khoảng 80k records (Postgresql), thông tin chi tiết có thể xem ở đây https://explain.depesz.com/s/W7yl

   ```
   	EXPLAIN analyze
	SELECT * FROM orders WHERE id NOT IN (<List Ids here about 1000 items>)
	ORDER BY random()
	LIMIT 10
   ```

   ```
   Limit  (cost=109157.01..109157.04 rows=10 width=896) (actual time=804.610..804.614 rows=10 loops=1)
  ->  Sort  (cost=109157.01..109348.06 rows=76419 width=896) (actual time=804.608..804.611 rows=10 loops=1)
        Sort Key: (random())
        Sort Method: top-N heapsort  Memory: 29kB
        ->  Seq Scan on eateries  (cost=0.00..107505.63 rows=76419 width=896) (actual time=0.024..743.249 rows=77288 loops=1)
              Filter: (id <> ALL ('{<List ids dài quáaaaaaaaaaaaaaaaaaaaaaaa}'::integer[]))
              Rows Removed by Filter: 783
Planning time: 1.187 ms
Execution time: 804.675 ms
   ```

  - Per node type stats

   ```
| node type | count | sum of times | % of query |
|-----------|-------|--------------|------------|
| Limit     | 1     | 0.003 ms     | 0.0 %      |
| Seq Scan  | 1     | 743.249 ms   | 92.4 %     |
| Sort      | 1     | 61.362 ms    | 7.6 %      |
   ```

  - Nhìn vào query plan ở trên ta thấy `NOT IN` đã được chuyển thành `id <> ALL('{}'::integer[])`, và quan trọng nhất là index trên primary key `id` đã không được dùng mà planner đã dùng sequence scan aka full scan table, chiếm 743ms, quá chậm.
  - Ngoài ra còn một vấn đề khác là hàm `RANDOM()` cũng làm ảnh hưởng đến tốc độ, 61.3ms, và thực tế con số này rất vô chừng có thể tăng hoặc giảm tùy theo database stats, nhưng có thể chắc chắn sẽ chậm với lượng data lớn.
  - Vậy có cách nào khác để tăng tốc cho hàm `RANDOM()` không? Có thể sử dụng một vài cách khác để pick ra được random data nhưng theo đánh giá cá nhân của mình sẽ đều có trade off, tức tốc độ query sẽ tỉ lệ nghịch với mức độ `random`, và thật sự mà nói cũng không thể cải thiện được nhiều. Xem thêm [Best way to select random rows PostgreSQL
](https://stackoverflow.com/questions/8674718/best-way-to-select-random-rows-postgresql), [Performance of ORDER BY RANDOM to select random rows?](https://www.postgresql.org/message-id/flat/CAL_0b1u0UavZbUQo__a2jr0Vq31n2YK0wbUkjxx6H%3DNUT1u0zA%40mail.gmail.com#CAL_0b1u0UavZbUQo__a2jr0Vq31n2YK0wbUkjxx6H=NUT1u0zA@mail.gmail.com) và [Quality of PostgreSQL's random() function?
](https://stackoverflow.com/questions/9816114/quality-of-postgresqls-random-function). Mình sử dụng postgresql là chính nên chỉ research về postgresql, nhưng cơ bản random là vấn đề của computer science nói chung nên mình nghĩ ở database khác cũng sẽ khó có cách tiếp cận tốt hơn, xem thêm về cách Cloudflare random bằng cách dùng đèn lava [LavaRand in Production: The Nitty-Gritty Technical Details](https://blog.cloudflare.com/lavarand-in-production-the-nitty-gritty-technical-details/)


