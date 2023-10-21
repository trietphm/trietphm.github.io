+++
author = "Triet Pham"
date = "2019-03-03T11:02:55+07:00"
description = ""
tags = []
title = "Ứng dụng trên điện thoại Android"

+++

- Nhân dịp mới mua domain https://triet.dev + mới đổi sang dùng "quốc thoại" Xiaomi gần đây nên viết bài này.

- Câu hỏi mình thường gặp nhất từ bạn bè (IT) là "không sợ bị Xiaomi tracking/gửi dữ liệu về TQ này kia sao?". Dĩ nhiên là có, mình hơi dị ứng một chút với việc bị tracking và một số thể loại chạy app chạy background để thực hiện một vài thứ được cho là để "gia tăng trải nghiệm".

- Thế nên các app mình dùng hơi "khác người" một chút và đôi khi hơi phiền phức, nhưng bù lại mình thấy yên tâm hơn phần nào về privacy.

- Ngoài ra, viết bài này cũng vì một số bạn hỏi xem "có app gì hay ho không, giới thiệu với". Mình liệt kê một vài app _có lẽ_ ít người biết nhưng rất hữu dụng.

- **TL;DR**
  1. F-Droid
  2. Blokada
  3. Greenify
  4. SlimSocial
  5. 1Password
  6. Nova Launcher
  7. GadgetBridge
  8. Authy
  9. Firefox + Addon
  10. Google Podcasts
  11. Genious Scan
  12. Skytube
  13. Manga Reader

# 1. F-droid

