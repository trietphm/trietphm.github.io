+++
author = "Triet Pham"
date = "2017-08-29T05:30:56+07:00"
description = ""
tags = ["database"]
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

![alt text](/static/img/data/ddia_0101.png)

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

- **Operability**: được thiết kế sao cho team operation có thể dễ dàng vận hành hệ thống một cách trơn tru (monitor, track down cause of problems, keep things up to date, secure patches, perform complex maintaince tasks, Defining processes that make operations predictable and help keep the production environment stable, good practices & tool for deployment, configuration management,...)
- **Simplicity**: Đơn giản. Người mới khi join vào có thể hiểu được hệ thống một cách dễ dàng 
- **Evolvability**: hay nói cách khác là extensibility, hệ thống cho phép thay đổi dễ dàng trong tương lai để đáp ứng requirement

# Data Models and Query Languages
## Data Models

Hiện nay phổ biến nhất có thể kể đến 3 loại data models bao gồm: 
### Relational:

 - Được phát triển từ những năm 1970s, bên cạnh 2 loại khác là *network model* và *hierarchical model*, nhưng đã đè bẹp 2 loại kia, chiếm lĩnh vị trí đứng đầu và tiếp tục đến ngày nay.
 - Lưu trữ data trong các tables và giữa các tables có các mối quan hệ với nhau (relationship), cho phép `join` để lấy dữ liệu.
 - Do xu hướng phát triển theo OOP nên dễ dẫn đến mismatch giữa application models và data models, thường giải quyết thông qua các ORM frameworks như ActiveRecord, Hibernate,... nhưng chỉ ở một mức nào đó chứ không hoàn toàn.
 - Được sử dụng phổ biến nhất hiện nay với nhiều loại database: MySQL, PostgreSQL, Oracle, MariaDB, SQLite,...
 - Ứng dụng cho rất nhiều mục đích khác nhau: forum, social network, ecommerce, games,...

### Document:

- Được sinh ra với mục đích scale dễ dàng hơn so với relational databases, hổ trợ tốt hơn với trữ lượng data lớn cũng như có tốc độ write tốt hơn.
- Cho phép thực thi nhiều query mà relational databases chưa hổ trợ tốt.
- Do không có relationship nên để thể hiện các mối hệ như one-to-many, many-to-many các child objects sẽ được lưu chính bên trong (nested) parent object. Điều đó vừa mang đến việc lợi và hại, một vài mặt đối lập với relational database
- Lợi: 
  - Do các child objects đã được lưu trong parent object nên sẽ không tốn chi phí để `join` lấy data (normalization)
  - Flexible schema: mỗi record có thể có cấu trúc không giống nhau, lưu trữ dễ dàng hơn. Đồng thời việc đọc dữ liệu cũng không bị ràng buộc, cấu trúc object trả về phụ thuộc vào lúc read, schema-on-read, đây có thể coi như con dao hai lưỡi, vừa tốt vừa xấu. Trái ngược với Relational database là schema-on-write.
- Hại:
  - Dữ liệu bị phân mảnh, khi cần cập nhật dữ liệu phải cập nhật ở toàn bộ records khác thay vì chỉ cần cập nhật reference tables như ở relational database
  - Bị duplicate dữ liệu vì phải lưu ở nhiều nơi
  - Do cách lưu trữ nên mỗi record (document) thường sẽ rất lớn (chuỗi JSON, XML dài) và sẽ lãng phí tài nguyên nếu chỉ cần lấy một đoạn nhỏ trong record đó nhưng vẫn phải load toàn bộ record.
- Các database được sử dụng rộng rãi: MongoDB, CouchDB, RethinkDB, Elasticsearch, Sorl...

### Graph:

 - Khi dữ liệu chứa hầu hết các objects có mối quan hệ many-to-many, có thể biểu diện mối liên hệ giữa các objects lên một graph (đồ thị)
 - Một số database như: Neo4j, Titan, Datomic,...
 - Một graph bao gồm 2 đối tượng: 

#### Vertices - đỉnh (nodes or entities): 

   - Các đỉnh không giống nhau (unique)
   - Có nhiều cạnh từ 1 đỉnh ra đỉnh khác
   - Có nhiều cạnh từ đỉnh khác đi vào 1 đỉnh
   - 1 đỉnh có tập hợp nhiều properties (key-value pairs)

