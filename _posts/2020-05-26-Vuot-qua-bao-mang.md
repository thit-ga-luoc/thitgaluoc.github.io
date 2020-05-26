---
layout: post
title: Vượt qua bão mạng và tăng tốc đường truyền với v2ray
tags: [v2ray]
---

# Intro
Nếu anh em đã biết lưu lượng internet của VN được truyền qua các tuyến cáp AAG, APG thì hẳn anh em cũng đã biết chúng mong manh đến nhường nào.

Ở thời kỳ nhà nhà lên net như hiện nay, cuộc sống với băng thông dưới 5Mbps có thể được so sánh với thời kỳ đồ đá.

Hôm nay tôi xin giới thiệu với anh em một công nghệ proxy cũng mơi mới có thể giúp bạn xoa dịu nỗi đau mạng chậm. Công nghệ này mang tên V2Ray, tác giả là những người anh em Trung Hoa anh hùng. Ở đất nước 1,3 tỷ dân, internet được kiểm soát chặt chẽ, traffic ra và vào hệ thống được lọc qua một bộ tường lửa còn sạch hơn cả màng R.O. anh em ta hay lọc nước.

Để anh em coder, hắc cơ,... có thể nghiên cứu khoa học từ các trang Po... à nhầm **Git**hub hay Google, rất nhiều công nghệ proxy và VPN đã được phát triển.