Thứ đầu tiên mình cài là [F-droid](https://f-droid.org/en/), là một dạng App Store khác như Google Play Store, nhưng đây là một community-maintained software repository của Android.
Nói cách khác đây là một App Store cho mã nguồn mở và nhiều app khác bị cấm trên Google Play, nguyên nhân cấm thì có nhiều nhưng đa phần ở đây không phải là vì virus/mã độc mà là do ảnh hưởng đến việc kinh doanh của công ty khác (sẽ đề cập đến các app phía sau)
Nhiều ứng dụng phía sau cũng được tải từ F-Droid

Các bạn có thể vào đây để download file APK về cài đặt
https://f-droid.org/en/

Hầu hết các thứ mình cần đều sẽ search ở đây trước, nếu không có mới bắt đầu tìm ở Google Play
![alt text](/img/android-app/fdroid.png)

# 2. Blokada

- Nếu bạn đã dùng AdBlock, uBlock Origin,... trên web browser sẽ không thể bỏ qua app này. App sẽ giúp bạn chặn các thể loại **quảng cáo** từ A-Z: từ **quảng cáo trên web** (Chrome, Firefox,...), đến các **quảng cáo in-app** (tức các quảng cáo chạy trong các app khác), đến luôn các thể loại **tracking, thu thập dữ liệu,...** từ chính Google, Xiaomi,... tóm lại là toàn bộ thứ gì đi qua Network Connection trên điện thoại của bạn đều bị kiểm soát.

- Ngoài ra Blokada còn cho phép set DNS và custom filter (tự tạo filter list), mình dùng custom filter để chặn Xiaomi tracking.

- Hiển nhiên bạn sẽ không tìm thấy Blokada trên Google Play vì chặn quảng cáo = chặn nồi cơm của Google. Tải file APK ở trang chủ
https://blokada.org/

hoặc vào **F-droid** và search Blokada để cài đặt
https://f-droid.org/en/packages/org.blokada.alarm/
![](/static/img/android-app/blokada.png)

- Cách Blokada hoạt động: nói ngắn gọn là Blockada sẽ tạo một VPN và mọi thứ đi qua VPN sẽ bị filter (quảng cáo) dựa trên filter list. Và app được optimize khá tốt, không hề tốn pin dù chạy 24/24.
- "Thế lỡ bị chính thằng Blokada nó track thì sao?" Blokada không track và có open source trên Github https://github.com/blokadaorg/blokada, bạn có thể vào check và ném đá nếu phát hiện ra.

**Bất tiện**:
- Nếu bạn chơi game và có mục dạng "Xem video quảng cáo để nhận quà" thì chắc chắn bạn sẽ không load được video nào (chặn hết rồi, obviously), nếu cần có thể tắt Blokada và mở lại game.
- Thỉnh thoảng một số app dùng các service tracking hoặc send SMS (verify tài khoản lúc đăng ký) sẽ không gửi request đi được và báo lỗi, tắt tạm Blokada lên send SMS rồi mở lại là xong.

**Bonus:** Filter list chặn hết các request đến Xiaomi
```
https://gist.githubusercontent.com/trietphm/6b73a101c64c50c70ac8808bbc75730b/raw/be8d3121f3cb251b63e964d6c3ede8eb10609529/blokada-xiaomi-blocklist
```

# 3. Greenify

Chặn các ứng dụng chạy nền (sync dữ liệu, push notification, tracking,...) bằng cách hibernate (ngủ đông) app đó, không còn bị tốn Pin (rất nhiều) vào mấy thứ này, rất nhiều app dù bạn có tắt, force stop các kiểu nó vẫn chạy và ăn pin như thường.
Các app sau khi bị hibernated sẽ chỉ "thức dậy" khi bạn chủ động mở nó lên.
Có thể tải Greenify trên Google Play
![alt text](/static/img/android-app/greenify.png)

https://play.google.com/store/apps/details?id=com.oasisfeng.greenify&hl=en

Có bản Pro (donate version - tốn tiền) có nhiều chức năng hơn và nếu máy bạn đã root thì càng mạnh mẽ hơn.

Quick guide:
- Mở app lên, chọn **(+)** và chọn các app cần hibernate
- Mình thường set "Sleep and Hibernate" của Greenify làm _Assist & voice input_ trong **Default apps**, để chỉ cần ấn giữ nút Home (trên màn hình) để tắt màn hình (lock máy) và hibernate cùng lúc.
![](/static/img/android-app/default-apps.png)
![](/static/img/android-app/default-apps-2.png)

# 4. SlimSocial

Ứng dụng thay thế cho Facebook/Facebook Lite. Vì:
- Chặn quảng cáo
- Gọn nhẹ (~100KB)
- Open Source
- No tracking
- No background stuff
- Customizable (Dark mode, font size,...)

Bản chất app này cũng chỉ wrap cái site touch.facebook.com lại và thêm một vài tùy biến nhỏ

**Bất tiện:**
Không "xịn" và nhiều chức năng như app Facebook, hiển nhiên, nhưng đối với mình là quá đủ, chủ yếu để xem/like/comment là đủ, những thứ khác không cần thiết.

Có thể tải SlimSocial trên Google Play hoặc F-Droid. Open source: https://github.com/rignaneseleo/SlimSocial-for-Facebook
Google Play: https://play.google.com/store/apps/details?id=it.rignanese.leo.slimfacebook&hl=en
F-Droid: https://f-droid.org/en/packages/it.rignanese.leo.slimfacebook/
![](/static/img/android-app/slimsocial.jpg)

# 5. 1Password (Có phí)

Gần như là must-have app vì hầu như toàn bộ password của mình là random generated và được lưu trong 1Password. Mỗi lần cần đăng nhập mình sẽ dùng 1Password để auto-fill.
Ngoài ra, còn một vấn đề về security nữa là nếu bạn dùng một app keyboard bên thứ 3 (VD Laban key, GoTiengViet, Google Keyboard,...) thì các app này có thể đọc và _vô tình_ lưu password của bạn.

Vậy copy Password từ 1Password ra rồi paste vào là ổn? Thật sự thì chưa, vì bất cứ app nào cũng được quyền đọc Clipboard và biết được password của bạn.
Tốt nhất nên dùng chức năng auto-fill của 1Password hoặc chuyển sang **1Password Keyboard** mỗi lần nhập password.
Bạn có thể xem thêm về security hole này ở đây:
https://github.com/grepx/android-clipboard-security

![](https://1password.com/img/downloads/screenshot-android-mobile@2x.f1dc98bb5f825a473cd6f24c2a653dfe.png)

Google Play: https://play.google.com/store/apps/details?id=com.agilebits.onepassword&hl=en

# 6. Nova Launcher

Thử rất nhiều Launcher nhưng đây là cái nhanh, ổn định, customizable nhất mình từng dùng. Ngoài ra các chức năng của bản Pro cũng đáng để bỏ tiền ra mua.
Tải trên Google Play nha.
https://play.google.com/store/apps/details?id=com.teslacoilsw.launcher&hl=en

# 7. Gadgetbridge

Mình có một cái vòng đeo tay Miband nhưng lại không muốn đưa dữ liệu cho Xiaomi, nhưng muốn xem dữ liệu activity/sleeping tracking thì phải sync với app Xiaomi Fit :(
Và Gadgetbridge là giải pháp, một open-source Android app để thay thế cho app Xiaomi Fit. Ngoài ra, Gadgetbridge còn hổ trợ một số thiết bị đeo tay khác như Amazfit, Pebble,...

Mặc dù UI/UX hơi bị chuối nhưng đối với mình cũng chấp nhận được.

![](https://i.imgur.com/OZ43x0T.png)

F-Droid: https://f-droid.org/en/packages/nodomain.freeyourgadget.gadgetbridge/

# 8. Authy

Giống như **Google Authenticator** nhưng có thể backup được, lỡ mất điện thoại còn có thể recovery lại.
https://play.google.com/store/apps/details?id=com.authy.authy&hl=en

![](https://lh3.googleusercontent.com/rZrPOfWeeot2-KZY5sIkjn1b9B3trGhp0y_oHJprXjUTNMI5y5qFaLSSHA9TPv3uvuyf=w2880-h1522-rw)

# 9. Firefox
Mình rất công nhận Chrome load nhanh hơn Firefox rất nhiều, nhưng vẫn dùng để ủng hộ Firefox. Ngoài ra còn vì Firefox Android có thể cài Add-on được.

- **Tap Translate** (để quick translate, chỉ cần select từ muốn tra sẽ có cái button hiện ra để dịch từ đó)
- **User-Agent Switcher**: để chuyển web-agent sang thành... Chrome. Nhiều website của Google (Google search, Gmail,...) load trên Firefox cực kỳ xấu (Google cố tình làm vậy), cả Facebook cũng giảm độ phân giải ảnh xuống, nên phải chỉnh agent thành chrome để giả mạo.

# 10. Google Podcasts
Nếu bạn thường nghe Podcasts thì đây là một app không thể bỏ qua.
Hàng chính chủ Google, đơn giản, gọn nhẹ, dễ dùng.

![](https://lh3.googleusercontent.com/wkD5vF9dVBn3Xvd_Pq13ZdBLQ_JFSwnXkeiTJxqmrx6QLZZObQQRxVSFFRkZ4aekz3o=)

Google Play: https://play.google.com/store/apps/details?id=com.google.android.apps.podcasts&hl=en

# 11. Genious Scan

Từ lúc CamScanner tính phí mình chuyển qua cái này để scan/chụp tài liệu bằng camera điện thoại, nhanh gọn lẹ.
https://play.google.com/store/apps/details?id=com.thegrizzlylabs.geniusscan.free&hl=en

# 12. SkyTube

Dùng để xem và... nghe YouTube. Vấn đề lớn nhất của app YouTube là bạn không thể tắt màn hình để nghe nhạc được (nhạc tắt theo luôn), trừ phi bạn có đăng ký YouTube Premium (đáng tiếc là cũng không available ở Việt Nam)
Bạn có thể xem video trên SkyTube và tắt màn hình thoải mái.
Do đạp nồi cơm của YouTube nên bạn chỉ có thể tải SkyTube trên F-Droid

https://f-droid.org/en/packages/free.rm.skytube.oss/
![](/static/img/android-app/skytube.jpg)

# 13. Manga Reader
The best in town! Là app đọc truyện tranh với rất nhiều nguồn khác nhau, từ tây đến ta đến tàu, ngoại trừ UI hơi chuối một chút thì những thứ còn lại cực kỳ ổn:
https://play.google.com/store/apps/details?id=com.comikin.reader2&hl=en
