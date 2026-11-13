JobConnect — Hướng dẫn cài đặt và chạy website
Dự án sử dụng HTML, CSS, JavaScript cho Frontend, Python FastAPI cho Backend và MySQL để lưu trữ dữ liệu.

Frontend và Backend hoạt động trên cùng một địa chỉ website:

http://127.0.0.1:8000/
Không cần PHP, XAMPP, Docker, Live Server hoặc file .bat. Tuy nhiên, bạn vẫn cần cài và khởi động MySQL Server riêng.

1. Yêu cầu cài đặt
Cần có:
- VS Code
- Python (kiểm tra bằng lệnh py --version)
- MySQL Server (có thể dùng MySQL Workbench để quản lý)
Cấu trúc thư mục
JobConnect_Python_MySQL_Mot_Trang_Web/
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── app.js
│   ├── portal.js
│   ├── api-bridge.js
│   ├── theme.js
│   └── images/
│
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
2. Khởi động MySQL và tạo database (chỉ làm lần đầu)
Bước 1. Bật dịch vụ MySQL Server trên Windows.

Tên dịch vụ có thể là MySQL80, MySQL84 hoặc tên khác theo bản cài đặt.

Bước 2. Mở MySQL Workbench (hoặc công cụ quản lý MySQL bạn đang sử dụng), rồi thực thi:
CREATE DATABASE IF NOT EXISTS job_portal
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
Nếu đã có database job_portal chứa dữ liệu, không xóa database đó.

3. Cấu hình kết nối MySQL (chỉ làm lần đầu)
Bước 1. Mở thư mục dự án trong VS Code.

Mở Terminal → New Terminal, sau đó chuyển tới thư mục Backend:
cd Backend
Bước 2. Sao chép file cấu hình mẫu thành .env:
Copy-Item .env.example .env
Bước 3. Mở file Backend/.env vừa tạo và chỉnh nội dung theo tài khoản MySQL trên máy của bạn:
DATABASE_URL=mysql+pymysql://root:MAT_KHAU_MYSQL@127.0.0.1:3306/job_portal
JOB_SECRET_KEY=THAY_BANG_CHUOI_NGAU_NHIEN_DAI_VA_BI_MAT
Trong đó:
- root: tên tài khoản MySQL (chỉ là ví dụ).
- MAT_KHAU_MYSQL: thay bằng mật khẩu MySQL của bạn.
- 127.0.0.1: máy chạy MySQL.
- 3306: cổng MySQL mặc định.
- job_portal: tên database.
Nếu tài khoản root không có mật khẩu, dùng dạng:
DATABASE_URL=mysql+pymysql://root@127.0.0.1:3306/job_portal
Nếu mật khẩu chứa ký tự đặc biệt như @, #, %, cần mã hóa URL trước khi đưa vào DATABASE_URL.

Không đẩy file .env lên GitHub. Đảm bảo tên là .env, không phải .env.txt hoặc chỉ chỉnh .env.example.

4. Cài thư viện Python và tạo bảng (chỉ làm lần đầu)
Mở Terminal trong thư mục Backend.
Bước 1. Tạo môi trường Python
py -m venv .venv
Bước 2. Cài thư viện dự án
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
Bước 3. Kiểm tra Python đọc được .env
.\.venv\Scripts\python.exe -c "from dotenv import load_dotenv; import os; load_dotenv(); print('OK' if os.getenv('DATABASE_URL') else 'THIEU DATABASE_URL')"
Nếu hiện OK, tiếp tục bước sau. Nếu hiện THIEU DATABASE_URL, kiểm tra lại mục 3.
Bước 4. Tạo bảng trong MySQL
.\.venv\Scripts\python.exe init_db.py
init_db.py tạo bảng còn thiếu nhưng không tự sửa cấu trúc các bảng cũ. Nếu database có sẵn từ bản khác, hãy sao lưu và kiểm tra cấu trúc trước.

