---
title : "Deploy Source Code và Cấu hình Web Server"
date : 2026-08-02
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

* Vào **EC2**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/12.png)

* Vào **Instances**, rồi bấm **Connect**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/13.png)

* Kiểm tra trạng thái **3/3 checks passed**, rồi chọn **SSM Session Manager**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/14.png)

* Bấm **Connect**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/15.png)

* Sau khi bấm **Connect**, bạn được chuyển sang phiên **Systems Manager**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/16.png)

## Triển khai ứng dụng Splitly trên EC2

Sau khi truy cập EC2 qua **EC2 → Connect → Session Manager → Connect**, chuyển sang tài khoản `ec2-user`:

```bash
sudo su - ec2-user
```

### 1. Tạo thư mục triển khai

```bash
sudo mkdir -p /opt/splitly
sudo chown -R ec2-user:ec2-user /opt/splitly
cd /opt/splitly
```

### 2. Clone mã nguồn từ GitHub

```bash
git clone <URL_GITHUB_REPOSITORY> .
```

Ví dụ:

```bash
git clone https://github.com/username/splitly.git .
```

Cấu trúc thư mục dự kiến:

```text
/opt/splitly
├── app
└── backend
```

### 3. Triển khai Backend

```bash
cd /opt/splitly/backend
```

Tạo file `.env` và điền các giá trị cần thiết:

```bash
nano .env
```

```env
PORT=5000

MONGODB_URI=<CHUOI_KET_NOI_MONGODB_ATLAS>
MONGODB_DB=Splitly

JWT_SECRET=<CHUOI_BI_MAT_JWT>

EMAIL_PROVIDER=gmail
GMAIL_SMTP_USER=<DIA_CHI_EMAIL_GMAIL>
GMAIL_APP_PASSWORD=<MAT_KHAU_UNG_DUNG_GMAIL>
EMAIL_FROM=Splitly <<DIA_CHI_EMAIL_GMAIL>>

AWS_REGION=ap-southeast-1
S3_RECEIPTS_BUCKET=<TEN_BUCKET_S3>
S3_RECEIPTS_PREFIX=receipts/
S3_PRESIGN_EXPIRES_SECONDS=3000

FRONTEND_URL=http://<PUBLIC_IP_EC2>

VNPAY_TMN_CODE=
VNPAY_HASH_SECRET=
VNPAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
```

Lưu và đóng file bằng `Ctrl + O`, `Enter`, `Ctrl + X`.

> Không commit file `.env` hoặc công khai các giá trị bảo mật (MongoDB URI, JWT secret, Gmail App Password, thông tin VNPay).

Cài đặt dependencies và build backend:

```bash
npm install
npm run build
```

Khởi chạy backend bằng PM2:

```bash
pm2 start dist/server.js --name splitly-api
pm2 save
pm2 status
```

Tiến trình `splitly-api` cần có trạng thái `online`.

### 4. Triển khai Frontend

```bash
cd /opt/splitly/app
```

Tạo file `.env.production`:

```bash
nano .env.production
```

```env
VITE_API_URL=http://<PUBLIC_IP_EC2>
VITE_RECEIPTS_PUBLIC_BASE_URL=
VITE_GOOGLE_CLIENT_ID=<GOOGLE_CLIENT_ID>
```

Lưu và đóng file bằng `Ctrl + O`, `Enter`, `Ctrl + X`, rồi cài đặt và build:

```bash
npm install
npm run build
```

Kiểm tra kết quả build frontend:

```bash
test -f dist/index.html && echo "Frontend build: OK"
```

Kết quả mong đợi:

```text
Frontend build: OK
```

### 5. Cấu hình và chạy Nginx

Tạo file cấu hình Nginx cho Splitly:

```bash
sudo nano /etc/nginx/conf.d/splitly.conf
```

```nginx
server {
    listen 80;
    server_name _;

    root /opt/splitly/app/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Lưu và đóng file, rồi kiểm tra và khởi động lại Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl is-active nginx
```

Kết quả mong đợi:

```text
syntax is ok
test is successful
active
```

### 6. Kiểm tra toàn bộ quá trình triển khai

Kiểm tra tiến trình backend, cổng lắng nghe, build frontend, cấu hình Nginx và website cục bộ:

```bash
pm2 status
sudo ss -lntp | grep 5000
test -f /opt/splitly/app/dist/index.html && echo "Frontend build: OK"
sudo nginx -t
sudo systemctl is-active nginx
curl -I http://127.0.0.1
```

Kết quả thành công chứa:

```text
HTTP/1.1 200 OK
```

### 7. Truy cập ứng dụng Splitly

Mở trình duyệt và truy cập ứng dụng bằng Public IP của EC2:

```text
http://<PUBLIC_IP_EC2>
```

Ví dụ:

```text
http://13.xxx.xxx.xxx
```

Nếu triển khai thành công, giao diện frontend Splitly được hiển thị, và các yêu cầu bắt đầu bằng `/api/` được Nginx chuyển tiếp đến backend chạy trên cổng `5000`.
