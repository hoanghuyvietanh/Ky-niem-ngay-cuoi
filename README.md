# Kỷ niệm ngày cưới ♥

Trang web kỷ niệm ngày cưới: mỗi năm một mục, thêm ảnh ngay trên web, có nhạc nền, bộ đếm ngày bên nhau, cột mốc và thư hẹn giờ.

Web: https://hoanghuyvietanh.github.io/Ky-niem-ngay-cuoi/

## Cấu trúc

| File | Công dụng |
|---|---|
| `index.html` | Toàn bộ trang web (giao diện + code) |
| `NoiNayCoAnh.mp3` | Nhạc nền |
| `og.png` | Ảnh hiện ra khi gửi link qua Zalo, Facebook, Messenger (nên thay bằng ảnh cưới của hai bạn, cỡ 1200x630) |

Dữ liệu (tên, ngày cưới, danh sách ảnh, lời kể, thư tình) lưu trên **Firebase Firestore**. Ảnh lưu trên **Cloudinary**. Không có gì phải sửa trong code khi thêm nội dung mới.

## Mỗi năm thêm kỷ niệm mới

1. Mở web, **chạm 5 lần vào trái tim ♥ ở chân trang**, đăng nhập bằng tài khoản quản trị.
2. Mục **Thêm ảnh mới**: nhập năm, bấm chọn ảnh từ máy (chọn được nhiều ảnh), nhập lời tựa nếu muốn, bấm **Tải ảnh lên và thêm vào năm này**. Trang tự thu nhỏ ảnh trước khi tải.
3. Mục **Thông tin từng năm**: nhập địa điểm, lời kể ngắn, thư tình. Có thể **khóa thư đến một ngày** (ví dụ ngày kỷ niệm năm sau): trước ngày đó thư hiện ổ khóa 🔒 kèm đếm ngược. Đây chỉ là khóa vui, người rành kỹ thuật vẫn đọc được nội dung.
4. Mục **Ảnh đã thêm**: xóa từng ảnh. Mục **Xóa cả một năm**: xóa toàn bộ ảnh của một năm.

## Sao lưu và khôi phục

Trong bảng quản trị, mục **Sao lưu và khôi phục**:

- **Tải bản sao lưu về máy:** tải một file `sao-luu-ky-niem-<ngày>.json` vào thư mục Tải xuống. File chứa danh sách ảnh (đường dẫn), lời kể, thư tình, cài đặt; **không chứa chính các tấm ảnh** (ảnh nằm ở Cloudinary, nên giữ ảnh gốc trong máy và đừng xóa tài khoản Cloudinary). Nên chép file vào Google Drive hoặc email cho chính mình. Đừng đưa file lên GitHub công khai vì có thư tình.
- **Khôi phục từ file sao lưu:** chọn lại file `.json`, xác nhận, dữ liệu hiện tại sẽ bị ghi đè bằng bản sao lưu. Trước khi ghi đè trang tự tải về một bản `truoc-khoi-phuc-<ngày>.json` để bạn có đường lùi.

Nên sao lưu sau mỗi lần thêm nhiều ảnh hoặc viết thư mới.

## Tính năng trên trang

- Màn hình mở thiệp: bấm phong bì thì nhạc tự phát.
- Bộ đếm năm, tháng, ngày; dòng thống kê vui xoay vòng; đếm ngược tới kỷ niệm kế tiếp (đúng ngày có tim bay chúc mừng).
- Cột mốc 100, 200, 300, 500, 1000... ngày bên nhau.
- Nút **🎲 Kỷ niệm ngẫu nhiên** và **▶ Trình chiếu** (phím cách để tạm dừng, mũi tên để chuyển ảnh, vuốt trái phải trên điện thoại).
- Bấm vào tên hai bạn hoặc bộ đếm để thả tim.

## Đổi nhạc, tên, ngày cưới, Cloudinary

Trong bảng quản trị, mục **Cài đặt chung**. File nhạc phải nằm cùng thư mục với `index.html` trên GitHub. Chỉ dùng nhạc bạn có quyền sử dụng. Cloud name và Upload preset (phải đặt chế độ **Unsigned**) cũng sửa ở đây.

## Bảo mật

- Quyền ghi dữ liệu do **Firestore Rules** kiểm soát: ai cũng đọc được, chỉ tài khoản quản trị (đúng UID) mới ghi được. Không có mật khẩu nào trong code.
- `apiKey` của Firebase trong `index.html` không phải bí mật, để công khai được. Cloudinary preset Unsigned cũng chỉ cho phép tải ảnh lên, không cho xóa hay đọc tài khoản; không bao giờ đưa **API Secret** của Cloudinary vào code.
- Repo công khai và dữ liệu đọc được bởi mọi người có link, nên đừng đăng ảnh hoặc thư quá riêng tư.
- Nếu đổi tài khoản quản trị, vào Firebase Console → Firestore → Rules và thay UID mới.

## Gỡ lỗi nhanh

| Hiện tượng | Cách xử lý |
|---|---|
| Sửa code xong mà web vẫn cũ | Chờ 1 phút (GitHub Pages build), rồi bấm Ctrl + F5 |
| Đăng nhập báo `auth/unauthorized-domain` | Firebase → Authentication → Settings → Authorized domains, thêm `hoanghuyvietanh.github.io` |
| Lưu dữ liệu báo `permission-denied` | Chưa đăng nhập, hoặc UID trong Rules sai |
| Tải ảnh báo lỗi `preset` / `whitelisted` | Cloudinary → Settings → Upload → mở preset, đặt Signing mode là **Unsigned** |
| Ảnh không hiện | Kiểm tra mạng; xóa ảnh lỗi trong mục "Ảnh đã thêm" rồi tải lại |
| Nhạc không phát | Bấm vào phong bì hoặc nút tròn ở góc phải; kiểm tra tên file mp3 |