#### Edges - cạnh (relationships or arcs) có đặc tính:

   - Khác nhau (unique)
   - Đỉnh mà cạnh bắt đầu (tail vertex)
   - Đỉnh mà cạnh kết thúc (head vertex)
   - Label để mô tả mối quan hệ giữa 2 đỉnh
   - Tập hợp nhiều properties (key-value pairs)

### Hybrid database

Một số database hổ trợ đồng thời việc lưu trữ ở nhiều loại data models khác nhau như:

 - PostgreSQL (từ 9.3), MySQL (từ 5.7), IBM DB2 (từ 10.5) đã hổ trợ lưu trữ JSON (có thể thao tác như document model chứ không đơn thuần là raw text).
 - RethinkDB là document database nhưng vẫn support relational-like joins trong query, một số driver của MongoDB cũng support việc join, Arango support document & graph models.

## Lựa chọn data models nào?

- Không thể nói data model nào là tốt nhất, mỗi data models đều có một ưu điểm khác nhau và sẽ phù hợp cho những nhu cầu khác nhau, do đó tùy thuộc vào mục đích của app mà đưa ra lựa chọn thích hợp
- Nếu dựa vào mối quan hệ giữa các đối tượng trong data có thể đưa ra lựa chọn như sau:
 + Data ít có mối quan hệ lẫn nhau, ít có nhu cầu `join` để lấy dữ liệu, các đơn vị data riêng lẽ: Document model có thể là lựa chọn tốt, relational model vẫn có thể đáp ứng được.
 + Data có nhiều mối quan hệ, mỗi item trong data đều có liên quan lẫn nhau: Document model là lựa chọn không tốt, trong khi đó relational model vẫn chấp nhận được và graph models sẽ thể hiện một cách tự nhiên nhất.
 + Data mix nhiều loại, không quá nhiều cũng không quá ít mối quan hệ: Không ảnh hưởng nhiều lắm vì việc xử lý dữ liệu vẫn có thể thực hiện được. VD nếu cần join ở document model có thể xử lý trên application code, relational model vẫn có thể thao tác graph-query.

## Query language

# Storage and Retrieval
## Data structures: index

Giả sử ta có một database key-value đơn giản được thiết kế bằng đoạn code sau:

```
#!/bin/bash
db_set () {
        echo "$1,$2" >> database
}

db_get () {
        grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
```

Lưu dữ liệu bằng command `db_set key value`
```
$ db_set 123 '{"name":"dog","talk":"bark"}'
$ db_set 46 '{"name":"cat","talk":"mew"}'
$ db_set 46 '{"name":"cat","talk":"mew mew"}'
```

Lấy dữ liệu `db_get key value`

```
$ db_get 46
{"name":"cat","talk":"mew mew"}
```

Kiểm tra database 
```
$ cat database
123,{"name":"dog","talk":"bark"}
46,{"name":"cat","talk":"mew"}
46,{"name":"cat","talk":"mew mew"}
```

- Hoàn toàn có thể sử dụng được và nếu chỉ để lưu trữ thì có thể nói performance khá tốt vì chỉ append vào cuối file, cách hoạt động tương tự như việc log của database nhưng đơn giản hơn vì không có các vấn đề như concurrency control, lấy lại dữ liệu khi log tăng dần, xử lý lỗi, write partial,...
- Data mới sẽ được append-only và file database sẽ ngày một lớn hơn (vd update 1 key 100 lần đồng nghĩa append 100 lần)
- Phải tốn nhiều thời gian để tìm được dữ liệu mong muốn, độ phức tạp là O(n). VD nếu `database` có 2000 dòng thì để lấy được record ở vị trí 2000 (append vào cuối cùng) phải đọc toàn bộ file `database` để kiểm tra (grep) và lấy được dữ liệu. Và cách giải quyết trong trường hợp này là sử dụng index.
- Index là một meta data, sẽ trỏ chính xác vào vị trí data cần tìm

### Hash index

Giả sử ta có một hash table như bên dưới, và hash table này sẽ được lưu trữ trong memory (RAM) để tăng tốc độ truy xuất

![alt text](/static/img/data/ddia_0301.png)

