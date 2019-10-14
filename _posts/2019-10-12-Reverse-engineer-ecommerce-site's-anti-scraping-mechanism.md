---
layout: post
title: Giải mã obfuscated public API của một trang thương mại điện tử tại VN
tags: [xoppi,reverse engineering]
---

# Intro
Nếu ai đã từng code tool cho các sàn TMĐT, hẳn đã nhận ra 1 trong số chúng có public API, lại còn trả về dữ liệu đã được format JSON và có thể dễ dàng bắt được bằng DevTools của Chrome/Firefox mà không gặp khó khăn gì cả.

Nhưng...

Một khi bạn đã viết được 1 cái script tàm tạm tận dụng điều vừa nêu, bạn sẽ thấy dữ liệu API này trả về browser khác hẳn dữ liệu nó trả về khi bạn request trực tiếp đến API. 

Chúng ta tạm gọi sàn TMĐT trong bài là sàn X
# More on the problem
Hãy xem 4 ảnh sau để thấy vấn đề : 

Ảnh 1 là request được thực hiện khi truy cập vào website của sàn X

Ảnh 2 là request trực tiếp đến public API của X
![request headers số 1](http://thitgaluoc.com/public/img/08:59:19.2019-10-14.png)
![JSON response số 1](http://thitgaluoc.com/public/img/08:59:58.2019-10-14.png)
![request headers số 2](http://thitgaluoc.com/public/img/09:02:26.2019-10-14.png)
![json response số 2](http://thitgaluoc.com/public/img/09:03:09.2019-10-14.png)

Như trong hình, khi thực hiện request trực tiếp đến API mà không kèm header If-None-Match-, dữ liệu trả về đã được obfuscated. 

Hãy để ý request header trong hình 1 là **If-None-Match-** (có dấu gạch ngang) chứ không phải **If-None-Match** [như cơ chế cache bằng ETag header](https://en.wikipedia.org/wiki/HTTP_ETag).

Nếu nhìn thoáng qua, 2 cặp dữ liệu này trông giống như (1) đã được cached ở browser và (2) thì chưa. Nhưng cache kiểu gì mà *price* lại *null* thế kia, shop_location lại còn thiếu chữ nữa. Rõ ràng là obfuscated data chứ không phải cache rồi.

# Lục lọi trong code
Đầu tiên phải nói rằng mình không có tí kiến thức gì mấy về js, cũng như debug js code. File js này lại lên đến hơn 40k dòng, dùng các công cụ deobfs trên mạng như jsnice nó nuốt k trôi.

Nhưng mà chắc cũng giống xem trên youtube thôi với mấy tut cờ rắc trên mạng thôi. Thử vận may xem sao :P
F12 mở DevTools ra nào, set breakpoint với tất cả các url chứa có dấu hiệu của endpoint đang cần tìm (search_items), sau đó F5 browser thôi
![enter image description here](http://thitgaluoc.com/public/img/xoppi_1.png)
Khi đã dừng ở breakpoint, ta unminify code xem cho dễ
![enter image description here](http://thitgaluoc.com/public/img/xoppi_2.png)
Sau khi đã unminified, code trông dễ thở hơn nhiều, ta thấy có cái biến **t** có kèm headers trong đó (dòng 27524), có vẻ hay, set breakpoint luôn ở mấy dòng có **t**.
![enter image description here](http://thitgaluoc.com/public/img/xoppi_4.png)

Ngó qua một hồi thì khi chạy đến dòng 27524 kể trên, header đã có kèm giá trị If-None-Match- bên trong. Vì ta đang tìm xem giá trị này được tính như thế nào nên phải để ý sang bảng Call Stack bên phải. 

Click click một hồi ta thấy hàm không tên anonymous có vẻ khá khả nghi. Hàm này chứa biến **f** là header bổ sung cho request. Set tiếp breakpoint tại đây và những chỗ chứa **f** xem sao.
![enter image description here](http://thitgaluoc.com/public/img/xoppi_7.png)
Ở đây mình để ý thấy ban đầu f không có if-none-match. Vậy mình set breakpoint ngay trước những chỗ gọi đến các hàm để xem khi nào thì **f** có thêm if-none-match thêm vào (là các mũi tên màu xanh da trời)

Thực hiện lại các thao tác trên website (chuyển trang để gọi lại API), ta thấy từ dòng 5645 biến **f** không có header if-none-match thì sang dòng 5646 đã có biến **f**, do đó việc tính toán if-none-match chắc chắn thực hiện ở line 5645.
![enter image description here](http://thitgaluoc.com/public/img/xoppi_8.png)
Nhìn qua các hàm thì có replace không đáng quan tâm lắm, hàm anon hình như cũng k có, có **m()** là khả nghi nhất. Di chuột vào **m()** và đợi 1s, thấy ngay nó được định nghĩa ở line 5708.
![enter image description here](http://thitgaluoc.com/public/img/xoppi_10.png)
Định nghĩa hàm này khá lằng nhằng, nhưng thôi k sao, breakpoint tất tần tật xem nó là cái gì. Nhìn qua thấy md5 có vẻ khả quan r đây.
![enter image description here](http://thitgaluoc.com/public/img/xoppi_11.png)
Set breakpoint tá lả xong thì F8 vài lần cho code nó chạy, đến dòng 5723 ta để ý thấy **c.a** có giá trị **55b03** rồi nối với gạch ngang, trông cũng na ná header mình cần tìm đó (if-none-match-: 55b03-fbb1f05b7982c5314d66ec68da70e94f).

Đoạn sau thì ta để ý thấy **u** có liên quan đến **md5**, xong ở dưới lại *c.a + "-" + u("" + c.a + u(t) + c.a)*. Xâu chuỗi lại có thể thấy có gì đó liên quan đến md5 hash.

Vì ngu js nên đọc mãi k hiểu lắm, mình quyết định copy cả cụm ném vào [jsnice.org](http://jsnice.org/). Kết quả được như sau : 
![enter image description here](http://thitgaluoc.com/public/img/xoppi_13.png)
Bingo, có vẻ như công thức là if-none-match = 55b03-md5_hash(55b03+t+55b03)

t là gì thì lại F8 vài lần nữa cho qua line 5723 nêu trên, thấy ngay t chính là query string của endpoint đang truy cập vào hehe.
![enter image description here](https://raw.githubusercontent.com/thit-ga-luoc/thitgaluoc.github.io/master/public/img/xoppi_18.png)

Xong tut, các bạn có thể kiểm tra lại bằng cách lấy query string tính theo công thức trên, đảm bảo ăn. Còn đây là sàn nào thì xem kĩ ảnh biết ngay thôi!!!