Bên cạnh các công nghệ VPN phổ biến toàn thế giới đang dùng như OpenVPN, SoftEther, hay mới đây nhất là WireGuard, hầu hết đều đã bị vô hiệu hóa, tôi xin điểm qua một vài cái tên nội địa : Shadowsocks, ShadowsocksR, Brook, Trojan, V2Ray,.... . Shadowsocks và Tor vốn được sử dụng rộng rãi, đã bị rơi rụng lả tả sau khi bức tường lửa của chủ tịch Tệp được level up lên một cấp độ mới, [sử dụng cả heuristic analysis để quét các traffic bất thường (mà nó tạo ra)](https://conferences.sigcomm.org/imc/2015/papers/p445.pdf). 

Complex problems require complex solutions. Vỏ quýt dày có móng tay nhọn, V2Ray được sinh ra để kế thừa và phát triển các công nghệ trước đó. Nó sử dụng giao thức UDP, trao đổi thông tin với server dưới dạng các gói tin đã được mã hóa sao cho trông như traffic bình thường. Về các feature của nó, tốt nhất anh em nên xem trên trang chủ, tôi chỉ nhớ láng máng vài từ như WebSocket, mKCP, UDP, BBR.


## Liên quan?

Như đã nói trên, vì traffic được gói trong những gói tin như traffic truy cập web bình thường qua https, websocket, ... , ta có thể giao tiếp với server proxy qua một hệ thống CDN như Cloudflare. Các gói tin sẽ đi như này : PC ==> Cloudflare CDN gần nơi bạn ở ==> Cloudflare internal network ==> Cloudflare exit node ==> server và ngược lại.

Do Cloudflare đã có DC ở Việt Nam và các nước lân cận, chiều từ PC ===> Cloudflare sẽ rất nhanh, nhanh như bạn vào trang chủ của VNPT vậy.

Do hệ thống của Cloudflare cũng trải rộng trên toàn thế giới, nên traffic đi qua hệ thống của nó và đến server của bạn có lẽ còn nhanh hơn ở trên.

**Kết luận :** dùng VPN/proxy kiểu này sẽ nhanh hơn truy cập trực tiếp (đi qua load balancer của ISP, nơi mà traffic của khách lẻ sẽ bị bóp không thương tiếc)

## Nguồn thông tin thêm

[Trang chủ project V2Ray tiếng Anh](https://www.v2ray.com/en/)

[Trang chủ V2Ray tiếng Việt](https://www.v2ray.com/vi/welcome/start.html)

[Hướng dẫn chi tiết thiết lập V2Ray hoạt động với Cloudflare](https://blog.sprov.xyz/2019/03/11/cdn-v2ray-safe-proxy/)

[QV2Ray, cross-platform client cho V2Ray](https://github.com/Qv2ray/Qv2ray)

[V2RayNG, client cho Android](https://play.google.com/store/apps/details?id=com.v2ray.ang&hl=en_US)

[Các client cho hệ điều hành khác](https://www.v2ray.com/en/awesome/tools.html)

[233v2, script cài đặt nhanh với 1 click cho server](https://233v2.com/post/1/)

[Great Firewall of China](https://www.comparitech.com/privacy-security-tools/blockedinchina/)

[Bypass GFW China 2019](https://medium.com/@phoebecross/bypass-gfw-china-2019-9d293b322e20)

[The story of China’s Great Firewall, the world’s most sophisticated censorship system](https://www.abacusnews.com/who-what/story-chinas-great-firewall-worlds-most-sophisticated-censorship-regime/article/3030478)


**Kết quả**

![Bên trái không có V2Ray, bên phải có V2Ray](http://i.imgur.com/pKfXdlc.png)

Bên trái không có V2Ray, bên phải có V2Ray

Tất nhiên là kết quả không có gì khác nhau lắm, vì mạng dùng gói doanh nghiệp nên ít bị bóp, mạng ở nhà chắc sẽ bị bóp mạnh hơn :D

## Chuẩn bị

- 1 server/VPS bất kỳ, khuyên dùng gói 5 Trump của DigitalOcean là đủ

- 1 tài khoản Cloudflare

- 1 domain name đã trỏ về Cloudflare


## Thực hiện

Tóm tắt vì em chưa có thời gian :)))

Bước 1 : Trỏ domain về Cloudflare, tạo A record trỏ về IP của VPS, tạm tắt Cloudflare proxy status màu cam đi đã

![](http://i.imgur.com/aiugilU.png)



Bước 2 : ssh vào VPS, cài `curl` nếu chưa có, sau đó gõ 

```bash
sudo bash <(curl -s -L https://git.io/v2ray.sh)
```

Bước 3 : Cài đặt theo script và [hướng dẫn ở đây](https://233v2.com/post/1/). Vì toàn tiếng Tàu nên anh em cần Google Translate nhoa :D Tôi hay chọn option như thế này : 

- Bước 1 chọn số [1] để cài v2ray

- Bước 2 chọn số [4] để dùng WebSocket + TLS.

- Bước 3 chọn port, gõ vào port 26626

- Bước 4 gõ tên domain (như ví dụ là so.indoheadlines.club)

- Bước 5 chọn [Y] để xác nhận IP của VPS là đúng

- Bước 6 chọn [Y] để cài Caddy web server và tự động cấu hình certificate cho nó

- Bước 7 : Chọn [N] với tất cả các câu hỏi sau, chủ yếu về các hình thức obfuscate traffic để khó bị phát hiện hơn.

- Bước 8 : Khi đã ra màn hình thế này thì ấn Enter cho nó chạy thôi

  ![](http://i.imgur.com/sZfZcdf.png)



- Bước 9 : Script chạy xong thì gõ `sudo v2ray qr` để lấy link có dạng `https://233boy.github.io/tools/qr.html#vmess://xxxx`. Dán link này vào trình duyệt ta được mã QR.

- Bước 10 : Vào cloudflare mở lại Proxy status màu cam cho domain/subdomain

- Bước 11 : Lấy điện thoại quét mã QR này để add VPN vào mà dùng thôi. Với PC thì có thể gõ `sudo v2ray url` để lấy url dạng `vmess://xxxxx` sau đó dán vào client để add VPN. 

  ![](http://i.imgur.com/r1aY9tu.png)


** Cloudflare analytics : **

![](http://i.imgur.com/i9jjLmJ.png)

![](http://i.imgur.com/LVK1NmD.png)


