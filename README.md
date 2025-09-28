# 🔑 Token Authentication (JWT)

## 📌 Giới thiệu
Dự án này minh họa cách xây dựng hệ thống **Authentication** bằng **JWT (JSON Web Token)** trong Node.js + Express.  
Người dùng có thể đăng ký, đăng nhập và nhận token để truy cập các tài nguyên (resources) bảo vệ.  

---

## ⚙️ Cài đặt & Chạy thử

### 1️⃣ Clone repo
```bash
git clone https://github.com/HoaiLoc9/token_auth.git
cd token_auth
``` 
2️⃣ Cài đặt dependencies
npm install

3️⃣ Chạy server
node app.js

### 🔑 Chức năng & Cách test với Postman
## 1. Register
Endpoint: POST http://localhost:3000/register
Body (JSON):
```bash
{
  "username": "admin",
  "password": "12345"
}
```
Kết quả: Lưu user mới vào MongoDB.
![register](https://github.com/HoaiLoc9/token_auth/blob/main/public/results/register5.png?raw=true)
![register_db](https://github.com/HoaiLoc9/token_auth/blob/main/public/results/register5_checkdb.png?raw=true)

### 2. Login
Endpoint: POST http://localhost:3000/login

Body (JSON):
```bash
{
  "username": "admin",
  "password": "12345"
}
```
Kết quả: Trả về JWT token:

![login](https://github.com/HoaiLoc9/token_auth/blob/main/public/results/login5.png?raw=true)


Copy token này để sử dụng ở các request khác.

3. Profile (with token)
Endpoint: GET http://localhost:3000/profile

Headers:

Authorization: Bearer <token>
Kết quả: Trả về thông tin user nếu token hợp lệ.
![profile](https://github.com/HoaiLoc9/token_auth/blob/main/public/results/profile5_token.png?raw=true)

### 🔎 Cách sửa code để JWT có thời gian hết hạn

Trong code login hiện tại có:
```bash
const token = jwt.sign({ id: user._id }, 'secretKey');
```

👉 Để token hết hạn, chỉ cần thêm option expiresIn vào jwt.sign:
```bash
// Token hết hạn sau 1 giờ
const token = jwt.sign(
  { id: user._id },
  'secretKey',
  { expiresIn: '1h' }
);
```
⏱ Ví dụ thời gian hết hạn

expiresIn: "60s" → hết hạn sau 60 giây

expiresIn: "15m" → hết hạn sau 15 phút

expiresIn: "1h" → hết hạn sau 1 giờ

expiresIn: "7d" → hết hạn sau 7 ngày

🔎 Khi token hết hạn

Nếu gọi API /profile sau khi token đã hết hạn → middleware authMiddleware sẽ báo lỗi:
```bash
{
  "error": "jwt expired"
}
```

✅ Như vậy, chỉ cần thêm { expiresIn: "1h" } (hoặc thời gian bạn muốn) vào lệnh jwt.sign() là token sẽ có thời gian sống giới hạn.
### 📂 Cấu trúc dự án
```bash
token_auth/
│── app.js              
│── models/User.js      
│── routes/auth.js      
│── package.json
│── README.md
```
