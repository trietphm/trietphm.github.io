+++
author = "Triet Pham"
date = "2017-08-27T15:45:17+07:00"
description = "Chuyện giao tiếp"
tags = ["talk"]
title = "Chuyện giao tiếp"

+++

![alt text](/static/img/chuyen-giao-tiep/img6.jpg)
# Giao tiếp 

> Lời nói không mất tiền mua, lựa lời mà nói cho vừa lòng nhau

Giao tiếp là chuyện rất bình thường nhưng lại có thể vô tình trở thành ngọn nguồn của rất nhiều vấn đề. 
Trong khuôn khổ bài viết sẽ nói đến giao tiếp sẽ ảnh hưởng thế nào đến việc phát triển phần mềm, và có thể có ít nhiều liên quan đến các việc khác (hoặc không).

> TL;DR
> Hãy giao tiếp như một team

# Ngữ cảnh

Để mô tả các vấn đề một cách dễ nhất cùng nhau đi qua câu chuyện như sau:

> Công ty X vừa nhận được một dự án xây dựng một mạng xã hội phú ông, hệ thống phần mềm cần làm là service API, app iOS và một cái web.
> App này mục đích chính là để các phú ông phú bà khoe của và một vài chức năng _nho nhỏ_ khác

- **Ông Trum aka Boss** (là người Việt họ Ông tên Trùm, tên nước ngoài không dấu là Trum): phú ông ở làng Phù Thủng người bỏ tiền ra cho dự án và kiêm luôn quyết định chức năng của App.
- **Mr.Tony**: là người quản lý mà phú ông thuê từ bên Mẽo về để hổ trợ quản lý dự án và Tony không biết nói tiếng Việt.
- **Tí**: Backend developer, chịu trách nhiệm làm API, sống ở làng Phù Đổng, tốt nghiệp cấp 3 trường làng biết code nhưng tiếng Anh lại bập bõm.
- **Tèo**: iOS developer, nhà giàu có điều kiện nên du học ở Ả Rập từ bé, nói tiếng Anh như gió. là iFan chân chính nên anh học code iOS cho đủ bài.
- **Bạch Tuyết**: Designer, sống trong rừng với 7 chú lùn, ban ngày rãnh rỗi nên Tuyết nhận thêm việc design freelance kiếm thêm chút tiền mua táo về pha detox uống giảm cân.
- **Bờm**: Bò mất giá nên anh không chăn bò nữa mà chuyển sang làm Quản lý ở công ty X aka project manager, anh là quản lý cho dự án này.

# Quy trình hoạt động 

[Ông Trum] >-----> [Mr.Tony] >-----> [Bờm] + [Tuyết] >-----> [Tí] + [Tèo]

Cụ thể như sau:
- **Ông Trum** đưa ra yêu chung về ứng dụng, các chức năng mong muốn.
- **Mr.Tony**: phân tích yêu cầu chức năng và đưa ra các yêu cầu về kỹ thuật, tiến độ, thời gian, chất lượng.
- **Bờm**: nhận yêu cầu từ Tony, phân tích và lập kế hoạch phát triển. Rà soát và làm rõ các yêu cầu của ứng dụng, có thể coi như kiêm luôn Business Analyst
- **Tuyết**: xem xét yêu cầu và design.
- **Tí**: nhận design từ Bạch Tuyết và yêu cầu từ Bờm và code app, các yêu cầu nào cần đến API thì request Tèo cung cấp API.
- **Tèo**: nhận yêu cầu từ Bờm và bắt đầu làm API, viết documents cho API để Tí làm việc dễ dàng hơn. Tèo sẽ phải bắt đầu công việc trước để Tí có đủ API cần thiết để đảm bảo tiến độ.

Mọi người giao tiếp với nhau qua **Skype**

# Các vấn đề phát sinh
### 1. My teammates are ninjas
#### Vấn đề: 

![alt text](/static/img/chuyen-giao-tiep/img7.jpg)

