# Hướng dẫn đưa website KAIZENCONS lên mạng

Mục tiêu: đưa web lên mạng miễn phí (Netlify) và bật được **đăng nhập trang quản trị `/admin`** để bạn tự thêm dự án, tin tức.

Có 3 phần, làm lần lượt. Khoảng 20–30 phút, hoàn toàn miễn phí.

---

## PHẦN 1 — Đưa code lên GitHub

1. Đăng ký tài khoản tại https://github.com (nếu chưa có). **Ghi nhớ tên tài khoản** (username), lát nữa cần dùng.
2. Bấm dấu **+** góc trên phải → **New repository**.
   - Repository name: `kaizencons-web`
   - Để ở chế độ **Public**, rồi bấm **Create repository**.
3. Ở trang repo vừa tạo, bấm dòng **uploading an existing file**.
4. Giải nén file `kaizencons-web.zip`, rồi kéo **toàn bộ nội dung bên trong** (file `index.html` và các thư mục `admin`, `data`, `images`) vào khung tải lên. Bấm **Commit changes**.
5. Sửa tên repo trong cấu hình: mở file `admin/config.yml` ngay trên GitHub → bấm biểu tượng **bút chì** → tìm dòng:

   ```
   repo: TEN-TAI-KHOAN-GITHUB/kaizencons-web
   ```

   Thay `TEN-TAI-KHOAN-GITHUB` bằng **username GitHub của bạn**. Ví dụ username là `ngoc-kaizen` thì sửa thành `repo: ngoc-kaizen/kaizencons-web`. Bấm **Commit changes**.

---

## PHẦN 2 — Đưa web lên Netlify

1. Vào https://app.netlify.com, bấm **Sign up with GitHub** để đăng ký bằng chính tài khoản GitHub.
2. Bấm **Add new project** → **Import an existing project** → **GitHub**. Cho phép Netlify truy cập, rồi chọn repo `kaizencons-web`.
3. Màn hình cấu hình deploy: **để trống hết** (Build command để trống, Publish directory để trống hoặc gõ `.`). Bấm **Deploy**.
4. Chờ khoảng 1 phút. Vào **Site configuration** → **Change site name**, đặt tên `kaizencons`. Web sẽ chạy tại:

   ```
   https://kaizencons.netlify.app
   ```

   Nếu tên `kaizencons` đã có người lấy, đặt tên khác (ví dụ `kaizencons-vn`), rồi sửa 2 dòng `site_url` và `display_url` trong `admin/config.yml` cho khớp địa chỉ mới.

Tới đây web đã xem được trên mạng, nhưng `/admin` chưa đăng nhập được. Làm tiếp Phần 3.

---

## PHẦN 3 — Bật đăng nhập trang quản trị (phần hay kẹt nhất)

Cần làm 2 việc: **(A)** tạo một "chìa khóa" trên GitHub, **(B)** đưa chìa khóa đó cho Netlify giữ.

### A. Tạo GitHub OAuth App

1. Trên GitHub, bấm **ảnh đại diện** góc trên phải → **Settings**.
2. Kéo xuống cuối menu trái, bấm **Developer settings**.
3. Bấm **OAuth Apps** → **New OAuth App**. Điền:
   - **Application name:** `KAIZENCONS Admin`
   - **Homepage URL:** `https://kaizencons.netlify.app`
   - **Authorization callback URL:** `https://api.netlify.com/auth/done`

   > Quan trọng: ô callback phải gõ **đúng y hệt** `https://api.netlify.com/auth/done` — đây là địa chỉ của Netlify, KHÔNG phải địa chỉ web của bạn. Gõ sai dòng này là nguyên nhân lỗi phổ biến nhất.
4. Bấm **Register application**.
5. Màn hình hiện ra:
   - Chép lại **Client ID**.
   - Bấm **Generate a new client secret**, rồi chép lại chuỗi **secret** (chỉ hiện một lần, nên chép ngay).

### B. Đưa chìa khóa cho Netlify

1. Về Netlify, mở site `kaizencons`, vào **Site configuration** → **Access & security** → mục **OAuth**.
2. Bấm **Install provider**.
3. Chọn **GitHub**, dán **Client ID** và **Client Secret** vừa chép ở bước A vào, rồi **Save** / **Install**.

Xong. Giờ vào `https://kaizencons.netlify.app/admin`, bấm **Login with GitHub**, cho phép, là vào được trang quản trị.

### Nếu bấm "Login with GitHub" mà báo lỗi
- **Hiện bảng xin quyền rồi quay lại trang trắng / báo lỗi:** kiểm tra lại ô **Authorization callback URL** ở bước A.3 có đúng `https://api.netlify.com/auth/done` không.
- **Báo "Error: Not Found" hoặc không thấy repo:** kiểm tra dòng `repo:` trong `admin/config.yml` đã đúng `username/kaizencons-web` chưa.
- **Cửa sổ pop-up không mở:** tắt trình chặn pop-up cho trang, rồi thử lại.

---

## Dùng trang quản trị
- **Dự án:** thêm tên, địa điểm, phạm vi công việc, ảnh. Tích **Hiện trên banner trang chủ** nếu muốn dự án chạy trên banner.
- **Khách hàng & Đối tác:** thêm tên công ty và logo (ảnh PNG nền trong suốt là đẹp nhất).
- **Tin tức**, **Thông tin liên hệ:** điền tương tự.
- Mỗi lần sửa xong bấm **Publish**. Web tự cập nhật sau khoảng 1 phút.

## Nhận email từ form Liên hệ
Trên Netlify vào **Forms** → **Enable form detection**, rồi vào **Deploys** → **Trigger deploy** một lần. Sau đó vào **Forms** → **Form notifications** → **Add notification** → **Email notification**, điền email nhận.

## Sau này: gắn tên miền riêng
Mua tên miền (ví dụ `kaizencons.com`), trên Netlify vào **Domain management** → **Add a domain** và làm theo hướng dẫn. Khi đổi tên miền, nhớ sửa lại `site_url` và `display_url` trong `admin/config.yml`.
