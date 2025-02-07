# Cấu hình gửi cảnh báo qua tele
## Tạo bot tele
**Mở Telegram và tìm kiếm BotFather**

![image](https://github.com/user-attachments/assets/4aec214c-56d2-484c-bfb7-65b49dde524b)

Gửi lệnh ```/newbot``` và làm theo hướng dẫn

![image](https://github.com/user-attachments/assets/9cd46d95-7887-4d7c-ba65-b4c7ec816f01)

Sau khi tạo, BotFather sẽ gửi cho bạn API Token, lưu lại token để dùng cho các bước sau
## Lấy Chat ID
Mở chat với bot vừa tạo (bấm nút Start).
Gửi một tin nhắn bất kỳ cho bot.
Truy cập URL sau (thay <TOKEN> bằng token của bạn):
```
https://api.telegram.org/bot<TOKEN>/getUpdates
```
Trong phản hồi JSON, tìm "chat": { "id": ... } để lấy Chat ID của bạn (ví dụ: 987654321).
## Tạo Script Gửi Thông Báo cho Telegram
Trên Zabbix Server, mở terminal và tạo file script trong thư mục ```/usr/lib/zabbix/alertscripts/``` :
```
sudo vi /usr/lib/zabbix/alertscripts/telegram.sh
```
Nội dung file script nên như sau (thay thế TOKEN và CHAT_ID bằng giá trị của bạn):
```
#!/bin/bash
# Telegram Bot API Token và Chat ID của bạn
TOKEN="123456789:ABCDEFghIJKLMNOpqRSTUvWXyz"
CHAT_ID="987654321"

# Nội dung tin nhắn truyền từ Zabbix
MESSAGE="$1"

# URL gửi tin nhắn qua API Telegram
URL="https://api.telegram.org/bot$TOKEN/sendMessage"

# Sử dụng curl để gửi POST request
curl -s -X POST $URL -d chat_id=$CHAT_ID -d text="$MESSAGE"
```
Cấp quyền thực thi cho file:
```
chmod +x /usr/lib/zabbix/alertscripts/telegram.sh
```
## Cấu Hình Media Type trong Zabbix
![image](https://github.com/user-attachments/assets/01d734c7-9b7d-4b02-a132-39f2aeb182cd)
Nhấn Create media type và điền thông tin:
Name: Telegram Alert
Type: Chọn Script
Script name: Nhập telegram.sh

## Liên Kết Media với Người Dùng
![image](https://github.com/user-attachments/assets/81a2dbe8-067b-4bbe-8bb1-93c08aa487b8)

Chọn user mà bạn muốn nhận cảnh báo (ví dụ: Admin).
Vào tab Media và nhấn Add.
Cấu hình:
Type: Chọn Telegram Alert (media type vừa tạo).
Send to:  nhập Chat ID.
When active: (Thiết lập khoảng thời gian nhận thông báo, mặc định là 00:00-24:00).
Use if severity: Chọn mức độ cảnh báo mà user này sẽ nhận (ví dụ: từ Warning trở lên).

## Cấu Hình Action Gửi Thông Báo
![image](https://github.com/user-attachments/assets/3cffe3e8-3780-44fe-bf00-df8c42667137)

Chọn hoặc tạo Action mới (ví dụ: “Send Telegram alerts”).
Trong tab Operations, thêm một Operation:
Send to Users: Chọn user đã cấu hình Media Telegram.
Send only to: Chọn Telegram Alert.
Thiết lập các điều kiện (Conditions) sao cho phù hợp với các trigger mà bạn muốn gửi thông báo.


![image](https://github.com/user-attachments/assets/8d812df9-fce3-43b4-a245-81a9b648f2ff)

## Kiểm Tra Hoạt Động
kích hoạt một trigger để kiểm tra xem Zabbix có gửi thông báo tự động qua Telegram hay không.
