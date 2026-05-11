# Known Issues - Buổi 1

Ghi chú các lỗi hoặc vấn đề gặp phải trong quá trình setup môi trường.

## Vấn đề gặp phải

### 1. Docker daemon không khởi động
**Triệu chứng:** `docker: Cannot connect to the Docker daemon`

**Giải pháp:**
- Kiểm tra Docker Desktop đã chạy chưa (Windows/Mac)
- Trên Linux, chạy: `sudo systemctl start docker`
- Hoặc: `sudo service docker start`

### 2. Permission denied khi chạy docker
**Triệu chứng:** `Got permission denied while trying to connect to the Docker daemon socket`

**Giải pháp:**
- Thêm user vào group docker: `sudo usermod -aG docker $USER`
- Log out và log in lại, hoặc: `newgrp docker`

### 3. Docker Compose version không khớp
**Triệu chứng:** `docker-compose: command not found` hoặc phiên bản cũ

**Giải pháp:**
- Cập nhật Docker Desktop (nó bao gồm Docker Compose V2)
- Hoặc cài đặt riêng: `pip install docker-compose --upgrade`

### 4. Git chưa cấu hình
**Triệu chứng:** `fatal: not inside a git repository`

**Giải pháp:**
```bash
git config --global user.name "Họ Tên Sinh Viên"
git config --global user.email "email@example.com"
git init
```

### 5. Node.js hoặc Python chưa cài đặt
**Triệu chứng:** `node: command not found` hoặc `python: command not found`

**Giải pháp:**
- Cài đặt Node.js từ https://nodejs.org
- Cài đặt Python từ https://python.org hoặc qua package manager

---

## Tình trạng hiện tại

- [ ] Không có lỗi nào
- [ ] Có lỗi (ghi rõ ở trên)

**Ghi chú thêm:**

(Thêm chi tiết nếu cần)
