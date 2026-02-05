# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.nursingcarebooking.com

---

## 📬 Endpoints Overview

### 1. `POST /api/auth/register` — Register User

Register a profile with required name, email,role and password after that user will get the OTP.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kusuma@123",
    "name": "Kusuma",
    "role": "provider"
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
    "email": "kusumayekula0191@gmail.com"
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
    "otp": "919484"
}

Success Response

{
    "message": "Account created successfully",
    "account": {
        "id": "698422fd71003a7641cc84af",
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "userId": 3897,
        "role": "provider"
    }
}

```

### 4. `POST /api/auth/login` — Login User

Login profile with required email and password.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kusuma@123"
}

Success Response

{
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY5ODQyMmZkNzEwMDNhNzY0MWNjODRhZiIsImVtYWlsIjoia3VzdW1heWVrdWxhMDE5MUBnbWFpbC5jb20iLCJyb2xlIjoicHJvdmlkZXIiLCJpYXQiOjE3NzAyNjc0ODgsImV4cCI6MTc3MDM1Mzg4OH0.QHOjv1VXya_sL18XRx3XxnWm_MLxlO3eOWPke-2A6vw",
    "user": {
        "id": "698422fd71003a7641cc84af",
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "userId": 3897,
        "role": "provider"
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
    "otp": "192418",
    "password": "Nissy@123"
}

Success Response

{
    "message": "Password reset successful"
}

```
### 7. ` /api/auth/delete` — Delete User

Deleting profile by UserId.

Request Body

```json
{
    "userId": "3897"
}

Success Response

{
    "message": "User deleted successfully",
    "userId": "3897"
}

```

### 8. `POST /api/provider/create` — Creating provider profile

creating provider profile with provider details .

Request Body

```json
{
  "userId": 3592,
  "typeOfCare": "Domiciliary Care",
  "location": "Hyderabad",
  "serviceRadiusKm": 10,
  "services": ["Elder Care", "Home Nursing"],
  "pricing": {
    "hourly": 300,
    "daily": 2000,
    "monthly": 45000
  },
  "availability": {
    "monday": "9AM-6PM",
    "tuesday": "9AM-6PM"
  },
  "credentials": ["Nursing Certificate", "CPR Certified"]
}


Success Response

{
    "success": true,
    "message": "Provider profile created successfully",
    "provider": {
        "pricing": {
            "hourly": 300,
            "daily": 2000,
            "monthly": 45000
        },
        "_id": "698422fd71003a7641cc84af",
        "userId": 3897,
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad",
        "phone": "",
        "avatar": "",
        "gallery": [],
        "password": "$2b$10$6j9LsZtino41sI7NV.Ajl.eJL6UAH0PWrD6H7WweuGwOalFbuZSg6",
        "services": [
            "Elder Care",
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate",
            "CPR Certified"
        ],
        "isVerified": true,
        "createdAt": "2026-02-05T04:56:29.874Z",
        "updatedAt": "2026-02-05T05:03:07.106Z",
        "availability": {
            "monday": "9AM-6PM",
            "tuesday": "9AM-6PM"
        },
        "serviceRadiusKm": 7
    }
}

```
### 9. `post /api/provider/profile/userid` — Getting provider profile

Getting provider profile by userId.

Request Body

```json
{
    "userId": "3897"
}

Success Response

{
    "success": true,
    "provider": {
        "pricing": {
            "hourly": 300,
            "daily": 2000,
            "monthly": 45000
        },
        "_id": "698422fd71003a7641cc84af",
        "userId": 3897,
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad",
        "phone": "",
        "avatar": "",
        "gallery": [],
        "services": [
            "Elder Care",
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate",
            "CPR Certified"
        ],
        "isVerified": true,
        "createdAt": "2026-02-05T04:56:29.874Z",
        "updatedAt": "2026-02-05T05:03:07.106Z",
        "availability": {
            "monday": "9AM-6PM",
            "tuesday": "9AM-6PM"
        },
        "serviceRadiusKm": 7
    }
}

```
### 10. `PUT /api/provider/profile` — Updating provider profile 

Updating provider profile by UserId.

Request Body

```json
{
  "userId": 3897,
  "typeOfCare": "Domiciliary Care",
  "location": "Hyderabad, Madhapur",
  "serviceRadiusKm": 8,
  "services": ["Elder Care", "Home Nursing"],
  "pricing": {
    "hourly": 300,
    "daily": 2000,
    "monthly": 45000
  },
  "availability": {
    "monday": "9AM-6PM",
    "tuesday": "9AM-6PM"
  },
  "credentials": ["Nursing Certificate", "CPR Certified"]
}


Success Response

{
    "success": true,
    "message": "Provider profile updated successfully",
    "provider": {
        "pricing": {
            "hourly": 300,
            "daily": 2000,
            "monthly": 45000
        },
        "_id": "698422fd71003a7641cc84af",
        "userId": 3897,
        "name": "Kusuma",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad, Madhapur",
        "phone": "",
        "avatar": "",
        "gallery": [],
        "password": "$2b$10$6j9LsZtino41sI7NV.Ajl.eJL6UAH0PWrD6H7WweuGwOalFbuZSg6",
        "services": [
            "Elder Care",
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate",
            "CPR Certified"
        ],
        "isVerified": true,
        "createdAt": "2026-02-05T04:56:29.874Z",
        "updatedAt": "2026-02-05T05:09:06.306Z",
        "availability": {
            "monday": "9AM-6PM",
            "tuesday": "9AM-6PM"
        },
        "serviceRadiusKm": 8
    }
}

```
### 11. ` /api/auth/delete` — Delete Provider Profile

Deleting provider profile by UserId.

Request Body

```json
{
  "userId": 3897
}

Success Response

{
    "success": true,
    "message": "Provider profile deleted successfully",
    "deletedUserId": 3897
}

```
