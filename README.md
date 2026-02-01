# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.homecookt.com

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

