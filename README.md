# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.nursingcarebooking.com

---

## 📬 Endpoints Overview

### 1. `POST /api/auth/register` — Register User

Register a profile with required name, email and password after that user will get the OTP.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kussu@123",
    "name": "Kusuma"
}

Success Response

{
    "message": "OTP sent to email"
}

```
### 2. `POST /api/auth/resend-otp` — Resend OTP

Resend OTP with required name, email and password after that user will get the OTP.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kussu@123",
    "name": "Kusuma"
}

Success Response

{
    "message": "OTP resent to email"
}

```
### 3. `POST /api/auth/verify-otp` — Verify OTP

Verify the OPT. 

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "otp": "712294"
}

Success Response

{
    "message": "User registered successfully",
    "user": {
        "id": "6981a62454ab5d9fd4d57691",
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "userId": "257"
    }
}

```

### 4. `POST /api/auth/login` — Login User

Login profile with required email and password.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kussu@123"
}

Success Response

{
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI2OTgxYTYyNDU0YWI1ZDlmZDRkNTc2OTEiLCJ1c2VyRW1haWwiOiJrdXN1bWF5ZWt1bGEwMTkxQGdtYWlsLmNvbSIsImlhdCI6MTc3MDEwNDQ3MiwiZXhwIjoxNzcwMTkwODcyfQ.F7dUd1fW2DiVTaswU-M54SJJKlqN8gK5MSQt-ig5jv0",
    "user": {
        "id": "6981a62454ab5d9fd4d57691",
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "userId": "257"
    }
}
```

### 5. `POST /api/auth/forgot-password` — Forgot Password

Sending OTP if user forgot password.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com"
}

Success Response

{
    "message": "OTP sent to email"
}

```

### 6. `POST /api/auth/reset-password` — Reset Password

Recreating the password with OTP

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "OTP": "192418",
    "password": "Nissy@123"
}

Success Response

{
    "message": "Password reset successful"
}

```

### 7. `DELETE /api/auth/delete-user` — Delete User

Deleting profile by UserId.

Request Body

```json
{
    "userId": "257"
}

Success Response

{
    "message": "User deleted successfully",
    "userId": "257"
}

```
