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
- Nút **🎬 Xem lại cả hành trình**: phát toàn màn hình như một video tổng hợp, từ màn tên hai bạn, đến từng năm (tiêu đề, địa điểm, lời kể, ảnh chuyển động nhẹ, thư tình đã mở khóa), kết bằng số ngày bên nhau. Có thanh tiến trình, nút tạm dừng, tới/lui cảnh, toàn màn hình. Phím: cách = tạm dừng, mũi tên = chuyển cảnh, Esc = đóng; vuốt trái/phải trên điện thoại. Thư còn khóa sẽ không xuất hiện trong video.
- Bấm vào tên hai bạn hoặc bộ đếm để thả tim.

## Đổi nhạc, tên, ngày cưới

Trong bảng quản trị, mục **Cài đặt chung**. Ô **Nhạc nền** nhận nhiều bài, **mỗi dòng một bài** (tên file hoặc link MP3): các bài phát lần lượt, hết bài cuối thì quay lại bài đầu; chỉ có một bài thì lặp lại bài đó; bài nào lỗi sẽ tự được bỏ qua. File nhạc phải nằm cùng thư mục với `index.html` trên GitHub (Add file → Upload files). Chỉ dùng nhạc bạn có quyền sử dụng.

Cloud name và Upload preset của Cloudinary được đặt sẵn trong code (hằng `DEFAULTS` ở đầu phần `<script>` của `index.html`: `cloudName`, `uploadPreset`). Preset phải ở chế độ **Unsigned**. Nếu đổi tài khoản Cloudinary thì sửa hai giá trị đó.

## Góc tâm sự (khách để lại lời chúc)

Lời nhắn lưu ở collection riêng `guestnotes`. Cần thêm luật này vào Firestore Rules (trong cùng `match /databases/{database}/documents { ... }`):

```
match /guestnotes/{id} {
  allow read: if true;
  allow create: if request.resource.data.text is string
                && request.resource.data.text.size() <= 120
                && request.resource.data.author.size() <= 40;
  allow delete: if request.auth != null && request.auth.uid == "MbkvgXFwvqPC7Xm9ZCOR2qmLz953";
}
```

Không mở quyền ghi công khai cho `settings/main`, vì như vậy ai cũng ghi đè được dữ liệu của bạn.

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
