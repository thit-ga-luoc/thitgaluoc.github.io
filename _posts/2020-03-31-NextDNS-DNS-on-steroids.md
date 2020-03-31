---
layout: post
title: NextDNS - DNS on steroids
tags: [nextdns]
---

# Intro
Bạn đã biết về Google DNS 8.8.8.8, Cloudflare DNS 1.1.1.1 hay Quad9 9.9.9.9? Vậy thì xin giới thiệu một trải nghiệm mới : NextDNS. Với NextDNS, bạn có thể làm những việc sau : 
 - Mã hóa các gói tin DNS query qua DNS-over-HTTPS (DoH) hay DNS-over-TLS (DoT)
 - Block quảng cáo ở network level, có nghĩa điện thoại, TV, PC, tablet, tủ lạnh thông minh, smart sextoy đều được hưởng lợi mà không phải mất công cài đặt riêng cho từng thiết bị. Và tôi nói : network level, không chỉ block quảng cáo trên trình duyệt.
 - DNS Analytics, cho bạn biết câu trả lời cho câu hỏi : Một tháng tôi vào pornhub bao nhiêu lần?
 - Native Windows/Linux/MacOS/OpenWRT/Android/iOS/pfSense,... client
 - DNS rewrites, cho phép cấu hình dantri.com.vn trỏ về IP của xvideos chẳng hạn.
 - Cryptojacking/Safe Browsing blocking built-in
 
 TẤT CẢ NHỮNG ĐIỀU TRÊN ĐỀU ĐƯỢC THỰC HIỆN TRÊN "ĐIỆN TOÁN ĐÁM MÂY" (một khái niệm được quảng bá bởi BKAV)

# Setup
Đầu tiên, bạn cần truy cập http://nextdns.io/ để đăng ký một tài khoản, tài khoản free cho phép tối đa 300.000 query 1 tháng, yên tâm là bạn chẳng dùng hết con số ấy đâu.
Sau đó đăng nhập, bạn sẽ thấy như thế này : 
![NextDNS Dashboard](http://thitgaluoc.com/public/img/nextdns-dashboard.png)
## Trên PC
Cách 1 : Dùng Firefox, setup DNS over HTTPS theo hướng dẫn này : [Firefox DNS-over-HTTPS](https://support.mozilla.org/en-US/kb/firefox-dns-over-https#w_manually-enabling-and-disabling-dns-over-https). Bạn cần dán vào firefox địa chỉ (1) như đánh dấu ở đầu bài.

Cách 2 : Cài đặt client của NextDNS theo link ngay ở dashboard như hình trên, sau đó điền ID lấy từ NextDNS Dashboard là được.

## Trên Router
Dùng cách này, bạn sẽ cấu hình được NextDNS cho tất cả các thiết bị kết nối đến router. Bạn nên có một con router cài đặt sẵn OpenWRT hoặc DD-WRT hoặc Entware hoặc Padavan hoặc Gargoyle hoặc gì gì đó miễn nó là linux.
Thường router sẽ dùng dnsmasq, bạn cần SSH đến router, sau đó cấu hình file `dnsmasq.conf` có mấy dòng sau : 
```
server=45.90.28.10
server=45.90.30.10
```
file `dnsmasq.conf` ở đâu thì phụ thuộc vào router của bạn, tôi dùng Padavan nên nó sẽ nằm ở `/etc/storage/dnsmasq`
cấu hình xong thì reboot thôi!


# Cấu hình chặn quảng cáo
**Bước 1** : Đăng nhập NextDNS, chọn tab Privacy

**Bước 2** : Bấm Add Blocklist và chọn blocklist như đang cấu hình uBlock/Adblock thôi. Việt mình có cả list hostVN và Fanboy.
Sau đó thì Enjoy!!!!


# Một số tính năng cool ngầu khác : 
## DNS analytics
![NextDNS analytics](http://thitgaluoc.com/public/img/nextdns-analytics.png)
## DNS rewrites
![NextDNS RWW](http://thitgaluoc.com/public/img/nextdns-rewrites.png)

# Cool, huh?
