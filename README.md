# Bài 3: Cấu hình tường lửa UFW và chuẩn đoán cổng mạng

## 1. Mục tiêu

Cấu hình UFW trên Ubuntu Server để:

- Chặn kết nối đi vào mặc định.
- Cho phép kết nối đi ra mặc định.
- Cho phép kết nối SSH qua port 22/tcp.
- Cho phép ứng dụng Web qua port 8080/tcp.
- Kiểm tra trạng thái Firewall và các port đang lắng nghe.

## 2. Cấu hình UFW

### Thiết lập chính sách mặc định

```bash
ufw default deny incoming
ufw default allow outgoing
```

### Mở port SSH

```bash
ufw allow 22/tcp
```

### Mở port Web

```bash
ufw allow 8080/tcp
```

### Kích hoạt Firewall

```bash
ufw enable
```

## 3. Kiểm tra trạng thái UFW

Lệnh:

```bash
ufw status verbose
```

Kết quả thực tế:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
8080/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
8080/tcp (v6)              ALLOW IN    Anywhere (v6)
```

## 4. Kiểm tra các port đang lắng nghe

Lệnh:

```bash
ss -tlnp
```

Kết quả thực tế:

```text
State    Recv-Q   Send-Q       Local Address:Port       Peer Address:Port   Process
LISTEN   0        4096            127.0.0.54:53              0.0.0.0:*       users:(("systemd-resolve",pid=746,fd=17))
LISTEN   0        4096         127.0.0.53%lo:53              0.0.0.0:*       users:(("systemd-resolve",pid=746,fd=15))
LISTEN   0        4096                     *:22                    *:*       users:(("sshd",pid=1159,fd=3),("systemd",pid=1,fd=69))
```

Port 22 đang được `sshd` lắng nghe.

Port 8080 chưa xuất hiện trong `ss -tlnp` vì hiện tại chưa có ứng dụng Web chạy và lắng nghe trên port này. Tuy nhiên, port 8080 đã được UFW cho phép kết nối vào.

## 5. Kết luận

Đã hoàn thành cấu hình Firewall theo yêu cầu:
![alt text](1.png)

- UFW: `active`
- Incoming mặc định: `deny`
- Outgoing mặc định: `allow`
- SSH `22/tcp`: `ALLOW IN`
- Web `8080/tcp`: `ALLOW IN`
- Đã kiểm tra các port đang lắng nghe bằng `ss -tlnp`.
  ![alt text](2.png)