- Mỗi `key` sẽ tương ứng với vị trí `byte offset` trong `database`
- Để lấy dữ liệu của `key` ta chỉ cần lấy `byte offet` trong memory và đọc file `database` từ vị trí `byte offset` => Tiết kiệm được rất nhiều thời gian, chỉ cần 1 disk seek thay vì phải đọc toàn bộ file
- Mỗi khi có thao tác thêm/sửa/xóa dữ liệu trong `database` cũng đồng thời cập nhật lại `index`
- Trên thực tế đây cũng là cách mà Bitcask (default storage engine của Riak) hoạt động

#### Compaction
- Do file `database` (log) hiện tại là `append-only` nên càng có nhiều thay đổi thì file size sẽ càng lớn hơn
- Cách giải quyết sẽ là chia nhỏ file log ra thành nhiều `segments` với một kích thước nhất định, khi một `segment` append đến kích thước giới hạn thì data mới sẽ được append vào file mới.
- Các file segment sẽ được `compact` lại bằng cách lọc bớt dữ liệu, chỉ giữ lại giá trị mới nhất ở mỗi key

![alt text](/static/img/data/ddia_0302.png)

- Quá trình `compact` sẽ lưu dữ liệu vào một file mới, và những `segments` mới này sẽ gọn nhẹ hơn nên có thể merge lại với nhau, quá trình merge và compact có thể thực hiện đồng thời

![alt text](/static/img/data/ddia_0303.png)

- Khi mọi thứ hoàn tất các file `segments` cũ có thể xóa đi, các request read sẽ chuyển sang các file mới.
- Mỗi `segment` mới được tạo ra cũng đồng thời tạo index riêng cho mỗi segment.
- Việc read sẽ đọc trên từng file index bắt đầu từ file mới nhất cho đến khi tìm thấy

#### Issues

Ý tưởng đơn giản, có vẻ hiệu quả nhưng cũng đi kèm với nhiều vấn đề:

- _File format_: CSV có thể sẽ được nhiều người nghĩ đến, nhưng việc lưu trữ bằng binary sẽ cho nhanh và đơn giản hơn.
- _Delete_: do append-only nên để đánh dấu một `key`đã bị xóa, thường `value` của `key` đó sẽ là một giá trị đặc biệt gọi là `tombstone`. Nếu request đến `key` có giá trị `tombstone` sẽ return về `not found`. Và quá trình merge sẽ loại bỏ các `tombstone` này và các giá trị được lưu trữ trước đó (từ thời điểm mark `tombstone` trở về trước).
- _Crash recovery_: Nếu restart database đồng nghĩa với việc hash maps trong memory sẽ mất và dĩ nhiên việc đọc lại toàn bộ `segments` từ đầu để tạo index sẽ tốn rất nhiều thời gian, Bitcask giải quyết bằng cách định kỳ snapshot index xuống file và read lên lại khi restart database.
- _Partially written records_: giả sử database bị crash trong lúc mới chỉ append data được một nữa, sẽ tạo ra corrupt data. Bitcask tạo checksums trước khi append và sẽ check+ignore corrupt data.
- Cách append-only có vẻ lãng phí so với việc overwrite lại các giá trị cũ nhưng xem xét kỹ thì có nhiều ưu điểm:
 - Do append-only nên việc cập nhật dữ liệu hoặc quá trình compact, merge đều là sequential write operations, nhanh hơn so với việc tìm và update - random write.
 - Concurrency & crash recovery cũng sẽ đơn giản hơn, thử tưởng tượng trường hợp đang overwrite data thì database bị crash, đồng nghĩa với việc data cũ & mới đều bị corrupt.
 - Merge segments data file làm cho dữ liệu hạn chế việc bị fragment theo thời gian 
- Hash table phải chứa đủ trong memory, mặc dù có thể lưu ở disk nhưng chi phí random access I/O sẽ cao hơn, đồng thời việc cập nhật hashmap trong các trường hợp như hash collision sẽ phức tạp hơn.
- Không thể range query được (vd lấy từ id = 10 đến id = 100), chỉ có thể gọi và lấy từng key một

