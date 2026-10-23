# GAMEVAULT Secure

Bản tách frontend/backend cho GAMEVAULT, dùng Flask + SQLite.

## Có gì đã thay đổi
- Không còn `admin/admin123` hay `KIEN2026` trong frontend.
- Mật khẩu user được hash bằng Werkzeug.
- Mật khẩu game được mã hóa bằng Fernet, khóa nằm trong `.env`.
- Phân quyền server-side: `admin`, `leader`, `user`.
- Leader xem được mật khẩu game nhưng không sửa/xóa, không xem email, không quản lý thành viên.
- Admin quản lý tài khoản game, email liên kết, thành viên, Leader, yêu cầu mật khẩu, chat và đổi mật khẩu Admin.
- Username có cooldown 14 ngày ở server.
- Session đăng nhập dùng cookie HttpOnly.

## Chạy
1. Tạo môi trường ảo và cài `requirements.txt`.
2. Sao chép `.env.example` thành `.env` và điền secret thật.
3. Chạy `python backend/server.py`.
4. Mở `http://127.0.0.1:5000`.

Không commit `.env` hoặc `*.db` lên GitHub.

## Lưu ý triển khai thật
Đặt `COOKIE_SECURE=1` khi chạy HTTPS, dùng secret mạnh, bật HTTPS, rate limiting và CSRF protection phù hợp với môi trường production. Có thể chuyển SQLite sang PostgreSQL khi triển khai nhiều người dùng.


## Triển khai trên Render
- Đẩy toàn bộ thư mục này lên GitHub, không đẩy `.env` hoặc database.
- Trên Render chọn **New → Blueprint** để dùng `render.yaml`, hoặc tạo Web Service từ repository.
- Khai báo các biến bí mật được yêu cầu: `ADMIN_USERNAME`, `ADMIN_PASSWORD`, `ADMIN_REVEAL_CODE`, `SESSION_SECRET`, `GAME_PASSWORD_KEY`.
- `render.yaml` gắn Persistent Disk để giữ SQLite qua lần deploy; đừng xóa disk nếu cần giữ dữ liệu.
- Khi chạy HTTPS, `COOKIE_SECURE=1` được bật trong cấu hình Render.
