# HƯỚNG DẪN ĐỔI HÌNH NÈN & GIAO DIỆN LAUNCHER

Thư mục cấu hình hiện tại của bạn: `themes/custom/`

---

## 1. Cách đổi hình nền tự chọn

1. **Chuẩn bị ảnh**:
   - Chọn bất kỳ bức ảnh nào bạn thích (khuyên dùng tỉ lệ **16:9**, độ phân giải **1920x1080**, dung lượng dưới 500 KB để TV load mượt nhất).
   - Đổi tên file ảnh thành: `wallpaper.jpg`

2. **Chép ảnh vào thư mục**:
   - Lưu đè file `wallpaper.jpg` vào đường dẫn:
     `themes\custom\wallpaper.jpg`

3. **Chạy lệnh cập nhật lên TV**:
   Mở PowerShell tại thư mục dự án và chạy:
   ```powershell
   $env:TV_HOST = "192.168.1.99"
   .\tools\theme.ps1 themes\custom
   ```
   *(Màn hình TV sẽ chớp nhẹ và đổi sang hình nền mới ngay lập tức)*.

---

## 2. Dùng các hình nền có sẵn tuyệt đẹp trong dự án

Dự án có sẵn một số bộ hình nền phong cảnh và cực quang rất đẹp:

- **Bộ 14 hình nền phong cảnh thiên nhiên (tự đổi mỗi giờ)**:
  ```powershell
  .\tools\theme.ps1 themes\scenes
  ```
- **Bộ cực quang xanh tím (Aurora)**:
  ```powershell
  .\tools\theme.ps1 themes\aurora
  ```
- **Bộ phong cảnh hồ nước chiều thu (Ruska)**:
  ```powershell
  .\tools\theme.ps1 themes\ruska
  ```
- **Quay lại theme custom của bạn (YouTube, TV360, Netflix, FPT Play)**:
  ```powershell
  .\tools\theme.ps1 themes\custom
  ```
- **Bỏ hình nền, quay về màu đen mặc định**:
  ```powershell
  .\tools\theme.ps1 -Reset
  ```

---

## 3. Chỉnh sửa thứ tự app hoặc tuỳ chọn trong `config.json`
Mọi thay đổi trong file `themes\custom\config.json` (thêm app, đổi số dòng, tắt/bật hiệu ứng) chỉ cần lưu lại rồi chạy lại lệnh:
```powershell
.\tools\theme.ps1 themes\custom
```