### SSTables & LSM-Tree
#### SSTables
- SSTables - Sorted String Table, tương tự như việc lưu log ở trên, append vào segment, nhưng data trong mỗi segment được sắp xếp theo key (sorted by key). Cách lưu trữ này sẽ có các ưu điểm:
 - Quá trình merge & compact diễn ra đơn giản và hiệu quả hơn. Do segment đã sort theo key, nên lúc merge chỉ cần đọc qua các segments theo thứ tự, copy key nhỏ nhất vào new segment. Nếu các key giống nhau thì chỉ lấy key mới nhất (segment mới nhất) và bỏ qua các key còn lại (ảnh bên dưới)
 - Index cho SStable sẽ không cần phải lưu toàn bộ key trong memory, thay vào đó chỉ cần lưu một vài key đánh dấu. VD 
 - Có thể group các nhóm key-value lại thành một block và compress trước khi write xuống disk, mỗi entry index ở trên sẽ trỏ vào vị trí đầu tiên của mỗi compressed block này.

![alt text](/static/img/data/ddia_0304.png)

Các key được sort theo Alphabelt

#### Constructing and maintaining SSTables
- Có thể dùng cấu trúc cây cân bằng để lưu trữ SSTables như red-black trees hoặc AVL trees
- Khi có data mới sẽ được cập nhật vào cây này trong memory - còn gọi là _memtable_
- Khi _memtable_ đạt đến độ lớn nhất định, vd vài MB => Save toàn bộ cây xuống disk => new SStable file (segment). Và đưa data mới vào một cây mới.
- Tiếp tục sẽ có nhiều SSTables files (segments) được tạo ra, và quá trình compact & merge sẽ thu gọn các file này lại
- Khi tìm kiếm dữ liệu sẽ lướt qua _memtable_, nếu không tìm thấy sẽ tiếp tục sang các file segments từ mới đến cũ.
- Do sorted theo key nên có thể dùng range query

#### LSM-Tree
- Các storage engine được xây dựng theo cách merge và compact sorted files được gọi là *Log-Structed Merge* (LSM) storage engine.
- Thuật toán trên được dùng trong LevelDB và RocksDB, Cassandra & HBase cũng sử dụng storage engine tương tự lấy cảm hứng từ Google Bigtable.
- **Ưu**:
 - Tốc độ write nhanh (sequential write)
 - Dữ liệu được compress tốt qua quá trình merge & compact nên tiết kiệm bộ nhớ và tránh được tình trạng fragmention.
- **Khuyết**:
 - Dữ liệu phải được rewrite nhiều lần mỗi khi merge & compact.
 - Tìm kiếm dữ liệu chậm vì phải check nhiều files segments
 - Quá trình merge & compact đôi khi tạo ra áp lực lớn xuống disk, vì việc write chiếm dụng hầu hết disk bandwidth.
 - Nếu quá trình merge & compact không được thực hiện vì lý do nào đó (vd disk bandwidth không đủ để thực thi) sẽ làm cho file segments ngày càng lớn, tốc độ đọc sẽ ngày càng chậm.
 - Mỗi key có thể bị duplicate và nằm ở nhiều segments khác nhau (chưa được merge).

#### Performance optimizations
- Nếu phải tìm kiếm một key không tồn tại trong database sẽ tốn rất nhiều thời gian (phải check memtable và toàn bộ segments để kiểm tra key có tồn tại hay không). Bloom filters sẽ được ứng dụng vào để khắc phục điểm yếu này.
- Có 2 stategies thường dùng để compact & merge SSTables:
 - Size-tiered (Hbase, Cassandra): Các SSTables mới và nhỏ hơn sẽ được merged liên tiếp vào các SSTables cũ và lớn hơn.
 - Leveled (LevelDB, RocksDB, Cassandra): SSTables được chia nhỏ theo key range và dữ liệu cũ được chia theo nhiều "levels" khác nhau.

### B-Trees

