# 📺 Hướng Dẫn Từng Bước Dọn Dẹp & Cài Launcher Mới Cho TV (CleanYourTV)

> **Repository:** [https://github.com/TanDungg/clean-your-tv.git](https://github.com/TanDungg/clean-your-tv.git)  
> **Dành cho:** Android TV và Google TV (Đặc biệt tối ưu cho **Sony BRAVIA**, TCL, Xiaomi, Hisense,...).  
> **Ưu điểm:** Không cần root, không làm mất bảo hành, tăng tốc TV, loại bỏ quảng cáo/theo dõi, có thể khôi phục lại trạng thái cũ bất cứ lúc nào.

---

## 📦 KHI CLONE CODE VỀ MÁY TÍNH MỚI

### 1. Phần mềm cần có sẵn trên máy tính

- **Python** (phiên bản 3.9 trở lên, ví dụ Python 3.11): Dùng để chạy script ADB qua mạng.  
  _(Khi cài đặt Python trên Windows, nhớ tích chọn ô **"Add python.exe to PATH"**)_.
- **Java JDK 17** & **Android SDK** _(Chỉ cần khi muốn build Launcher cài lên TV)_:
  - Cài **Android Studio** là máy sẽ tự động có đầy đủ cả Java (nằm trong thư mục `jbr`) và Android SDK.

### 2. Các lệnh khởi tạo sau khi clone (Chỉ chạy 1 lần duy nhất)

Mở cửa sổ **PowerShell** và chạy lần lượt:

```powershell
# 1. Tải code từ GitHub về máy
git clone https://github.com/TanDungg/clean-your-tv.git
cd clean-your-tv

# 2. Tạo môi trường ảo Python
python -m venv tools\venv

# 3. Cài đặt các thư viện kết nối ADB không dây
tools\venv\Scripts\pip install -r tools\requirements.txt
```

> _(Chạy xong bước này là máy tính đã sẵn sàng 100% để kết nối và thao tác với TV)._

---

## 🛠️ GIAI ĐOẠN 1: Chuẩn Bị Trên TV

1. **Kết nối mạng:** Kết nối TV vào **cùng một mạng Wi-Fi** (hoặc cắm chung dây mạng LAN) với máy tính.
2. **Kích hoạt menu Nhà phát triển:**
   - Trên remote TV, vào **Cài đặt (Settings)** ➔ **Hệ thống (System)** ➔ **Giới thiệu (About)**.
   - Tìm mục **Bản dựng (Build / Android TV OS Build)**.
   - Bấm phím **OK** trên remote **7 lần liên tục** đến khi TV thông báo: _"Bạn đã là nhà phát triển"_.
3. **Bật chế độ Gỡ lỗi (ADB Debugging):**
   - Quay lại menu trước ➔ Chọn **Tùy chọn nhà phát triển (Developer options)**.
   - Bật mục **Gỡ lỗi USB (USB debugging)**. _(Nếu có thêm mục "Gỡ lỗi mạng / Network debugging" thì bật luôn)_.
4. **Xem địa chỉ IP của TV:**
   - Vào **Cài đặt** ➔ **Mạng & Internet** ➔ Bấm vào tên Wi-Fi đang kết nối để xem IP (ví dụ: `192.168.1.50`).

---

## 💻 GIAI ĐOẠN 2: Thao Tác Với TV Trên Máy Tính

Mở ứng dụng **PowerShell** tại thư mục dự án `clean-your-tv`:

### Bước 1: Kết nối thử nghiệm với TV

Thay địa chỉ IP dưới đây bằng **IP thực tế của TV nhà bạn** (đã xem ở Giai đoạn 1):

```powershell
# 1. Đặt địa chỉ IP của TV
$env:TV_HOST = "192.168.1.50"

# 2. Thử kết nối và kiểm tra thông tin TV
tools\venv\Scripts\python tools\tv.py --probe
```

> ⚠️ **LƯU Ý:** Lúc này trên màn hình TV sẽ hiện một popup:  
> **"Cho phép gỡ lỗi USB? (Allow USB debugging?)"**  
> 👉 Hãy dùng remote TV tích chọn **"Luôn cho phép từ máy tính này"** rồi bấm **OK / Cho phép**.  
> Sau khi bấm xong, màn hình PowerShell sẽ in ra đầy đủ model TV, chip xử lý và phiên bản Android.

---

### Bước 2: Tắt ứng dụng rác, quảng cáo và theo dõi (Debloat)

Chọn file danh sách tương ứng với dòng TV của bạn trong thư mục `examples\`:

#### Dành cho TV Sony:

- Nếu là **Sony đời 2022 - 2024 (Google TV 12/14)** như KD-43X80L, X85K, A80K,...:
  ```powershell
  tools\disable.ps1 -List examples\sony-kd-43x80l\debloat.txt
  ```
- Nếu là **Sony dòng VH21 / A80K / X85K**:
  ```powershell
  tools\disable.ps1 -List examples\sony-bravia-4k-vh21-eu\debloat.txt
  ```
- Nếu là **Sony đời Android 9/10** (như KD-55X80J, KD-55XF8096, X900F,...):
  ```powershell
  tools\disable.ps1 -List examples\sony-kd-55xf8096\debloat.txt
  ```
  _(Đối với các hãng khác như TCL, Xiaomi, Philips,... bạn vào thư mục `examples\` để chọn file debloat tương ứng)._

> 💡 **Cơ chế an toàn:** Hệ thống sẽ tự động sao lưu toàn bộ trước khi tắt. Các cổng HDMI, cổng loa eARC, tuner truyền hình và remote đều được giữ nguyên an toàn 100%.

---

### Bước 3: Biên dịch và cài đặt Launcher mới lên TV

Chạy script đóng gói và cài đặt tự động:

```powershell
launcher\build-and-install.ps1
```

Script sẽ tự động:

1. Đóng gói mã nguồn Java thành file APK tối ưu.
2. Cài đặt APK lên TV qua mạng nội bộ.
3. Đặt giao diện Clean Launcher làm màn hình chính (Home) mặc định.

🎉 **Hoàn tất!** Hãy bấm phím **Home** trên remote TV để trải nghiệm giao diện mới mượt mà, tối giản.

---

## 🔒 GIAI ĐOẠN 3: Bảo Mật Sau Khi Cài Xong (Rất Quan Trọng)

Để tránh các thiết bị lạ trong mạng nội bộ can thiệp vào TV của bạn:

1. Vào **Cài đặt** trên TV ➔ **Tùy chọn nhà phát triển** ➔ **Tắt Gỡ lỗi USB (USB debugging)**.
2. Khởi động lại TV (Restart).
3. Trên máy tính, chạy lệnh kiểm tra xem cổng ADB đã đóng an toàn chưa:
   ```powershell
   tools\venv\Scripts\python tools\adb-closed.py $env:TV_HOST
   ```
   _Nếu hiển thị thông báo `... is closed` là TV đã được bảo mật tuyệt đối._

---

## 🔄 CÁCH HOÀN TÁC (Khôi Phục Lại Giao Diện Gốc Cũ)

Nếu bất kỳ lúc nào bạn muốn quay lại trạng thái xuất xưởng ban đầu:

1. Bật lại **USB debugging** trên TV.
2. Chạy script khôi phục trên PowerShell:
   ```powershell
   $env:TV_HOST = "192.168.1.50"
   tools\restore.ps1 -List examples\sony-kd-43x80l\debloat.txt
   ```
   Toàn bộ các ứng dụng và màn hình gốc của TV sẽ được kích hoạt lại y như lúc chưa làm.

---

## ❓ Xử Lý Sự Cố Thường Gặp

| Tình huống                                                      | Cách xử lý                                                                                                            |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Lệnh `tv.py --probe` báo lỗi timed out / không kết nối được** | Kiểm tra TV và máy tính có bắt chung một mạng Wi-Fi không; kiểm tra xem TV có bị đổi IP không.                        |
| **Không thấy hiện popup "Cho phép gỡ lỗi" trên TV**             | Tắt công tắc USB debugging trên TV rồi bật lại, sau đó chạy lại lệnh `tools\venv\Scripts\python tools\tv.py --probe`. |
| **Cổng HDMI hoặc Remote không nhận sau khi cài**                | Khởi động lại TV (rút nguồn 10 giây cắm lại để TV cold-boot). Các driver HDMI và remote luôn được mã nguồn bảo vệ.    |
