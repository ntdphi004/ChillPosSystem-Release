# ChillPOS — Bản phát hành (Windows / macOS / Linux)

**Hệ thống POS portable** cho quán cafe / F&B: order QR, quầy thu ngân, bếp (KDS), in bill LAN, chạy offline ngay trên máy — **không cần cài XAMPP / WAMP**.

Tải file đóng gói tại trang **[Releases](https://github.com/ntdphi004/ChillPosSystem-Release/releases)**.

---

## Chọn bản phù hợp

| Hệ điều hành | File tải | Cách chạy |
|--------------|----------|-----------|
| **Windows 10/11** | `ChillPos_POS_Release.zip` | Giải nén → double-click `ChillPOS-Start.exe` |
| **macOS Apple Silicon** (M1/M2/M3/M4) | `ChillPos_POS_Release_macOS_arm64.zip` | Giải nén → mở `ChillPOS-Start.command` |
| **macOS Intel** | `ChillPos_POS_Release_macOS_x64.zip` | Giải nén → mở `ChillPOS-Start.command` |
| **Linux x86_64** | `ChillPos_POS_Release_linux_x64.zip` | Giải nén → `./ChillPOS-Start.sh` |
| **Linux ARM64** | `ChillPos_POS_Release_linux_arm64.zip` | Giải nén → `./ChillPOS-Start.sh` |
| **Linux (Docker, tuỳ chọn)** | Source + Compose | `docker compose up` |

> Linux portable cũng kèm **Caddy + PHP + MySQL** (giống Windows/macOS). Docker chỉ là lựa chọn thay thế.

---

## Yêu cầu hệ thống

### Windows portable

- Windows 10 hoặc 11 (64-bit)
- ~1.5 GB trống trên ổ đĩa
- Cổng tự do: **8999** (web), **9990** (PHP), **33306** (MySQL)
- Không cần cài PHP/MySQL/Caddy trước

### macOS portable

- macOS **14+** (Sonoma trở lên)
- Chip Apple Silicon **hoặc** Intel đúng đúng file zip
- ~1.5 GB trống
- Cùng cổng 8999 / 9990 / 33306
- Lần đầu có thể cần bỏ qua Gatekeeper (xem bên dưới)

### Linux portable

- glibc **2.28+** (Ubuntu 20.04+, Debian 10+, RHEL 8+)
- ~2 GB trống; đúng chip **x64** hoặc **arm64**
- Cổng tự do: **8999** / **9990** / **33306**
- Không cần cài PHP/MySQL/Caddy hệ thống

### Linux Docker (tuỳ chọn)

- Docker Engine + Compose plugin
- ~2 GB trống cho image + volume

---

## Cài đặt & chạy — Windows (portable)

### 1. Tải và giải nén

1. Vào [Releases](https://github.com/ntdphi004/ChillPosSystem-Release/releases) → tải `ChillPos_POS_Release.zip`.
2. Giải nén vào đường dẫn **ngắn, không có dấu cách**, ví dụ:
   - `C:\ChillPOS`
   - `D:\POS\ChillPOS`
3. **Không** chạy trực tiếp từ trong file zip.

### 2. Khởi động

1. Double-click **`ChillPOS-Start.exe`**  
   (hoặc `start.bat` nếu EXE bị SmartScreen chặn).
2. Lần đầu có thể mất 30–90 giây (khởi tạo MySQL + migrate + demo).
3. Trình duyệt mở POS tại: **http://127.0.0.1:8999**

### 3. Tắt máy chủ

- Double-click **`ChillPOS-Stop.exe`** (hoặc `stop.bat`).

### 4. Cho tablet / máy bếp cùng Wi‑Fi (LAN)

1. Chạy **`allow_lan.bat`** (chọn Yes khi Windows hỏi Admin).
2. Script sẽ:
   - mở cổng 8999 trên firewall (profile Private),
   - lắng nghe LAN,
   - cập nhật `APP_URL` theo IP máy.
3. Trên tablet/bếp mở đúng URL in ra, ví dụ: `http://192.168.1.25:8999`  
   (**không** dùng `127.0.0.1` trên máy khác).

### 5. Máy in USB (Windows)

1. Cắm máy in, cài driver Windows nếu cần.
2. Chạy `list-printers.bat` để xem tên máy in.
3. Làm theo `HUONG_DAN_CAU_HINH_LOCAL_PRINTER_BRIDGE.md`.
4. Chạy `start-printer-bridge.bat` rồi cấu hình chế độ `local_bridge_http` trong POS/Admin.

Chi tiết thêm: [`docs/HUONG_DAN_WINDOWS.txt`](docs/HUONG_DAN_WINDOWS.txt)

---

## Cài đặt & chạy — macOS (portable)

### 1. Tải đúng kiến trúc

| Chip | File |
|------|------|
| Apple Silicon (M1+) | `ChillPos_POS_Release_macOS_arm64.zip` |
| Intel | `ChillPos_POS_Release_macOS_x64.zip` |

Kiểm tra chip: menu Apple → About This Mac.

### 2. Giải nén

- Giải nén vào thư mục **không có khoảng trắng**, ví dụ `~/ChillPOS`.
- Tránh giải nén vào iCloud Desktop/Downloads nếu đồng bộ chậm.

### 3. Mở lần đầu (Gatekeeper)

Nếu macOS báo “không thể mở vì không xác định được nhà phát triển”:

```bash
cd ~/ChillPOS   # đổi đúng đường dẫn bạn giải nén
xattr -cr .
chmod +x ChillPOS-Start.command ChillPOS-Stop.command start.sh stop.sh
open ChillPOS-Start.command
```

Hoặc: System Settings → Privacy & Security → **Open Anyway**.

### 4. Chạy / dừng

- Start: double-click **`ChillPOS-Start.command`**
- Stop: **`ChillPOS-Stop.command`**
- Web: **http://127.0.0.1:8999**

### 5. In trên macOS

Portable macOS **không** kèm USB printer-bridge Windows. Dùng:

- `network_tcp` (máy in LAN ESC/POS), hoặc
- `browser_bridge` tại `/printer/{store_uuid}` trên thiết bị cầm tay.

Chi tiết: [`docs/HUONG_DAN_MACOS.txt`](docs/HUONG_DAN_MACOS.txt)

---

## Cài đặt & chạy — Linux (portable)

### 1. Tải đúng kiến trúc

| Chip | File |
|------|------|
| x86_64 / amd64 | `ChillPos_POS_Release_linux_x64.zip` |
| ARM64 / aarch64 | `ChillPos_POS_Release_linux_arm64.zip` |

Kiểm tra: `uname -m` → `x86_64` hoặc `aarch64`.

### 2. Giải nén & chạy

```bash
cd ~/ChillPOS   # đổi đúng đường dẫn giải nén (không có khoảng trắng)
chmod +x ChillPOS-Start.sh ChillPOS-Stop.sh start.sh stop.sh allow_lan.sh
./ChillPOS-Start.sh
```

Web: **http://127.0.0.1:8999** — Stop: `./ChillPOS-Stop.sh`

### 3. LAN (tablet / bếp)

```bash
./allow_lan.sh
./ChillPOS-Stop.sh && ./ChillPOS-Start.sh
# Mở http://<IP-LAN>:8999 trên thiết bị khác
```

### 4. Docker (tuỳ chọn)

Xem [`docs/HUONG_DAN_LINUX_DOCKER.txt`](docs/HUONG_DAN_LINUX_DOCKER.txt) mục B.

---

---

## Đăng nhập lần đầu (demo)

Sau migrate `--with-demo`, tài khoản bootstrap thường là:

| Mục | Giá trị |
|-----|---------|
| User | `owner` |
| Password | xem thông báo trên màn hình / tài liệu bàn giao (đổi ngay sau khi vào) |

**Bắt buộc:** đổi mật khẩu owner ngay trong Admin trước khi đưa vào vận hành thật.

---

## Cổng mạng (portable Win/macOS)

| Dịch vụ | Cổng | Ghi chú |
|---------|------|---------|
| Web POS (Caddy) | **8999** | Mở cho LAN khi chạy `allow_lan` |
| PHP FastCGI | **9990** | Chỉ localhost |
| MySQL | **33306** | Chỉ localhost |

Nếu cổng bị chiếm, tắt app khác hoặc dừng instance ChillPOS cũ bằng Stop.

---

## Kiểm tra nhanh sau khi chạy

1. Trang chủ `http://127.0.0.1:8999` mở được.
2. Đăng nhập owner thành công.
3. Tạo / xem bàn và menu demo.
4. (Windows) In thử sau khi cấu hình printer bridge.
5. (LAN) Tablet mở được bằng IP máy chủ, không phải 127.0.0.1.

Script smoke trên macOS package: `./macos-smoke-check.sh` (sau khi Start thành công).

---

## Sự cố thường gặp

| Hiện tượng | Cách xử lý |
|------------|------------|
| SmartScreen chặn EXE (Windows) | More info → Run anyway, hoặc dùng `start.bat` |
| Gatekeeper chặn `.command` (macOS) | `xattr -cr .` rồi Open Anyway |
| Trang không mở / trắng | Chạy Stop rồi Start lại; kiểm tra cổng 8999 |
| “Port already in use” | Stop ChillPOS cũ; `netstat` kiểm tra process chiếm cổng |
| Tablet không vào được | Chạy `allow_lan.bat`, dùng IP LAN, Wi‑Fi cùng mạng, firewall Private |
| In USB không ra (Windows) | Bridge chưa chạy / sai tên máy in / sai port bridge |
| Thiếu extension PHP / lỗi migrate | Gói zip lỗi — tải lại bản Release mới nhất |
| Đường dẫn có dấu cách | Giải nén lại vào `C:\ChillPOS` hoặc `~/ChillPOS` |

---

## Nội dung gói portable (Win/macOS)

```
ChillPOS/
  ChillPOS-Start.*     # khởi động
  ChillPOS-Stop.*      # tắt
  caddy/               # web server
  php/                 # PHP runtime
  mysql/               # MySQL runtime (chưa có data — tạo lần đầu)
  www/                 # mã ứng dụng + .env portable
  printer-bridge/      # chỉ Windows
  HUONG_DAN_*.txt
```

- `mysql/data` được tạo **tại máy khách** lần đầu — không nằm trong zip.
- HTTP **không** gọi được `/api/migrate.php` (chỉ chạy lúc Start).

---

## Bảo mật nhanh

- Portable mặc định bind localhost; chỉ mở LAN khi thật sự cần (`allow_lan`).
- Đổi mật khẩu owner ngay.
- Không chia sẻ thư mục `mysql/data` / `.env` công khai.
- Bản release **không** kèm `license.key` developer; kích hoạt theo quy trình bàn giao riêng.

---

## Liên kết

- Hướng dẫn Windows: [`docs/HUONG_DAN_WINDOWS.txt`](docs/HUONG_DAN_WINDOWS.txt)
- Hướng dẫn macOS: [`docs/HUONG_DAN_MACOS.txt`](docs/HUONG_DAN_MACOS.txt)
- Hướng dẫn Linux/Docker: [`docs/HUONG_DAN_LINUX_DOCKER.txt`](docs/HUONG_DAN_LINUX_DOCKER.txt)
- Checklist cài đặt: [`CUSTOMER_INSTALL.txt`](CUSTOMER_INSTALL.txt)
- Tải bản dựng: [Releases](https://github.com/ntdphi004/ChillPosSystem-Release/releases)

---

© ChillPOS — repo này chỉ chứa hành bản đóng gói + hướng dẫn khách hàng.
Source phát triển nằm ở repo riêng (private).
