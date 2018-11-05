---
layout: post
title: OpenWRT WAN watchdog - kiểm tra kết nối mạng và đổi nguồn phát repeater
tags: [openwrt]
---

# Intro
Mình có 2 router, một con TP-Link WR740N, một con Xiaomi Nano, tất cả đều đã cài OpenWRT.

# Thân bài
Mình quyết định chèn thêm một đoạn mã cho phép router của mình (đang ở chế độ simple repeater, WISP, whatever) ping liên tục đến server công cộng với điều kiện không bị nhận dạng nhầm là icmp flood - ping flood), nếu server không trả lời thì đổi nguồn phát. Do đã biết 2 SSID có tín hiệu mạnh nhất, tạm gọi là A và B, đồng thời thiết lập của router được chứa trong /etc/config/wireless nên mình có thể làm 2 file config này và viết script thay thế 2 file này cho nhau khi ping fail. 
Trình tự thực hiện như sau :
1. ssh đến router, đăng nhập như người lớn.
2. Hiện tại file `/etc/config/wireless` đang trỏ sang nguồn phát A, ta nhân bản nó sang file khác bằng cách gõ `cp /etc/config/wireless /etc/config/wireless-2`, sau đó `vi /etc/config/wireless-2` để mô-đi-phê file vừa cóp trỏ sang nguồn phát B
3. tạo file shell script ở `/root/watchdog.sh` với nội dung như sau : 

```
#!/bin/sh

tries=0
while [[ $tries -lt 5 ]]
do
	if /bin/ping -c 1 8.8.8.8 >/dev/null
	then
		exit 0
	fi
	tries=$((tries+1))
done

mv /etc/config/wireless /etc/config/wireless-temp
mv /etc/config/wireless-2 /etc/config/wireless
mv /etc/config/wireless-temp /etc/config/wireless-2

/etc/init.d/network restart
```

4. Cấp quyền cho nó chạy : `chmod +x /root/watchdog.sh`
5. Thiết lập crontab cho watchdog nó chạy khi khởi động router : 
 - Cách 1 là đăng nhập vào luci → System → Scheduled Tasks, thêm đoạn `* * * * * /root/wan-watchdog.sh` và Submit
 - Cách 2 là `vi /etc/crontabs/root` sau đó làm như trên.
 6. (Optional)
 Với OpenWRT mặc định crontab không được bật, để bật nó thì bạn phải gõ thêm 2 lệnh sau : 
 ```
 /etc/init.d/cron start
 /etc/init.d/cron enable
 ```
 
 Xong! Giờ tôi đã có thể cho trẻ con ăn sáng một cách nhẹ nhàng hơn! :'(
