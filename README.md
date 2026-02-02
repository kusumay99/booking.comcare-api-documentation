# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.nursingcarebooking.com

---

## 📬 Endpoints Overview

### 1. `POST /api/auth/register` — Register User

Register a profile with required name, email, password and confirmPassword.

Request Body

```json
{
    "name": "Nissy",
    "email": "kusumayekula0191@gmail.com",
    "password": "Kussu@123",
    "confirmPassword": "Kussu@123"
}

Success Response

{
    "message": "User registered successfully",
    "user": {
        "id": "697fdbd83d0fb874cd57a684",
        "name": "Nissy",
        "email": "kusumayekula0191@gmail.com"
    }
}
```

### 2. `POST /api/auth/login` — Login User

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
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI2OTdmZGJkODNkMGZiODc0Y2Q1N2E2ODQiLCJpYXQiOjE3Njk5ODcyODAsImV4cCI6MTc3MDA3MzY4MH0.5YzYpRO3AzpsQPgmZ9JpsAYf84swH0KqRQIbKKUPewA",
    "user": {
        "id": "697fdbd83d0fb874cd57a684",
        "name": "Nissy",
        "email": "kusumayekula0191@gmail.com"
    }
}
```

### 3. `POST /api/auth/forgot-password` — Forgot Password

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

### 4. `POST /api/auth/reset-password` — Reset Password

Recreating the password with OTP

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "OTP": "192418",
    "password": "Nissy@123",
    "confirmPassword": "Nissy@123"
}

Success Response

{
    "message": "Password reset successful"
}

```