- Được giới thiệu từ năm 1970 và trở thành Indexing structure được sử dụng phổ biến nhất.
- Dùng trong rất nhiều loại database như MySQL, PostgreSQL, Oracle, SQLite,...
- Tương tự như LSM-Tree, B-trees cũng lưu sorted key-value nhưng thay vì lưu thành những file segments liên tiếp có dung lượng khoảng vài MBs, B-Trees chia nhỏ dữ liệu thành các fixed-size blocks hoặc pages, thông thường là 4KB, read/write sẽ thực hiện trên toàn page 1 lúc. Thiết kế này giống với ổ cứng cũng sắp xếp dữ liệu theo các fixed-size blocks.
- Mỗi page được xác định bằng địa chỉ, một page có thể trỏ đến những child pages. Mỗi child page lưu trữ một dãy keys, và mỗi reference giữa các 2 keys trỏ đến child page chứa các key nằm giữ 2 keys đó.
- Một page được gọi là root của B-tree, mọi hành động tìm kiếm đều bắt nguồn từ page này.
- Leaf page sẽ chứa value của key cần tìm
- Update dữ liệu => tìm leaf page chứa key, change value của page và write page ngược lại disk.
- Thêm dữ liệu => tìm pages chứa range keys tương ứng với key đó và add vào page, nếu page không đủ vùng nhớ sẽ tách ra thành 2 pages mới và parent page sẽ thêm một key với 2 references trỏ vào 2 pages con mới => Luôn đảm bảo cây được cân bằng.

#### Reliable
- Page luôn được overwrite mỗi khi có dữ liệu mới, có thể một hoặc nhiều pages được overwrite sau mỗi operation.
- Nếu trong quá trình overwrite mà database bị crash => corrupted index 
- Để giải quyết thông thường B-Tree sẽ được kèm theo một data structure trên disk: write-ahead log (WAL aka redo log), append-only file, trước khi B-Tree thay đổi page file, sẽ có một log tương ứng phát sinh. Khi database crash, đoạn log từ WAL sẽ dược dùng để restore B-Tree.
- Khác với LSM-tree, ở B-Tree mỗi key được lưu 1 lần duy nhất nên sẽ tiết kiệm được bộ nhớ.

#### Optimizations
- B-tree phải write data 2 lần: 1 cho WAL và 1 cho tree pages (và có thể thêm 1 lần nữa nếu phải chia nhỏ page) Tuy nhiên do ra đời rất lâu nên đã có rất nhiều biện pháp cải tiến được nghiên cứu và áp dụng, một trong số đó như:
- Thay vì overwrite pages và dùng WAL cho crash recovery, một số database như LMDB sử dụng copy-on-write, một page mới sẽ được tạo ra và reference cũ sẽ được trỏ đến page mới.
- Để tăng tốc độ đọc (giảm random read, tăng sequential read), có thể giữ cho các key liên tiếp nhau nằm ở gần nhau trong ổ cứng.
- Bổ sung thêm pointer để giảm chi phí scanning, vd bổ sung thêm reference đến các pages liền kề.

### B-Tree or LSM-tree?
- Trên lý thuyết mà nói LSM-Tree nhanh hơn khi write và B-Tree nhanh hơn khi reads, vì LSM-Tree phải check rất nhiều data structure để lấy được thông tin.

### Other Indexing Structures
#### Stroring values within the index

- Mỗi cặp key-value thay vì lưu value có thể lưu một reference đến một nơi khác chứa data, vùng chứa data này được gọi là **heapfile**
- **Heapfile** sẽ giúp hạn chế được việc duplicate dữ liệu khi có từ 2 indexes trở lên tồn tại, mỗi index sẽ chỉ trỏ đến một **heapfile** thay vì phải nhân đôi dữ liệu ở cả 2 index.
- Và dĩ nhiên quá trình update cũng thuận lợi hơn, chỉ cần update ở 1 nơi duy nhất.
- Trong một vài trường hợp, từ file index phải jump qua rất nhiều node để đi đến được **heapfile**. Do đó, để tiết kiệm thời gian, một key có thể lưu index trực tiếp đến một index khác, và index này sẽ trỏ thẳng xuống heapfile được gọi là `Clustered index`. VD trong MySQL's InnoDB storage engine, primary key là clustered index, and các index khác sẽ trỏ đến primary key thay vì trỏ đến heapfile.

#### Multi-column indexes
- Các thông tin bên trên chỉ xét đến việc index trong một column, còn với nhu cầu index multi-column sẽ khác.
- Cách phổ biến nhất gọi là **concatenated index**, tập hợp nhiều fields vào trong một key, và các fields sẽ được nối lại với nhau theo thứ tự. VD index cho một table `users` với 2 columns `firstname`, `lastname` => multi-column index `users_firstname_lastname_idx(firstname,lastname)`, việc tìm kiếm đầu tiên sẽ tìm theo index `firstname` rồi đến `lastname`, tuy nhiên sẽ không thể tìm bắt đầu từ `lastname`.
- Ngoài ra còn nhiều cách khác như sử dụng R-Tree cho việc index geolocation (PostGIS) 

