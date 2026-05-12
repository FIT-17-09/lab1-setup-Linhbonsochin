# Service Boundary của nhóm

## 1. Thông tin nhóm

- Tên nhóm: Nhóm 4
- Lớp: CNTT 17-09
- Thành viên: Bảo Linh, Trung Sơn, Thế Anh, Kim Cường
- Service nhóm phụ trách: Core Business Service
- Sản phẩm tổng thể của lớp: Hệ thống quản lý nhân sự & chấm công

## 2. Actor

Ai tương tác với hệ thống/service?

Các actor tương tác với Core Business Service gồm:

- Quản trị viên hệ thống: quản lý nhân viên, phòng ban, ca làm, quy tắc nghiệp vụ.
- Nhân viên: xem thông tin cá nhân, lịch làm việc, kết quả chấm công.
- Trưởng phòng / quản lý: theo dõi nhân viên thuộc bộ phận, duyệt hoặc kiểm tra dữ liệu nghiệp vụ.
- Attendance Service: gửi dữ liệu chấm công sang Core Business Service để kiểm tra và xử lý.
- Access Gate Service: gửi sự kiện ra/vào để Core Business Service xác định tính hợp lệ.
- AI Vision Service: gửi kết quả nhận diện khuôn mặt hoặc định danh người dùng.
- Analytics Service: lấy dữ liệu nghiệp vụ đã xử lý để tổng hợp báo cáo.

## 3. System Boundary

Nhóm em xây phần nào?

Nhóm em xây dựng **Core Business Service**, là service xử lý nghiệp vụ trung tâm của hệ thống quản lý nhân sự và chấm công. Service này tiếp nhận dữ liệu từ các service khác, kiểm tra quy tắc nghiệp vụ, xử lý logic chính và cung cấp dữ liệu cho các service liên quan.

### Phần nhóm kiểm soát:

- Quản lý thông tin nhân viên cơ bản.
- Quản lý phòng ban, chức vụ, trạng thái nhân viên.
- Quản lý ca làm việc, lịch làm việc.
- Kiểm tra dữ liệu chấm công có hợp lệ hay không.
- Xử lý nghiệp vụ khi có sự kiện chấm công hoặc sự kiện ra/vào.
- Lưu trữ kết quả nghiệp vụ đã xử lý.
- Cung cấp API cho các service khác truy vấn dữ liệu.
- Cung cấp API kiểm tra trạng thái service `/health`.

### Phần nhóm chỉ tích hợp:

- Không trực tiếp nhận dữ liệu từ camera.
- Không xử lý nhận diện khuôn mặt.
- Không trực tiếp điều khiển cổng ra/vào.
- Không trực tiếp tạo báo cáo thống kê chuyên sâu.
- Không trực tiếp xử lý thiết bị IoT.
- Chỉ nhận dữ liệu đã được xử lý từ các service như Attendance Service, Access Gate Service, AI Vision Service.
- Chỉ gửi dữ liệu đã chuẩn hóa cho Analytics Service để phục vụ báo cáo.

## 4. Service Boundary

Service của nhóm có trách nhiệm gì?

Core Business Service có trách nhiệm:

- Đóng vai trò trung tâm xử lý logic nghiệp vụ của hệ thống.
- Quản lý dữ liệu nhân sự như nhân viên, phòng ban, chức vụ.
- Quản lý dữ liệu ca làm và lịch làm việc.
- Kiểm tra nhân viên có tồn tại trong hệ thống hay không.
- Kiểm tra nhân viên có thuộc đúng phòng ban, ca làm, lịch làm không.
- Xác định sự kiện chấm công là hợp lệ hay không hợp lệ.
- Ghi nhận kết quả xử lý nghiệp vụ vào database.
- Cung cấp dữ liệu cho Attendance Service, Access Gate Service và Analytics Service.
- Đảm bảo các service khác sử dụng chung một nguồn dữ liệu nghiệp vụ thống nhất.

Service KHÔNG làm gì?

- Không xử lý hình ảnh camera.
- Không nhận diện khuôn mặt bằng AI.
- Không mở hoặc đóng cổng vật lý.
- Không thu thập dữ liệu trực tiếp từ thiết bị IoT.
- Không tạo biểu đồ, dashboard phân tích chuyên sâu.
- Không gửi thông báo trực tiếp cho người dùng.
- Không thay thế vai trò của Attendance Service.
- Không lưu video, hình ảnh hoặc dữ liệu camera gốc.

## 5. Input / Output

### Input

Core Business Service nhận các dữ liệu đầu vào như:

- Thông tin nhân viên.
- Thông tin phòng ban.
- Thông tin chức vụ.
- Thông tin ca làm việc.
- Lịch làm việc của nhân viên.
- Dữ liệu chấm công từ Attendance Service.
- Sự kiện ra/vào từ Access Gate Service.
- Kết quả nhận diện người dùng từ AI Vision Service.
- Yêu cầu truy vấn dữ liệu từ Frontend hoặc các service khác.
## 6. API dự kiến

| Method | Endpoint                           | Mục đích                             |
| ------ | ---------------------------------- | ------------------------------------ |
| GET    | /health                            | Kiểm tra service có hoạt động không  |
| GET    | /employees                         | Lấy danh sách nhân viên              |
| GET    | /employees/{id}                    | Lấy thông tin chi tiết một nhân viên |
| POST   | /employees                         | Thêm mới nhân viên                   |
| PUT    | /employees/{id}                    | Cập nhật thông tin nhân viên         |
| DELETE | /employees/{id}                    | Xóa hoặc vô hiệu hóa nhân viên       |
| GET    | /departments                       | Lấy danh sách phòng ban              |
| POST   | /departments                       | Thêm mới phòng ban                   |
| GET    | /work-shifts                       | Lấy danh sách ca làm việc            |
| POST   | /work-shifts                       | Tạo ca làm việc                      |
| POST   | /business-events/check-attendance  | Kiểm tra và xử lý sự kiện chấm công  |
| POST   | /business-events/access-validation | Kiểm tra quyền ra/vào của nhân viên  |
| GET    | /business-events/{id}              | Lấy chi tiết một sự kiện nghiệp vụ   |
| GET    | /employees/{id}/schedule           | Lấy lịch làm việc của nhân viên      |


## 7. Phụ thuộc service khác

Service này gọi đến service nào?
Core Business Service có thể gọi đến:

Attendance Service: lấy hoặc gửi kết quả xử lý chấm công.
Access Gate Service: trả kết quả kiểm tra quyền ra/vào.
AI Vision Service: nhận kết quả định danh người dùng.
Analytics Service: gửi dữ liệu nghiệp vụ đã xử lý để phục vụ báo cáo.
Notification Service nếu có: gửi yêu cầu thông báo khi có sự kiện bất thường.
Database: lưu trữ dữ liệu nhân viên, phòng ban, ca làm và sự kiện nghiệp vụ.

Service nào gọi đến service này?

Các service có thể gọi đến Core Business Service gồm:

Attendance Service: gọi để kiểm tra dữ liệu chấm công.
Access Gate Service: gọi để kiểm tra quyền ra/vào.
AI Vision Service: gọi để xác thực người được nhận diện.
Analytics Service: gọi để lấy dữ liệu đã xử lý.
Frontend / Web Admin: gọi để quản lý nhân viên, phòng ban, ca làm.
Mobile App nếu có: gọi để hiển thị thông tin cá nhân, lịch làm, trạng thái chấm công.
## 8. Sơ đồ minh họa

<img width="2434" height="818" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/8a22aa40-73c7-492f-8d08-73c7f9e92519" />