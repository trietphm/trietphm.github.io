+++
author = "Triet Pham"
date = "2017-12-12T07:23:58+07:00"
description = ""
tags = ["log","go", "golang","logrus","lumberjack", "tech"]
title = "Log trong Go và các vấn đề về log"

+++

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/9nzvr3r9bm_log-yard.jpg)

# Vì sao cần phải log và cần phải log một cách chỉnh chu?

## Log là gì?

- Hiểu một cách đơn giản thì log là những thứ dùng để lưu vết, những thông tin được thông báo, lưu lại trong quá trình hoạt động của một ứng dụng.
- Thế ghi log như thế nào? Có thể output ra màn hình, lưu vào file, stdout đâu đó để có thể xem lại được.
- Ghi log để làm gì? Dĩ nhiên là để có thể xem lại các thông tin hoạt động của ứng dụng trong quá khứ nhằm nhiều mục đích như debug, check health, xem info, xem lỗi, warning,...
- Nhưng ghi log là ghi gì? Cách đơn giản nhất là ghi lại toàn bộ thông tin nào bạn nghĩ là cần thiết để phục vụ cho các mục đích bên trên.

## Vì sao cần viết log có tâm?

- Nếu bạn đã từng làm việc với những files log thể nào bạn cũng sẽ phải ăn ít nhất một trong những đống hành này:
 - File log đó to quááááááááá (file to quá nên có tiếng vọng echo), làm sao để xem giờ T_T.
 - Một mớ thông tin hỗn độn như spaghetti, đủ mọi thông tin từ Trái Đất tới sao Hỏa.
 - Và bạn cũng chẳng thể nào biết được mớ thông tin trên đến từ đâu, từ ngữ cảnh nào, user nào,...
 - Và bạn muốn search cũng không search được, toàn raw text và chẳng theo cấu trúc nào.
 - Đứa ngớ ngẩn nào đó log ra những đoạn trong có vẻ nguy hiểm: _It works! Let's have some beer_ nhưng hoàn toàn vô dụng.
 - Đứa ít ngớ ngẩn hơn đã log ra cái lỗi nhưng chả thể nào biết được lỗi đó đến từ không gian thứ nguyên nào hay làm sao để preroduce, it said _Null pointer exception, you die wahaha_.
 - Nếu các file đến từ nhiều server, services, application thì, well, _let's have some beer_.
 - Bạn có một file log nặng 50GB được lưu trong 1 tháng và ai đó nhờ bạn check giùm log của một ngày giữa tháng, well, _wanna go grab some beer?_
 - ...
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/fc4jol71hl_Strips-Fucking-title-550-finalenglish.jpg)

# Log như thế nào?
## Log gì
- Việc đầu tiên trước khi output dòng log ra hãy tưởng tượng sau đó có thể sử dụng được không, hay chỉ là thông tin vô nghĩa. Đừng tự đánh lừa mình rằng _"Trust me, tôi biết tôi đang ghi gì ra mà, một vài ký tự viết tắt thôi, ez"_, dám cá là bạn sẽ quên ngay 5' sau đó. Hãy log nhiều thông tin nhất có thể, toàn bộ ngữ cảnh trong trường hợp đó.
- VD bạn cần log lại một request HTTP có response 500 Internal server error, thì thông tin ở đây ít nhất phải có:
 - Thời gian
 - HTTP request info: header, request, body,...
 - HTTP response info
 - Các thông tin trên tương đối đủ để bạn có thể reproduce lại, tuy nhiên tốt hơn nên có đầy đủ stack trace về error đó như lỗi ở đoạn nào, dòng nào, lỗi gì, input như thế nào,...

## Phân loại log
- Log có thể xem là một dạng raw database nên cũng nên được phân loại tùy theo mục đích sử dụng, nhưng ít nhất nên được chia theo level:
 - **INFO**: các thông tin mà bạn muốn collect thêm trong quá trình hoạt động của application. VD: log http request (link, status, duration,...) để biết traffic thế nào
 - **DEBUG**: các thông tin dùng để debug, hiển nhiên, có thể bật/tắt log này dựa vào mode của application. Bạn sẽ nghĩ cái nào debug log viết xong thì xóa chứ để làm gì nhỉ? Đúng là vậy, nhưng vẫn sẽ có một vài trường hợp cần thiết như quá trình đặt log debug tốn quá nhiều thời gian nếu làm rồi xóa đi rồi lại làm lại, hoặc một vài trường hợp hy hữu cần debug trên production (lol, vẫn có nhé), nhưng dĩ nhiên là không phải sửa code thẳng trên server mà chỉ bật/tắt debug mode thôi.
 - **WARNING**: hơi khó phân biệt với error. Ở đây cá nhân mình thường log những thứ nguy hiểm, lỗi ít hoặc không gây nguy hiểm đến ứng dụng nhưng cần được fix sớm trong tương lai. VD bạn đang load sản phẩm ra từ trong Database nhưng lúc đó phát hiện user tạo sản phẩm đó không còn tồn tại trong Database, thay vì báo lỗi, bạn bùa phép trả về một user default nào đó và log warning lại, một dạng auto heal.
 - **ERROR**: quá rõ ràng, error và toàn bộ thông tin liên quan nhiều nhất có thể để có thể reproduce lại được mà ít tốn thời gian nhất.

## Format log

