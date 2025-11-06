# ♟️ CARO GAME – JAVA SOCKET (CLIENT – SERVER)

## 🎯 Giới thiệu
**Caro Game (Gomoku)** là một ứng dụng trò chơi cờ caro 2 người chơi trực tuyến, được phát triển bằng **Java Swing** với kiến trúc **Client–Server** sử dụng **Socket TCP/IP**.  
Dự án được thực hiện trong môn **Lập Trình Mạng (LTM)**, mục tiêu là:
- Hiểu rõ cơ chế giao tiếp Socket trong Java.
- Thiết kế giao diện GUI với NetBeans.
- Quản lý người dùng, phòng chơi, và xử lý đồng bộ giữa các client qua server trung tâm.

---

## 🧱 Kiến trúc tổng thể

### ⚙️ Mô hình hoạt động
+-----------------+ +-----------------+ +-----------------+
| Client 1 (A) | <----> | SERVER | <----> | Client 2 (B) |
| (Người chơi A) | | (Socket + DB) | | (Người chơi B) |
+-----------------+ +-----------------+ +-----------------+

less
Sao chép mã

- **Server:** Quản lý kết nối socket, lưu thông tin user, phòng chơi, kết quả.
- **Client:** Hiển thị giao diện, nhận dữ liệu từ server, gửi nước đi và thông tin người chơi.

---

## 📂 Cấu trúc thư mục dự án

''' bash
caro_project/
├── caro-game-client-main/
│ ├── src/
│ │ ├── controller/
│ │ │ ├── Client.java
│ │ │ └── SocketHandle.java
│ │ ├── model/
│ │ │ ├── Point.java
│ │ │ ├── User.java
│ │ │ └── XOButton.java
│ │ └── view/
│ │ ├── LoginFrm.java
│ │ ├── MainFrm.java
│ │ ├── CompetitorInfoFrm.form
│ │ ├── CompetitorInfoFrm.java
│ │ ├── CreateRoomPasswordFrm.form
│ │ ├── CreateRoomPasswordFrm.java
│ │ ├── GameFrm.java
│ │ ├── WaitRoomFrm.java
│ │ └── (các form khác)
│ ├── build.xml
│ ├── manifest.mf
│ └── caro-game-client-main.iml
│
└── caro-game-server/
├── src/
│ ├── controller/
│ │ ├── Server.java
│ │ ├── ServerThread.java
│ │ └── RoomManager.java
│ ├── model/
│ │ ├── User.java
│ │ ├── Room.java
│ │ └── DatabaseConnection.java
│ └── view/
│ └── ServerFrm.java
├── build.xml
└── manifest.mf

markdown
Sao chép mã

---

## 🧩 Chức năng chính

### 🖥️ **Server**
- Xử lý nhiều client cùng lúc bằng **multi-thread (ServerThread)**.
- Quản lý user, phòng chơi, kết quả.
- Kết nối với **MySQL Database** qua JDBC.
- Gửi và nhận dữ liệu dưới dạng chuỗi (protocol tùy chỉnh).

### 🎮 **Client**
- Giao diện người dùng được thiết kế bằng **Java Swing**.
- Kết nối đến server qua `SocketHandle`.
- Các tính năng chính:
  - Đăng ký / Đăng nhập người chơi.
  - Tạo phòng hoặc tham gia phòng có mật khẩu.
  - Hiển thị thông tin đối thủ (`CompetitorInfoFrm`).
  - Thực hiện lượt đánh và kiểm tra thắng thua.
  - Chat với đối thủ trong phòng chơi.
  - Hiển thị danh sách người chơi online, phòng đang mở.

---

## 🗄️ Cơ sở dữ liệu

### 1️⃣ Tạo Database
1. Mở **XAMPP Control Panel** → Start **Apache** và **MySQL**.
2. Mở trình duyệt:  
   👉 http://localhost/phpmyadmin  
3. Tạo **database mới** tên `caro_database`.
4. Import file `caro_database.sql` (nếu có) bằng tab **Import**.

### 2️⃣ Cấu trúc bảng ví dụ
| Bảng | Mô tả |
|------|--------|
| `user` | Lưu thông tin tài khoản (id, username, password, score, status) |
| `match_history` | Ghi lại lịch sử các trận đấu |
| `room` | Lưu thông tin phòng chơi hiện tại |

### 3️⃣ Cấu hình kết nối
Trong file cấu hình database của server (ví dụ `DatabaseConnection.java`):
```java
private static final String URL = "jdbc:mysql://localhost:3306/caro_database";
private static final String USER = "root";
private static final String PASSWORD = "";
🚀 Cách chạy dự án
🧰 Yêu cầu môi trường
JDK 8 trở lên

NetBeans IDE 8.2 hoặc IntelliJ IDEA

XAMPP (MySQL + Apache)

Git (nếu clone từ GitHub)

🖥️ 1. Chạy Server
Mở thư mục caro-game-server trong IDE.

Mở file Server.java hoặc ServerFrm.java.

Chạy chương trình (Run).

Console sẽ hiển thị:

nginx
Sao chép mã
Server started on port 2209
Waiting for client connections...
💻 2. Chạy Client
Mở thư mục caro-game-client-main trong IDE.

Mở file MainFrm.java hoặc Client.java.

Chạy ứng dụng.

Trong màn hình kết nối, nhập:

yaml
Sao chép mã
IP: 127.0.0.1
Port: 2209
→ Kết nối đến server local.

Đăng nhập hoặc đăng ký tài khoản để vào phòng chơi.

🔍 Các Form quan trọng
Form	Vai trò	Mô tả
LoginFrm	Đăng nhập	Cho phép người dùng nhập tài khoản/mật khẩu
MainFrm	Trang chính	Hiển thị danh sách phòng và người chơi
CompetitorInfoFrm	Thông tin đối thủ	Xem thông tin, rank, kết quả của người chơi đối diện
CreateRoomPasswordFrm	Tạo phòng	Cho phép tạo phòng có mật khẩu
GameFrm	Giao diện chơi	Bàn cờ, xử lý lượt đánh, hiển thị kết quả
WaitRoomFrm	Chờ người chơi	Hiển thị khi đang đợi đối thủ vào phòng

🧠 Nguyên lý hoạt động
Client khởi tạo kết nối tới Server bằng socket TCP.

Server tạo thread riêng cho từng client (ServerThread).

Dữ liệu trao đổi qua chuỗi JSON hoặc định dạng riêng:

json
Sao chép mã
{ "action": "create_room", "data": "roomName=ABC" }
Server xử lý và phản hồi lại client tương ứng.

Khi 2 client cùng trong 1 phòng:

Một client gửi tọa độ nước đi → server nhận và gửi lại cho đối phương.

Kiểm tra thắng thua và cập nhật database.