#### Full-text search & fuzzy indexes
- Khác với các loại index khác sẽ tìm một giá trị chính xác, full-text search chỉ để tìm một hoặc một vài từ trong document, fuzzy search để tìm các từ giống hoặc _tương tự_ (vd do viết sai chính tả)
- Lucene sử dụng một cấu trúc tương tự như SSTable để làm index full-text
- Fuzzy search đòi hỏi phải sử dụng nhiều kỹ thuật hơn và thuộc về nhóm document classification và machine learning.

## Transaction Processing or Analytic

- Transaction: Access pattern phổ biến nhất được gọi là **Online transaction processing (OLTP)**, bắt nguồn từ commercial transaction phục vụ cho việc đặt hàng, thanh toán,... và mở rộng ra hơn thành khái niệm logic về read và write, hoạt động của database được diễn ra thông qua sự tương tác của application hay đúng hơn là input từ phía người dùng.
- Analytic: Access pattern này được gọi là **Online analytic processing (OLAP)**. Data ngày càng lớn và phát sinh nhu cầu trong việc phân tích nguồn data này để tìm ra insight cần thiết cho bussiness. Thường phải scan qua toàn bộ records để có được những tính toán thống kê như đếm, tình tổng, trung bình,... trên một hoặc nhiều columns, từ đây đưa ra những report phục vụ cho _business intelligent_.

| Property        | OLTP           | OLAP |
| --------------- |:--------------:| -----:|
| Main read pattern 	| Small number of records per query, fetched by key | Aggregate over large number of records 	|
| Main write pattern 	| Random-access, low-latency writes from user input | Bulk import (ETL) or event stream			|
| Primarily used by 	| End user/customer, via web application | Internal analyst, for decision support	|
| What data represents 	| Latest state of data (current point in time)		| History of events that happened over time |
| Dataset size		| Gigabytes to terabytes | Terabytes to petabytes					|

### Datawarehouse && Schemas for Analytic 
#### Datawarehouse
- Quá trình xử lý dữ liệu của OLAP thường tốn khá nhiều tài nguyên (vd những thống kê cần query toàn bộ data, tính tổng, trung bình,...), nếu cùng truy xuất vào chung dữ liệu với OLTP sẽ ảnh hưởng đến việc đảm bảo cho OLTP low latency và high avaibility.
- Bên cạnh đó với hệ thống phức tạp data thường phân bổ ở nhiều hệ thống nhỏ cũng gây nhiều khó khăn cho việc tính toán.
=> Datawarehouse ra đời với mục đích phân tách và tổng hợp dữ liệu phục vụ riêng cho OLAP và hầu hết data model của datawarehouse là Relational vì SQL thích hợp và thuận tiện hơn cho các analytic query.

#### Schemas for analytic

- Thường datawarehouse sử dụng schema gọi là **star schema** (dimensional modeling). Sẽ có một table gọi là _fact table_, là table trung tâm, chứa toàn bộ mọi event trong hệ thống, mọi thứ đều được lưu ở dây. Mỗi row sẽ chứa các identity cần thiết trỏ đến các table con, như VD bên dưới

![alt text](/static/img/data/ddia_0309.png)

- Mỗi fact table có thể có dung lượng rất lớn, đồng thời có thể có hơn 100 columns đủ để có thể trả lời được câu hỏi _who, what, when, where, how & why_ của event đó.
- Một số column trong fact table có thể là attribute column (price, amount,...), phần còn lại thường là foreign keys trỏ đến các table vệ tinh (dimension table).
- Ngoài ra còn có biến thể khác của star schema gọi là **snowflake schema**. Thay vì mỗi dimension table sẽ được chia nhỏ thành các sub dimension tables thay vì chỉ lưu string data ở một table. Thường thì star schema sẽ được dùng nhiều hơn vì thuận tiện hơn cho việc analytic.