Tèo có thắc mắc về design nên nhắn tin qua Skype cho Bạch Tuyết

	- [09:00 AM] Tèo: Chị Tuyết ơi, cho em xin link file thiết kế màn hình Login
	- <2h30 sau>
	- [11:30 AM] Bạch Tuyết: Đây em https://tinyurl.com/DesignLogin
	- [11:31 AM] Tèo: link bị 404 chị ơi :(
	- <2h30 nữa>
	- [02:00 PM] Bạch Tuyết: chị gửi lộn, link này nha https:/tinyurl.com/DesignLoginFinal
	- [02:01 PM] Tèo: Cám ơn chị

Và Tèo tốn 5h mòn mỏi đợi chờ. Đã có design, Tèo bắt tay vào làm, thử đăng nhập bằng API thì bị lỗi.

	- [02:05 PM] Tèo: Tí ơi, API lỗi rồi, đăng nhập nó toàn báo status 500, `Internal server error`
	- <1h sau>
	- [03:05 PM] Tí: Phải gửi thêm mã OTP nữa.
	- [03:06 PM] Tèo: Ờ, được rồi, sao không thấy ghi trong documents?
	- [03:07 PM] Tèo: ơ, đăng nhập lần nữa thì nó báo status 403, `Account is banned`, sao kỳ vậy :( 
	- <2h sau> (Tí seen nhưng thấy báo bug nên không thèm trả lời)
	- [05:00 PM] Tí: Gửi sai mã OTP, là bị khóa account nhé.
	- [05:00 PM] Tèo: Sao không ghi trong documents??
	- [05:01 PM] Tí: Quên
	
Và Tèo mất toi 8h lãng nhách chỉ vì ngồi đợi reply của team. 

#### Vấn đề: 
 + Teammates như ninja, thoắc ẩn thoắc hiện, không bao giờ thấy online, hỏi không trả lời,...
 + Lãng phí thời gian của team => trễ tiến độ => OT.
 + Gây mất lòng tin lẫn nhau, quá mệt mỏi vì đợi chờ.

#### Giải pháp:
 - Chửi (lầm thầm hoặc thẳng mặt), lần sau nó hỏi thì cho nó đợi lại cho biết mặt, cho chừa tội làm trễ deadline của mình. Làm ngược lại là được.
 - Góp ý nhẹ nhàng hoặc nên có các biện pháp ràng buộc để mọi người luôn luôn cố gắng trả lời tin nhắn sớm nhất có thể, tốt nhất là trong vòng 5', hạn chế làm trễ nãy công việc của nhau

### 2. Sử dụng kênh chat không hợp lý
#### Vấn đề: 

![alt text](/static/img/chuyen-giao-tiep/img2.jpg)

	- Tèo cần thông tin về API tìm kiếm user (không có trong documents) và nhớ mang máng Tí đã gửi đâu đó trong đoạn chat group trên Skype, và bắt đầu scroll lên tìm kiếm đoạn chat đó, phải mất 30' Tèo mới tìm ra.
	- Yêu cầu app có thay đổi và Bờm thông báo lên group chat, mọi người trao đổi một đoạn dài dài. Tí đi chơi với bạn gái về thấy dài quá, lười đọc, bỏ luôn. Tèo vì thấy group chat phiền quá, mấy đoạn hội thoại không liên quan đến mình nên tắt notification luôn => Vài hôm sau phát hiện ra code không đúng yêu cầu, OT ngồi sửa lại.
	- Bờm khiển trách và yêu cầu 2 người đọc lại chat log. Tí & Tèo bỏ 30' ngồi đọc đoạn chat, trao đổi phản biện và đã hiểu được yêu cầu, mặc dù mỗi người hiểu mỗi ý khác nhau?!

![alt text](/static/img/chuyen-giao-tiep/img1.png)

#### Giải pháp:

 - Hạn chế document trên chat log, nếu điều kiện không cho phép thì nên note lại nhanh các thông tin kết luận 
 - Nên tổng hợp lại các ý đã thảo luận để tránh tốn thời gian xem lại chat log mà chưa chắc đã nhận được kết quả mong muốn (VD như trường hợp trên nếu Bờm chủ động tổng hợp lại sẽ đỡ tốn thời gian của cả Tí và Tèo - 1h ngồi xem)
 - Sử dụng các kênh chat nào có notification hợp lý, vừa đảm bảo team nắm được thông tin, vừa đảm bảo không bị phân tâm vì quá nhiều notification.
 - Slack khá tốt trong trường hợp này với các tính năng như notification theo cá nhân, pin post, link documents, snippet,... Skype có nhiều cải thiện nhưng vẫn khá tệ (vd không thể phân biệt khi nào notification do người khác tag mình hay notification từ message hay các loại notification khác)

### 3. Luôn luôn lịch sử và hòa nhã
#### Vấn đề:

	- Bờm vốn là dân chăn bò nên hơi thẳng tính, mỗi khi có gì không vừa lòng Bờm chửi ngay. "Không hiểu à? Gì ngu như con bò vậy, à không, mấy con bò anh nói một tiếng là tụi nó đi theo liền nhé."
	- Mọi người trong team biết tính Bờm nhưng làm gì có ai bị chửi mà vui, lại còn chửi nặng.

#### Giải pháp:

 - We are a team, chửi nhau làm chi?
 - Hãy hòa nhã và lịch sự, thẳng thắng là tốt nhưng đừng thẳng quá, hãy luôn kiên nhẫn và mềm dẻo.
 - Sử dụng từ ngữ khôn khéo, nhẹ nhàng và tình cảm, thay vì luôn đẩy trách nhiệm về đối phương (vd "Chổ đó dễ ợt mà em không hiểu à?", "phần đó *em làm* có bug kìa, sửa lại đi, ẩu quá",...) thì nên giảm nhẹ lại ("Có phần nào anh nói chưa rõ không?", "Phần đó hình như có bug, em kiểm tra giúp anh nhé, nó bị abc... lẽ ra phải là xyz..., cám ơn em nhé")

### 4. I'm not mind reader!
#### Vấn đề: 
![alt text](/static/img/chuyen-giao-tiep/img3.jpg)

	- Bờm: @Tí ơi, bug <ném cho cái screen shoot>
	- Tí: Đâu anh, em có thấy gì đâu
	- Bờm: Đó, anh click vào cái *button đó* nó không ra gì hết.
	- Tí: button nào anh, có 5 cái button trên đó lận.
	- Bờm: Button màu đỏ đó
	- Tí: Button màu đó nào anh, 3 cái cancel/stop/burn anh nói cái nút nào
	- Bờm: Trời ơi, cái nút burn đó, sao chậm tiêu quá vậy, hỏi tới hỏi lui nữa, anh nói ngắn gọn em phải hiểu chứ
	- Tí: Dạ... (ai biết ông nghĩ gì mà hiểu)

#### Giải pháp:
 - Luôn nói ngắn gọn mà rõ ràng, không ai biết được bạn đang nghĩ gì đâu

### 5. We are a team not nemesis
#### Vấn đề:

![alt text](/static/img/chuyen-giao-tiep/img5.jpg)

	- Tí phàn nàn Tuyết làm UI/UX tệ quá, Tuyết ôm hận trong lòng và thỉnh thoảng đổi design trước ngày deadline của Tí với lý do "đổi UI/UX cho vừa lòng Tí"
	- Tí trễ deadline nên bị stress, giận cá chém thớt, Tí quay qua hằn hộc với Tèo, đòi phải thay đổi API cho đúng với design.
	- Tèo lọ mọ sửa và lâu lâu chơi lại Tí bằng việc lâu lâu update API mà không báo trước cho crash App chơi.
	- Cả team xỉa xói lẫn nhau, soi mói và đâm chọt nhau ngay khi có thể *cho bỏ ghét*

#### Giải pháp:

 - Luôn nhớ rằng chúng ta là một team, không phải kẻ thù.
 - Thái độ thù địch không giúp ích được gì cả, nếu muốn đi nhanh hãy đi một mình, nếu muốn đi xa hãy đi cùng nhau, đừng níu kéo, dìm dập lẫn nhau.

### 6. Tam sao thất bản
#### Vấn đề:
![alt text](/static/img/chuyen-giao-tiep/img4.jpg)
#### Giải pháp:
 - Làm cho mọi thứ thật rõ ràng, document cụ thể và chi tiết.
 - Nếu phát sinh vấn đề cần làm rõ, không tạo ra điểm mù và chắn chắc mọi người có liên quan đều nắm được
 
### 7. I'm superman, everybody follow me!
#### Tình huống: 
 App có chức năng cơ bản là đăng nhập với username và password, Tuyết nghĩ nó quá đơn giản và không đủ bảo mật với một app cho nhà giàu, Tuyết quyết định thêm vào một nút *Đăng nhập bằng võng mạc*
#### Vấn đề:

	- API được thiết kế không phù hợp với design, chức năng mới không có yêu cầu rõ ràng, Tí bắt đầu gào thét.
	- Chức năng quá dị, chưa từng làm bao giờ, tốn thời gian để research, prototype làm ban đầu không đúng với yêu cầu, Tèo bắt đầu gào thét.
	- Design làm cho estimation sai bét nhè, break plan, team hỏi flow hoạt động nhưng Bờm không biết, Bờm bắt đầu gào thét.

- **Giải pháp 1*: OT thôi!! Cả team cắm mặt ngồi code chức năng mới, vừa code vừa chửi.
- **Giải pháp 2*: Hãy giao tiếp như một team. Trao đổi với các thành viên trong team để tìm ra giải pháp thuận lợi nhất cho tất cả các bên (design, API, mobile app thay đổi ít nhất có thể). Cả team quyết định thay đổi thành *xác nhận bằng OTP*.

### 8. ...
Có cả tỉ vấn đề khác mà giờ lười quá không viết nữa (￣▽￣)

# Kết
 
- Không có cái kết nào cả vì chuyện giao tiếp là chuyện không bao giờ dứt, các vấn đề ở trên cũng chưa hẳn là vấn đề, các giải pháp cũng chưa hẳn là giải pháp, mọi thứ còn tùy thuộc vào thời điểm, vào team, vào công việc, vân vân và mây mây...
- Mục đích cuối cùng của việc thành lập một team là để hoàn thành công việc một cách tốt nhất (thời gian, chất lượng, số lượng,...)
- Mọi thứ thật sự tốt nếu nó hợp lý và giải quyết được vấn đề và mục tiêu chung của cả team.
