# Kỷ niệm ngày cưới ♥

Trang web kỷ niệm ngày cưới: mỗi năm một mục, thêm ảnh ngay trên web, có nhạc nền, bộ đếm ngày bên nhau và các cột mốc.

Web: https://hoanghuyvietanh.github.io/Ky-niem-ngay-cuoi/

## Cấu trúc

| File | Công dụng |
|---|---|
| `index.html` | Toàn bộ trang web (giao diện + code) |
| `NoiNayCoAnh.mp3` | Nhạc nền |
| `og.png` | Ảnh hiện ra khi gửi link qua Zalo, Facebook, Messenger (nên thay bằng ảnh cưới của hai bạn, cỡ 1200x630) |
| `photos/` | (nên tạo) Ảnh kỷ niệm, chia theo năm: `photos/2026/01.jpg` |

Dữ liệu (tên, ngày cưới, danh sách ảnh, lời kể, thư tình) lưu trên **Firebase Firestore**, không nằm trong code.

## Mỗi năm thêm kỷ niệm mới

1. Thu nhỏ ảnh trước khi tải lên (cạnh dài khoảng 1600px, dưới 500KB mỗi ảnh).
2. Trên GitHub: **Add file → Upload files**, đưa ảnh vào thư mục `photos/<năm>/`, đặt tên không dấu, không khoảng trắng (`01.jpg`, `bien-vung-tau.jpg`), rồi **Commit changes**.
3. Mở web, **chạm 5 lần vào trái tim ♥ ở chân trang**, đăng nhập bằng tài khoản quản trị.
4. Mục **Thêm ảnh mới**: nhập năm, dán đường dẫn ảnh (ví dụ `photos/2027/01.jpg`), thêm lời tựa, bấm thêm.
5. Mục **Thông tin từng năm**: nhập địa điểm, lời kể ngắn và thư tình cho năm đó.

> Dán link Google Drive cũng được, nhưng không bền (Google có thể giới hạn lượt tải). Nên để ảnh trong thư mục `photos/` của repo.

## Đổi nhạc, tên, ngày cưới

Trong bảng quản trị, mục **Cài đặt chung**. File nhạc phải nằm cùng thư mục với `index.html`. Chỉ dùng nhạc bạn có quyền sử dụng.

## Bảo mật

- Quyền ghi dữ liệu do **Firestore Rules** kiểm soát: ai cũng đọc được, chỉ tài khoản quản trị (đúng UID) mới ghi được. Không có mật khẩu nào trong code.
- `apiKey` của Firebase trong `index.html` không phải bí mật, để công khai được.
- Repo công khai và dữ liệu đọc được bởi mọi người có link, nên đừng đăng ảnh hoặc thư quá riêng tư.
- Nếu đổi tài khoản quản trị, vào Firebase Console → Firestore → Rules và thay UID mới.

## Gỡ lỗi nhanh

| Hiện tượng | Cách xử lý |
|---|---|
| Sửa code xong mà web vẫn cũ | Chờ 1 phút (GitHub Pages build), rồi bấm Ctrl + F5 |
| Đăng nhập báo `auth/unauthorized-domain` | Firebase → Authentication → Settings → Authorized domains, thêm `hoanghuyvietanh.github.io` |
| Lưu dữ liệu báo `permission-denied` | Chưa đăng nhập, hoặc UID trong Rules sai |
| Ảnh không hiện | Kiểm tra đường dẫn và chữ hoa/thường trong tên file |
| Nhạc không phát | Bấm vào phong bì hoặc nút tròn ở góc phải; kiểm tra tên file mp3 |