## Column-Oriented storage
### Idea
- Khi cần query từ fact table, hầu hết trường hợp không sử dụng hết data trong toàn bộ columns, vd fact table có 100 columns, và cần tính tổng doanh thu trong 1 ngày, query chỉ quan tâm đến 2 column là price và date, theo thông thường sẽ có 2 index ở 2 column này. Nhưng với cách lưu trữ data như trên, mỗi row sẽ được lưu cạnh nhau, thì cần phải load, parse, filter toàn bộ 100 column còn lại => Tốn thời gian và tài nguyên không cần thiết.
- Column-Oriented storage là giải pháp cho vấn đề này: data trong mỗi column sẽ được lưu chung trong một file, mỗi column tương ứng với một file nhất định. => Query chỉ load, parse, filter trong 1 file.

![alt text](/static/img/data/ddia_0310.png)

### Column compression
- Do data đã được lưu theo cột nên có thể compress dữ liệu lại bằng bitmap encoding.
- Bên cạnh việc tiết kiệm được bộ nhớ lưu trữ, còn giúp tiết kiệm được memory bandwidth và tối ưu được CPU (vectorized processing)

### Sort order
- Có thể make index bằng cách tương tự như SSTable, tuy nhiên không thể sort theo mỗi column độc lập khác nhau vì data lưu theo cột đồng nghĩa với việc chỉ biết một item ở vị trí `n` ở column này cùng hàng với item ở vị trí `n` của column kia, nếu muốn sort phải reconstruct lại toàn bộ data theo thứ tự sort mới.
- Do đó phải chọn trước column muốn sort khi tạo table và có chiến lược phù hợp để đáp ứng được nhu cầu trong tương lai

### Writing data

- Column-Oriented storage, compression, sorting giúp cho việc query read nhanh và hiệu quả hơn, tuy nhiên lại mang đến bất lợi cho việc insert. VD insert một giá trị trung bình vào giữa column price sẽ phải reconstruct lại toàn bộ các giá trị khác.
- Giải pháp được đưa ra bằng cách dùng LSM-Trees, write to memory, bulk write vaf merges với các column files trên disk. Đây cũng là cách mà Vertica sử dụng.

### Aggregation: Data Cubes and Materialized Views

- Nhiều query như COUNT, SUM, AVG, MIN, MAX... thường đòi hỏi phải scan qua toàn bộ dữ liệu để có thể tính được kết quả mong muốn và hiển nhiên phải tốn nhiều thời gian và tài nguyên.
- Data cubes: tạo sẳn một `cube` dữ liệu mới dựa trên hai hoặc nhiều dimensions. Vd như table bên dưới, là số lượng sản phẩm bán ra của mỗi `product_id` tương ứng với một `date_id` và `total` ở mỗi column/row tương ứng với tổng cộng số sản phẩm bán ra ở mỗi column/row.

```
	product_id
+-------+------+------+------+-----+-------+
|       |  31  |  32  |  33  | ... | total |
+-------+------+------+------+-----+-------+
| 1411  |   44 |   56 |   88 | ... | 233   |
+-------+------+------+------+-----+-------+
| 1412  |  123 |  344 |  121 | ... | 785   |
+-------+------+------+------+-----+-------+
| 1413  |  273 |   22 |   11 | ... | 992   |
+-------+------+------+------+-----+-------+
| ...   |  ... |  ... |  ... | ... | ...   |
+-------+------+------+------+-----+-------+
| total | 2344 | 3432 | 3452 | ... | TOTAL |
+-------+------+------+------+-----+-------+
date_id

```

- Tuy nhiên cách trên chỉ đáp ứng được cho một vài câu query nhất định
- Materialized views: Kết quả của một câu query sẽ được lưu lại thành một temporary table, giống như `view` nhưng thay vì phải query aggregate lại data thì data sẽ được lưu lại riêng.

# Encoding & Evolution
- Việc chuyển đổi từ in-memory sang byte được gọi là encoding (serialization, marshalling) và ngược lại gọi là decoding (deserialization, unmarshalling)
- XML, JSON và CSV được sử dụng phổ biến mặc dù vẫn tồn tại nhiều khuyết điểm:
 - XML không thể phân biệt giữa string và number, JSON không thể phân biệt giữa integer và float
 - JSON, XML không support binary string. Mặc dù có thể chuyển thành string base64, nhưng đổi lại dung lượng tăng lên khoảng 33% so với ban đầu.
 - CSV không có schema mà phụ thuộc vào việc định nghĩa column ở tầng application. Format cũng khá mơ hồ và cũng phụ thuộc vào parser.
