#  Developer Guide – Cấu trúc module dự án Django

##  Cấu trúc thư mục chuẩn

```
repo_root/
│── src/
│   ├── config/         # Module cấu hình chính của Django (settings, urls, wsgi, asgi)
│   ├── apps/           # Nơi chứa toàn bộ module ứng dụng (mỗi app là 1 module riêng)
│   │   ├── users/      # Ví dụ module: quản lý người dùng
│   │   ├── phishing/   # Ví dụ module: phát hiện phishing
│   │   └── __init__.py
│   ├── templates/      # Template HTML toàn cục (chung cho nhiều module)
│   ├── static/         # Static files toàn cục (CSS, JS, image dùng chung)
│   └── manage.py
│
├── requirements.txt    # Danh sách thư viện Python
├── README.md
└── .gitignore
```

---

##  Bản chất của một **app (module)** trong Django

* **App là một thành phần chức năng độc lập** trong hệ thống. Nó giống như một "plugin" có thể tái sử dụng được.
* Mỗi app thường quản lý **một lĩnh vực nghiệp vụ** (business domain) cụ thể, ví dụ:

  * `users/` → quản lý người dùng, xác thực, phân quyền.
  * `phishing/` → phát hiện và xử lý email phishing.
  * `payments/` → thanh toán, hóa đơn.
* Một project Django thường gồm **nhiều app kết hợp lại**, nhưng tất cả dùng chung **một database** (mỗi app đóng góp model vào database này).

---

##  Quy tắc làm việc với `apps/`

### 1. Thêm 1 module mới

* Tạo module bằng lệnh:

  ```bash
  python src/manage.py startapp <tên_module> src/apps/<tên_module>
  ```
* Cấu trúc 1 module chuẩn:

  ```
  apps/<tên_module>/
  ├── models.py      # Định nghĩa bảng dữ liệu (database models)
  ├── views.py       # Xử lý logic request/response
  ├── urls.py        # Route của module
  ├── templates/     # Template riêng cho module (apps/<tên_module>/templates/<tên_module>/)
  ├── static/        # Static files riêng cho module (apps/<tên_module>/static/<tên_module>/)
  └── tests.py       # Unit test
  ```
* Đăng ký module mới trong `src/config/settings.py` → `INSTALLED_APPS`.

---

### 2. Chỉnh sửa 1 module

* Sửa code trong `models.py`, `views.py`, `urls.py`, template hoặc static.
* Nếu có thay đổi model → chạy:

  ```bash
  python src/manage.py makemigrations <tên_module>
  python src/manage.py migrate
  ```

---

### 3. Xóa 1 module

* Gỡ module khỏi `INSTALLED_APPS` trong `settings.py`.
* Xóa thư mục `src/apps/<tên_module>/`.
* Nếu muốn giữ sạch CSDL → chạy:

  ```bash
  python src/manage.py migrate <tên_module> zero
  ```

  trước khi xóa.

---

##  Templates & Static

* **Dùng riêng cho module** → đặt trong `apps/<tên_module>/templates/<tên_module>/` và `apps/<tên_module>/static/<tên_module>/`.
* **Dùng chung toàn dự án** → đặt trong `src/templates/` và `src/static/`.

---

##  Lợi ích mô hình này

* `apps/` rõ ràng → dễ thêm/sửa/xóa module mà không ảnh hưởng các module khác.
* Mỗi module đóng vai trò như một "khối lego" → dễ mở rộng, tái sử dụng.
* Phân tách `templates/` và `static/` **chung** vs **riêng** → rõ ràng, gọn gàng.
* Cấu trúc chuẩn giúp team dễ cộng tác và bảo trì lâu dài.

---

 Hãy tuân thủ đúng mô hình này để giữ dự án luôn **sạch, rõ ràng và dễ mở rộng**.
