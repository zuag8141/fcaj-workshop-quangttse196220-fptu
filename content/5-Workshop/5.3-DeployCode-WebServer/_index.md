---
title : "Source Code Deployment and Web Server Configuration"
date : 2026-08-02
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

* Go to **EC2**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/12.png)

* Select **Instances**, then click **Connect**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/13.png)

* Verify the instance status shows **3/3 checks passed**, then choose **SSM Session Manager**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/14.png)

* Click **Connect**.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/15.png)

* After clicking **Connect**, you are redirected to the **Systems Manager** session.

![ec2](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.3-Deploy-Config/16.png)

## Deploy the Splitly Application on EC2

After accessing the EC2 instance via **EC2 → Connect → Session Manager → Connect**, switch to the `ec2-user` account:

```bash
sudo su - ec2-user
```

### 1. Create the Deployment Directory

```bash
sudo mkdir -p /opt/splitly
sudo chown -R ec2-user:ec2-user /opt/splitly
cd /opt/splitly
```

### 2. Clone the Source Code from GitHub

```bash
git clone <GITHUB_REPOSITORY_URL> .
```

Example:

```bash
git clone https://github.com/username/splitly.git .
```

The expected project structure is:

```text
/opt/splitly
├── app
└── backend
```

### 3. Deploy the Backend

```bash
cd /opt/splitly/backend
```

Create a `.env` file and fill in the required values:

```bash
nano .env
```

```env
PORT=5000

MONGODB_URI=<MONGODB_ATLAS_CONNECTION_STRING>
MONGODB_DB=Splitly

JWT_SECRET=<JWT_SECRET_KEY>

EMAIL_PROVIDER=gmail
GMAIL_SMTP_USER=<GMAIL_ADDRESS>
GMAIL_APP_PASSWORD=<GMAIL_APP_PASSWORD>
EMAIL_FROM=Splitly <<GMAIL_ADDRESS>>

AWS_REGION=ap-southeast-1
S3_RECEIPTS_BUCKET=<S3_BUCKET_NAME>
S3_RECEIPTS_PREFIX=receipts/
S3_PRESIGN_EXPIRES_SECONDS=3000

FRONTEND_URL=http://<EC2_PUBLIC_IP>

VNPAY_TMN_CODE=
VNPAY_HASH_SECRET=
VNPAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
```

Save and close with `Ctrl + O`, `Enter`, `Ctrl + X`.

> Do not commit the `.env` file or expose sensitive values (MongoDB URI, JWT secret, Gmail App Password, VNPay credentials).

Install dependencies and build the backend:

```bash
npm install
npm run build
```

Start the backend with PM2:

```bash
pm2 start dist/server.js --name splitly-api
pm2 save
pm2 status
```

The `splitly-api` process should be `online`.

### 4. Deploy the Frontend

```bash
cd /opt/splitly/app
```

Create a `.env.production` file:

```bash
nano .env.production
```

```env
VITE_API_URL=http://<EC2_PUBLIC_IP>
VITE_RECEIPTS_PUBLIC_BASE_URL=
VITE_GOOGLE_CLIENT_ID=<GOOGLE_CLIENT_ID>
```

Save and close with `Ctrl + O`, `Enter`, `Ctrl + X`, then install and build:

```bash
npm install
npm run build
```

Verify the frontend build:

```bash
test -f dist/index.html && echo "Frontend build: OK"
```

Expected result:

```text
Frontend build: OK
```

### 5. Configure and Run Nginx

Create the Nginx configuration for Splitly:

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

Save and close, then validate and restart Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl is-active nginx
```

Expected results:

```text
syntax is ok
test is successful
active
```

### 6. Verify the Complete Deployment

Check the backend process, port, frontend build, Nginx config, and the local website:

```bash
pm2 status
sudo ss -lntp | grep 5000
test -f /opt/splitly/app/dist/index.html && echo "Frontend build: OK"
sudo nginx -t
sudo systemctl is-active nginx
curl -I http://127.0.0.1
```

A successful response should contain:

```text
HTTP/1.1 200 OK
```

### 7. Access the Splitly Application

Open a web browser and access the app using the EC2 public IP:

```text
http://<EC2_PUBLIC_IP>
```

Example:

```text
http://13.xxx.xxx.xxx
```

If the deployment succeeds, the Splitly frontend is displayed, and requests beginning with `/api/` are forwarded by Nginx to the backend running on port `5000`.