- Binary encoding được sử dụng để convert raw data (human readable) sang binary để tiết kiệm bộ nhớ và phần nào tăng tốc độc parse. Cách tiếp cận cơ bản nhất là encode object thành binary, ngoài ra cách khác là Thrift và Protocol Buffer.
- VD encode JSON sau:

```
{
    "userName": "Martin",
    "favoriteNumber": 1337,
    "interests": ["daydreaming", "hacking"]
}
```

![alt text](/static/img/data/ddia_0401.png)

## Thrift & Protocol Buffer
- Open source 08/2007
- Cần schema cho việc encode.
- Thrift:

 ```
struct Person {
	1: required string userName,
	2: optional i64 favoriteNumber, 
	3: optional list<string> interests
}
 ```

- Protocol Buffer:

 ```
message Person {
        required string user_name       = 1;
        optional int64  favorite_number = 2;
        repeated string interests       = 3;
}
 ```

- Cung cấp code generation tool để encode/decode.
- Thrift có 2 binary encoding format: BinaryProtocol & CompactProtocol.
 - BinaryProtocol: tương tự như việc encode binary thông thường, nhưng thay vì encode cả field `key` _(userName, favoriteNumber, interests)_ thì chỉ đánh dấu bằng tag number.
 - CompactProtocol: Giống như BinaryProtocol nhưng data giống nhau sẽ được nén lại (compact)

![alt text](/static/img/data/ddia_0403.png)

- Protocol Buffer cũng encode tương tự như CompactProtocol và binary code không lưu các thông tin của field `required` hoặc `optional`, các field này chỉ được check ở runtime (code generation tool)

![alt text](/static/img/data/ddia_0404.png)

- Cả 2 dựa vào field _tags_ để encode/decode nên việc thay đổi schema sẽ dễ dàng (evolution):
 - Nếu có field mới được thêm vào schema (new version), thì old code (generated by old schema) chỉ việc skip qua.
 - Backward compatible? New code (generated by new schema) có thể đọc được schema cũ, và ngược lại. Chỉ trong trường hợp new schema có bổ sung thêm _required_ field và hiển nhiên old schema lúc send đi không có field đó và sẽ không pass được bước validate khi nhận ở New code.
 - Change datatype: hoàn toàn được, tuy nhiên khá nguy hiểm và có thể bị mất dữ liệu (vd change từ int64 sang int32), hoặc raise error nếu new data type không compatible (vd từ string sang int).
 - Protocol Buffer sử dụng `repeat` để biểu thị cho `list` hoặc `array` và binary code thực chất chỉ lưu _n_ records nhiều lần. Do đó khi change một `optional` type (single valued) sang `repeated` (multi-valued), và new code sẽ đọc được bình thường, xem như đó là 1 array với 1 phần tử.
 - Thrift có kiểu `list` riêng nên sẽ không thể thay đổi data type như Protocol Buffer, tuy nhiên sẽ hổ trợ nested list.

## Arvo
- Apache Arvo sinh sau đẻ muộn (2009) và mục đích phát triển ban đầu là một subproject của Hadoop.
- Sử dụng schema để encode và có 2 loại schema language:
- Arvo IDL (easy for human editing):

	 ```
	 record Person {
		string userName;
		union { null, long } favoriteNumber = null;
		array<string> interests;
	}
	 ```

- JSON (easily machine readable)

	```
	{
		"type": "record",
       		"name": "Person",
        	"fields": [
			{"name": "userName", "type": "string"},
			{"name": "favoriteNumber", "type": ["null", "long"], "default": null},
			{"name": "interests", "type": {"type": "array", "items": "string"}}
		] }
	```

- Có kích thước nhỏ nhất so với các kiểu encoding bên trên khi encode sang binary (32 bytes)

![alt text](/static/img/data/ddia_0405.png)

- Không có tag number nên để decode được Arvo phải dùng chính xác schema đã encode ban đầu để decode ngược lại.
- Để đáp ứng được schema evolution, Arvo chỉ yêu cầu writer's schema và reader's schema compatible với nhau.

![alt text](/static/img/data/ddia_0406.png)
