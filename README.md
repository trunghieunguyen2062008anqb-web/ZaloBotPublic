# 🤖 Zalo Auto-Reply Bot (Phiên bản Công khai - Public)

Chào mừng bạn đến với **Zalo Auto-Reply Bot**! Đây là phiên bản gọn nhẹ và bảo mật nhất giúp tài khoản Zalo cá nhân tự động phản hồi tin nhắn của bạn bè/khách hàng khi bạn bận hoặc offline.

---

## ✨ Tính năng nổi bật
* **Tự động trả lời 1-1:** Tự động gửi tin nhắn phản hồi mẫu khi có người nhắn tin riêng đến bạn.
* **Chống spam thông minh (Cooldown 2 tiếng):** Bot chỉ trả lời duy nhất 1 lần đầu tiên cho mỗi người. Các tin nhắn tiếp theo của họ trong vòng 2 tiếng sẽ bị bỏ qua để tránh gây phiền tối.
* **Nhận diện chính chủ:** Khi bạn chủ động nhắn tin trả lời họ, thời gian cooldown sẽ được tự động làm mới để sẵn sàng cho lần bận tiếp theo.
* **Siêu nhẹ:** Lược bỏ hoàn toàn giao diện web và tính năng rải tin nhóm để bot hoạt động ổn định nhất, không tốn tài nguyên.

---

## 🛠️ Hướng dẫn cài đặt và sử dụng chi tiết từ A - Z

### Bước 1: Cài đặt Python trên máy tính
1. Tải và cài đặt Python phiên bản mới nhất (từ 3.8 trở lên) tại: [python.org/downloads](https://www.python.org/downloads/).
2. *Lưu ý quan trọng:* Trong quá trình cài đặt, bạn **phải tích chọn** vào ô **"Add Python to PATH"** (hoặc **"Add python.exe to PATH"**) trước khi nhấn Install.

### Bước 2: Tải code và Cài đặt thư viện
1. Tải toàn bộ thư mục code này về máy tính của bạn và giải nén.
2. Mở cửa sổ dòng lệnh (CMD hoặc PowerShell) tại thư mục chứa code và chạy lệnh sau để cài đặt thư viện:
   ```bash
   pip install -r requirements.txt
   ```

### Bước 3: Cấu hình tài khoản Zalo của bạn (File `.env`)
1. Nhân bản tệp **`env.example`** và đổi tên thành **`.env`**.
2. Mở file **`.env`** vừa tạo lên bằng NotePad hoặc VS Code và điền thông tin của bạn vào:
   * **`COOKIE`**: Chuỗi cookie đăng nhập Zalo Web của bạn.
   * **`IMEI`**: Mã định danh thiết bị trình duyệt Zalo của bạn.
   * **`REPLY_MESSAGE`**: Nội dung tin nhắn bạn muốn tự động phản hồi cho khách hàng.

> [!TIP]
> **🛡️ Cơ chế Bảo mật Tự động:** Khi bot khởi chạy lần đầu tiên, nó sẽ tự động mã hóa các thông tin nhạy cảm của bạn (`COOKIE` và `IMEI`) trong file `.env` bằng khóa phần cứng máy tính (Hardware Key). 
> * Sau khi mã hóa, file `.env` sẽ chỉ hiển thị các ký tự mã hóa vô nghĩa dạng `enc:...` để ngăn người khác mở ra đọc trộm Cookie của bạn.
> * File `.env` này chỉ có thể giải mã và chạy trên chính máy tính của bạn. Nếu bị copy sang máy tính khác, bot sẽ từ chối chạy để bảo vệ tài khoản của bạn.
> * Nếu bạn muốn cập nhật Cookie mới, chỉ cần xóa file `.env` đi, tạo lại từ `env.example` và chạy lại bot!

---

## 🍪 Hướng dẫn chi tiết cách lấy COOKIE và IMEI Zalo Web

### 1. Cách lấy COOKIE:
1. Dùng trình duyệt máy tính (Chrome, Cốc Cốc, Edge...) truy cập và đăng nhập tài khoản của bạn tại: **[chat.zalo.me](https://chat.zalo.me)**.
2. Nhấn phím **F12** trên bàn phím (hoặc chuột phải chọn **Kiểm tra/Inspect**).
3. Chọn tab **Network** (Mạng) ở thanh công cụ phía trên.
4. Bấm vào một cuộc hội thoại bất kỳ trên Zalo Web để trình duyệt tải dữ liệu.
5. Ở cột bên trái xuất hiện các dòng tên mạng, nhấp vào dòng tên là **`getgroupinfo`** (hoặc `checkconnect`, `send_msg`...).
6. Nhìn sang bảng chi tiết bên phải, chọn tab **Headers** (Tiêu đề).
7. Kéo xuống mục **Request Headers** (Tiêu đề yêu cầu) $\rightarrow$ Tìm thuộc tính **`cookie:`**.
8. Copy toàn bộ chuỗi ký tự đứng sau chữ `cookie:` (bắt đầu bằng `_zi=...`). Dán chuỗi này vào mục `COOKIE=` trong file `.env`.

### 2. Cách lấy IMEI:
1. Vẫn tại trang F12 Zalo Web đó, chọn tab **Console** (Bảng điều khiển).
2. Sao chép toàn bộ đoạn mã bên dưới, dán vào bảng Console rồi nhấn **Enter**:
   ```javascript
   (function(){for(let i=0;i<localStorage.length;i++){let k=localStorage.key(i),v=localStorage.getItem(k);if(v&&v.includes('imei')){try{let p=JSON.parse(v);if(p&&p.imei){console.log("🟢 IMEI của bạn là:",p.imei);return;}}catch(e){}}}let d=localStorage.getItem('zalo_imei')||localStorage.getItem('imei');if(d)console.log("🟢 IMEI của bạn là:",d);else console.log("❌ Hãy đăng nhập chat.zalo.me trước!");})();
   ```
3. Copy dòng mã IMEI vừa được in ra (dạng chữ và số dài ngăn cách bởi dấu gạch ngang) dán vào mục `IMEI=` trong file `.env`.

---

## 🚀 Khởi chạy Bot
Chạy lệnh sau tại thư mục dự án để khởi động bot:
```bash
python bot.py
```
Khi màn hình in ra dòng chữ:
`🟢 Kết nối thành công! Đang trực tuyến và lắng nghe tin nhắn...`
Tức là bot đã bắt đầu hoạt động và tự động trực trả lời tin nhắn Zalo 24/24 cho bạn!

---

## 📦 Hướng dẫn đóng gói thành file `.exe` (Chạy không cần cài Python)
Nếu bạn muốn đóng gói thành file `.exe` duy nhất để nhấp đúp chạy luôn:
1. Chạy lệnh cài đặt thư viện đóng gói:
   ```bash
   pip install pyinstaller
   ```
2. Chạy lệnh biên dịch:
   ```bash
   pyinstaller --onefile --console bot.py
   ```
3. File chạy **`bot.exe`** sẽ được tạo ra nằm trong thư mục **`dist/`**. Bạn chỉ cần chia sẻ file `bot.exe` này và file `.env` cho mọi người sử dụng là xong!

---

*Phát triển bởi **trunghieunguyen2062008anqb-web** & **Antigravity***