Bước 5. Tạo dữ liệu và tài khoản demo (không bắt buộc)
Chỉ dùng trên database thử nghiệm:
.\.venv\Scripts\python.exe seed_demo.py
Không nên chạy dữ liệu mẫu trên database thật đang sử dụng.
5. Chạy website (mỗi lần sử dụng)
Bước 1. Đảm bảo MySQL Server đang chạy.

Bước 2. Mở VS Code và chuyển Terminal vào thư mục Backend nếu chưa ở đó:
cd Backend
Bước 3. Chạy FastAPI:
.\.venv\Scripts\python.exe -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
Khi chạy thành công, Terminal thường hiện:
Uvicorn running on http://127.0.0.1:8000
Bước 4. Mở trình duyệt tại:

http://127.0.0.1:8000/

Các địa chỉ quan trọng:
Địa chỉ	Chức năng
http://127.0.0.1:8000/	Website JobConnect
http://127.0.0.1:8000/docs	Tài liệu API FastAPI
http://127.0.0.1:8000/health	Kiểm tra trạng thái API (nếu bản Backend hỗ trợ)


Không cần mở index.html bằng Live Server. FastAPI đã phục vụ cả giao diện và API.

Giữ Terminal chạy trong lúc dùng website. Muốn dừng máy chủ, nhấn Ctrl + C ở Terminal.
6. Kiểm tra chức năng
Sau khi vào website:
1. Mở trang chủ và tìm việc làm.
2. Đăng ký tài khoản Ứng viên (mật khẩu tối thiểu 8 ký tự).
3. Đăng nhập, lưu việc và nộp đơn ứng tuyển.
4. Đăng nhập Nhà tuyển dụng để đăng tin và theo dõi ứng viên.
5. Đăng nhập Admin, mở Quản trị → Quản lý tài khoản để xem danh sách, Khóa/Mở khóa hoặc Xóa tài khoản thử nghiệm.
6. Vào Kiểm duyệt tin để duyệt tin tuyển dụng đang chờ.
Tài khoản demo
Chỉ có nếu đã chạy seed_demo.py thành công:
Vai trò	Email	Mật khẩu
Admin	admin@demo.vn	123456
Nhà tuyển dụng	recruiter@demo.vn	123456


Ứng viên có thể tự đăng ký.
Trước khi thử Xóa tài khoản, hãy sao lưu database. Không dùng tài khoản hoặc dữ liệu cá nhân thật để thử nghiệm.

7. Cách xử lý lỗi thường gặp
Lỗi Expected string or URL object, got None
Python chưa đọc được DATABASE_URL. Kiểm tra file Backend/.env đã tồn tại và có dòng DATABASE_URL=... đúng chưa.
Lỗi Access denied for user
Sai tài khoản hoặc mật khẩu MySQL. Kiểm tra DATABASE_URL trong .env.
Lỗi Unknown database 'job_portal'
Chưa tạo database. Thực hiện lại mục 2.
Lỗi Can't connect to MySQL server
MySQL Server chưa bật, hoặc host/cổng trong .env không đúng.
Lỗi ModuleNotFoundError
Chạy lại lệnh cài thư viện:
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
Lỗi Address already in use
Cổng 8000 đang được tiến trình khác sử dụng. Dừng máy chủ cũ rồi chạy lại.
Trang vẫn ghi DỮ LIỆU DEMO hoặc chưa có nút Khóa/Xóa
Kiểm tra đã mở đúng thư mục dự án mới và truy cập địa chỉ http://127.0.0.1:8000/, không phải Live Server hay bản Frontend cũ. Nhấn Ctrl + Shift + R để làm mới trang.
Nút Khóa/Xóa không hoạt động
Kiểm tra tài khoản đã đăng nhập có quyền Admin, MySQL đang chạy và các API tương ứng có mặt tại /docs.
8. Lưu ý
- Đây là dự án phục vụ học tập và kiểm thử cục bộ.
- CV PDF có thể vẫn được lưu trong trình duyệt, chưa phải tải lên Backend.
- Chưa có chức năng xác minh Gmail và đặt lại mật khẩu qua email.
- Không đưa .env, mật khẩu, khóa bí mật hay dữ liệu người dùng thật lên GitHub.