- Một điều quan trọng là log đó phải sử dụng được và dễ sử dụng: readable.
- Log nên được format theo định dạng của các standard library logger để có thể consume được.
- Có nhiều ứng dụng phục vụ cho việc collect data và analyze để có thể thống kê, search,... từ log đó như Logstash, Fluentd, Graylog,... Và file log nên follow theo format input của các ứng dụng này.
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/ynzg6tvzqt_Strip-Log-nonstandard-650-fianlenglish-1.jpg)
## Log rotate

- Log rotate là việc cắt nhỏ log ra và lưu trữ trên nhiều file thay vì một file, có thể sẽ lưu log riêng theo từng ngày, tuần hoặc tháng. Bạn sẽ có nhiều file dạng như: _mylog-20171201.log, mylog-20171202.log, mylog-20171202.log,..._
- Hoặc có thể file log sẽ cắt theo chiến lược khác như dung lượng file (vd tối đa 1GB), hoặc tùy theo mục đích cụ thể riêng của ứng dụng.
- Có thể thực hiện bằng nhiều cách như dùng logrotate trong linux hoặc một số logger library cũng support việc này.

# Sử dụng Logrus trong Go

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/ul3l86ba3i_Screen%20Shot%202017-12-12%20at%2010.22.55%20PM.png)

- Trong Golang built-in package đã có hổ trợ việc log https://golang.org/pkg/log/ nhưng khá đơn giản và không đáp ứng đủ những nhu cầu như trên.
- Logrus (https://github.com/sirupsen/logrus) là một structered logger cho Golang support hầu hết những thứ trên và output ra màu mè khá đẹp
![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/55immwl25d_687474703a2f2f692e696d6775722e636f6d2f505937714d77642e706e67.png)

Sử dụng khá dễ dàng, một ví dụ nhỏ:

```
package main

import (
  log "github.com/sirupsen/logrus"
)

func main() {
  log.WithFields(log.Fields{
    "animal": "walrus",
  }).Info("A walrus appears")
}
```

## Formater

Logrus support format với 2 định dạng là:
- `logrus.TextFormatter`: Log ra dạng plain text, nếu stdout là tty sẽ có bổ sung thêm color.
- `logrus.JSONFormatter`: Log ra theo dạng json

```
	log.SetFormatter(&log.TextFormatter{})
	log.SetFormatter(&log.JSONFormatter{})
```

## Structed log
Để đảm bảo log luôn được formated và parseable nên logrus bắt buộc log thông qua function `WithFields`

```
package main

import (
	"os"

	"github.com/sirupsen/logrus"
)

func main() {
	logrus.SetFormatter(&logrus.TextFormatter{})
	//log.SetFormatter(&log.JSONFormatter{})
	var log = logrus.New()
	log.Out = os.Stdout

	// Log to file
	// file, err := os.OpenFile("logrus.log", os.O_CREATE|os.O_WRONLY, 0666)
	// if err == nil {
	// 	log.Out = file
	// } else {
	// 	log.Info("Failed to log to file, using default stderr")
	// }

	log.WithFields(logrus.Fields{
		"event": "event",
		"topic": "topic",
		"key":   "key",
	}).Fatal("Failed to send event")
}
```

Bằng cách này, dù muốn hay không log khi được lưu lại đều có cùng định dạng, ví dụ với đoạn code trên sẽ cho output là:
- Text format (tty):

```
FATA[0000] Failed to send event                          event=event key=key topic=topic
```

- Text format (file):

```
time="2017-12-12T20:08:20+07:00" level=fatal msg="Failed to send event" event=event key=key topic=topic
```

- Json format:

```
{"event":"event","key":"key","level":"fatal","msg":"Failed to send event","time":"2017-12-12T20:03:29+07:00","topic":"topic"}
```

Mặc định logrus sẽ bổ sung các field `time`, `level`, `msg` vào trong output log, khá tiện lợi.

## Level logging
Logrus support 6 loại level: Debug, Info, Warn, Error, Fatal, Panic. Cách sử dụng như bên dưới:

```
log.Debug("Useful debugging information.")
log.Info("Something noteworthy happened!")
log.Warn("You should probably take a look at this.")
log.Error("Something failed but I'm not quitting.")
// Calls os.Exit(1) after logging
log.Fatal("Bye.")
// Calls panic() after logging
log.Panic("I'm bailing.")
```

## Log rotation
Đáng tiếc Logrus không support cái này, tuy nhiên bạn có thể dùng một thằng khác như [lumberjack](https://github.com/natefinch/lumberjack) mặc dù được khuyến khích là nên dùng `logrotate` chứ không nên xử lý ở tầng application, cơ mà mình thích thì mình dùng thôi, hơ hơ.

## Hook

Có thể add hook tùy theo logging level, ví dụ gửi error đến một tracking service nào đó như BugSnag, ElasticSearch, Fluentd, Logstash,... có rất nhiều library đi kèm, có thể xem thêm ở đây https://github.com/sirupsen/logrus#hooks

## Thread safe

- Logrus default sử dụng mutex để đảm bảo cho việc concurrent write (có thể tắt nếu cần).
- Logger out sử dụng `0_APPEND` flag để write và mỗi lượt write nhỏ hởn 4K cho phép ghi multi-thread/multi-process (Refer to http://www.notthewizard.com/2014/06/17/are-files-appends-really-atomic/)
