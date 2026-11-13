JOBCONNECT — HƯỚNG DẪN CÀI ĐẶT VÀ CHẠY WEBSITE
Áp dụng cho: JobConnect_Admin_Khoa_MoKhoa_Xoa_TaiKhoan_Python_MySQL.zip (bản tích hợp Frontend + Backend FastAPI + MySQL).

Website chạy cùng một địa chỉ http://127.0.0.1:8000/: Python FastAPI phục vụ giao diện HTML/CSS/JavaScript và API; MySQL lưu dữ liệu. Không cần PHP, XAMPP, Live Server, Docker hoặc chạy file .bat. Tuy nhiên, MySQL Server phải được cài và đang hoạt động.
1. Yêu cầu trước khi chạy
- Windows, VS Code, Python (kiểm tra bằng py --version).
- MySQL Server cài riêng, có thể dùng MySQL Workbench để quản lý database.
- Giải nén ZIP; cấu trúc thư mục chính:
JobConnect_Python_MySQL_Mot_Trang_Web/
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── app.js
│   ├── portal.js
│   ├── api-bridge.js
│   ├── theme.js
│   └── images/
└── Backend/
    ├── .env.example
    ├── main.py
    ├── database.py
    ├── models.py
    ├── schemas.py
    ├── config.py
    ├── init_db.py
    ├── seed_demo.py
    └── requirements.txt
2. Bật MySQL và tạo database (lần đầu)
Khởi động dịch vụ MySQL Server trong Windows Services (tên có thể là MySQL80, MySQL84 hoặc tên khác), hoặc dùng MySQL Workbench kiểm tra kết nối. Sau đó chạy câu lệnh SQL:
CREATE DATABASE IF NOT EXISTS job_portal
CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
Lưu ý: Database job_portal phải tồn tại trước khi chạy init_db.py. Không xóa database có dữ liệu thật.
3. Tạo và chỉnh file .env (lần đầu)
Mở VS Code tại thư mục dự án. Vào Terminal → New Terminal; nếu Terminal chưa ở Backend, chạy:
cd Backend
Sao chép mẫu cấu hình:
Copy-Item .env.example .env
Mở file Backend/.env (không chỉ sửa .env.example) và chỉnh theo tài khoản MySQL thực tế:
DATABASE_URL=mysql+pymysql://root:MAT_KHAU_MYSQL@127.0.0.1:3306/job_portal
JOB_SECRET_KEY=THAY_BANG_CHUOI_BI_MAT_NGAU_NHIEN_DAI
- Thay MAT_KHAU_MYSQL bằng mật khẩu thật. Nếu root không có mật khẩu, dùng mysql+pymysql://root@127.0.0.1:3306/job_portal.
- Nếu mật khẩu có ký tự đặc biệt (@, #, %, ...), cần URL-encode trong chuỗi kết nối.
- Giữ .env riêng tư, không upload lên GitHub. Không đăng ảnh hoặc dán mật khẩu lên mạng.
- Kiểm tra file .env ở cùng thư mục với database.py, không phải .env.txt.
Kiểm tra cấu hình đã được đọc (không in mật khẩu):
.\.venv\Scripts\python.exe -c "from dotenv import load_dotenv; import os; load_dotenv(); print('OK' if os.getenv('DATABASE_URL') else 'THIEU DATABASE_URL')"
Chỉ thực hiện lệnh kiểm tra trên sau khi đã tạo .venv ở bước 4.
4. Cài Python và khởi tạo dữ liệu (lần đầu)
Tại Terminal trong Backend:
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe init_db.py
init_db.py tạo các bảng còn thiếu, không tự nâng cấp các bảng cũ. Nếu database đã tồn tại từ bản khác, hãy sao lưu và so sánh schema trước khi thay đổi.

Nếu cần dữ liệu và tài khoản mẫu để kiểm thử trên database thử nghiệm, chạy:
.\.venv\Scripts\python.exe seed_demo.py
Không chạy dữ liệu mẫu trên database thật đang được sử dụng.
5. Khởi động cả Frontend và Backend bằng một lệnh
Mỗi lần mở website: bảo đảm MySQL Server đang chạy. Mở Terminal tại thư mục Backend và chạy:
.\.venv\Scripts\python.exe -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
Đợi Terminal hiện tương tự:
Uvicorn running on http://127.0.0.1:8000
Mở trình duyệt: http://127.0.0.1:8000/
- Website JobConnect: http://127.0.0.1:8000/
- Tài liệu API FastAPI: http://127.0.0.1:8000/docs
- Kiểm tra sức khỏe API: http://127.0.0.1:8000/health
FastAPI phục vụ luôn Frontend; không cần mở index.html bằng Live Server. Giữ Terminal đang chạy để tiếp tục sử dụng web. Nhấn Ctrl + C trong Terminal để dừng máy chủ.
6. Kiểm tra chức năng
1. Mở trang chủ, tìm kiếm việc làm.
2. Tự đăng ký tài khoản Ứng viên (mật khẩu ít nhất 8 ký tự).
3. Đăng nhập, lưu việc và nộp đơn ứng tuyển.
4. Đăng nhập tài khoản Nhà tuyển dụng mẫu (nếu đã seed) để đăng tin.
5. Đăng nhập Admin → Quản trị → Quản lý tài khoản để xem người dùng và thử Khóa/Mở khóa/Xóa trên tài khoản thử nghiệm; sao lưu DB trước khi thử Xóa.
6. Vào Kiểm duyệt tin để duyệt tin đang chờ.
Tài khoản demo (chỉ khi seed_demo.py tạo thành công trên database thử nghiệm):
Vai trò	Email	Mật khẩu
Admin	admin@demo.vn	123456
Nhà tuyển dụng	recruiter@demo.vn	123456


Tài khoản ứng viên tự đăng ký trên website.
7. Lỗi thường gặp
Thông báo / hiện tượng	Cách xử lý
Expected string or URL object, got None	Chưa có Backend/.env hoặc thiếu dòng DATABASE_URL; chỉ chỉnh .env.example là chưa đủ.
Access denied for user	Sai tài khoản hoặc mật khẩu MySQL trong .env.
Unknown database 'job_portal'	Chưa tạo database ở bước 2.
Can't connect to MySQL server	MySQL Server chưa chạy, sai host hoặc sai cổng.
ModuleNotFoundError	Cài lại thư viện bằng .\.venv\Scripts\python.exe -m pip install -r requirements.txt.
Address already in use	Cổng 8000 đang được chương trình khác sử dụng; dừng tiến trình cũ trước khi chạy lại.
Giao diện vẫn ghi DỮ LIỆU DEMO, không có nút Khóa/Xóa	Kiểm tra đã mở đúng bản ZIP mới ở cổng 8000, không phải thư mục Frontend cũ hoặc Live Server; thử Ctrl + Shift + R.
Khóa/Xóa không hoạt động	Kiểm tra đã đăng nhập Admin qua Backend và MySQL đang chạy; mở Developer Tools (F12) → Network để xem yêu cầu API bị lỗi.


8. Lưu ý
- Đây là bản tích hợp dành cho học tập và demo trên máy cá nhân. Chưa kiểm thử trực tiếp trên môi trường MySQL của mọi máy.
- Chức năng CV PDF và thư giới thiệu có thể vẫn lưu ở phía trình duyệt; không xem đó là chức năng upload file thật lên máy chủ.
- Chưa có chức năng Gmail xác minh hoặc quên mật khẩu qua email.
- Không đưa .env, mật khẩu, khóa bí mật, dữ liệu cá nhân hoặc database thật lên GitHub.