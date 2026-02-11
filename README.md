# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.nursingcarebooking.com

---

## 📬 Endpoints Overview

### 1. `POST /api/auth/register` — Register 

Register a profile with required name, email,role and password after that user will get the OTP. If they want to register as a user they need to give role as a user or if they want to be provider they need to give role as a provider.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kusuma@123",
    "name": "Kusuma Yekula",
    "role": "provider"
}

Success Response

{
    "success": true,
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
    "success": true,
    "message": "OTP resent successfully"
}

```
### 3. `POST /api/auth/verify-otp` — Verify OTP

Verify the OPT and later if it a user profile user profile will create if it is provider provider profile will create. 

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "otp": "919484"
}

Success Response

{
    "success": true,
    "message": "Provider account created successfully",
    "account": {
        "id": "698c31bc4465bcdf47d01157",
        "name": "Kusuma Yekula",
        "email": "kusumayekula0191@gmail.com",
        "providerId": 4283,
        "role": "provider"
    }
}

```

### 4. `POST /api/auth/login` — Login

Login profile with required email and password and we will get that profile as well.

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "password": "Kusuma@123"
}

Success Response

{
    "success": true,
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY5OGMzMWJjNDQ2NWJjZGY0N2QwMTE1NyIsImVtYWlsIjoia3VzdW1heWVrdWxhMDE5MUBnbWFpbC5jb20iLCJyb2xlIjoicHJvdmlkZXIiLCJwcm92aWRlcklkIjo0MjgzLCJpYXQiOjE3NzA3OTU1MTAsImV4cCI6MTc3MDg4MTkxMH0.d5BrDPG5UE7a55vuSFFfmyhw4UYoFSgxp8uHETAu-6w",
    "user": {
        "id": "698c31bc4465bcdf47d01157",
        "name": "Kusuma Yekula",
        "email": "kusumayekula0191@gmail.com",
        "providerId": 4283,
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
    "success": true,
    "message": "OTP sent to email"
}

```

### 6. `POST /api/auth/reset-password` — Reset Password

Recreating the password with OTP

Request Body

```json
{
    "email": "kusumayekula0191@gmail.com",
    "otp": "131911",
    "password": "Nissy@123"
}

Success Response

{
    "success": true,
    "message": "Password reset successful. You can login now."
}

```
### 7. ` /api/auth/delete` — Delete

If login profile is user profile we can delete profile by userId or it is provider we need to give providerId for deleting that login profile.

Request Body

```json
{
    "providerId": "3897"
}

Success Response

{
    "message": "User deleted successfully",
    "userId": "4283"
}

```

### 8. `POST /api/provider/create` — Creating provider profile

creating provider profile with provider details .

Request Body

```json
{
    "name": "Kusuma",
    "phone": "9542679596",
    "location": "Hyderabad, Madhapur",
    "providerId": 4283,
    "services": ["Home Nursing"],
    "serviceRadiusKm": "9",
    "typeOfCare": "Domiciliary Care",
    "credentials": ["Nursing Certificate"],
    "availability": {
        "monday": "9AM-6PM",
        "thursday": "9AM-6PM",
        "wednesday": "9AM-6PM",
        "tuesday": "9AM-6PM",
        "friday": "9AM-6PM",    
        "saturday": "9AM-6PM"
    },
    "pricing": {
        "daily": 2000,
        "hourly": 300,
        "monthly": 45000
    }
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
        "_id": "698c31bc4465bcdf47d01157",
        "providerId": 4283,
        "name": "Kusuma Yekula",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad, Madhapur",
        "phone": "",
        "password": "$2b$10$1eg.nYHNrEpeayLEwuHVGuUUlrObA8sm4yl.ofnvwMj8c420s5TEK",
        "services": [
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate"
        ],
        "isVerified": true,
        "avatar": "",
        "pictures": [],
        "createdAt": "2026-02-11T07:37:32.714Z",
        "updatedAt": "2026-02-11T07:53:14.677Z",
        "availability": {
            "monday": "9AM-6PM",
            "thursday": "9AM-6PM",
            "wednesday": "9AM-6PM",
            "tuesday": "9AM-6PM",
            "friday": "9AM-6PM",
            "saturday": "9AM-6PM"
        },
        "serviceRadiusKm": 9
    }
}
```
### 9. `post /api/provider/profile/providerId` — Getting provider profile by providerId

Getting provider profile by providerId. For getting profile we need to give access token as a authorization bearer. 

Request Body

```json
{
    "providerId": 4283
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
        "_id": "698c31bc4465bcdf47d01157",
        "providerId": 4283,
        "name": "Kusuma Yekula",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad, Madhapur",
        "phone": "",
        "services": [
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate"
        ],
        "isVerified": true,
        "avatar": "",
        "pictures": [],
        "createdAt": "2026-02-11T07:37:32.714Z",
        "updatedAt": "2026-02-11T07:53:14.677Z",
        "availability": {
            "monday": "9AM-6PM",
            "thursday": "9AM-6PM",
            "wednesday": "9AM-6PM",
            "tuesday": "9AM-6PM",
            "friday": "9AM-6PM",
            "saturday": "9AM-6PM"
        },
        "serviceRadiusKm": 9
    }
}
```
### 10. `PUT /api/provider/profile` — Updating provider profile 

Updating provider profile by UserId.

Request Body

```json
{
    "providerId": 4283,
    "name": "Nissy"
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
        "_id": "698c31bc4465bcdf47d01157",
        "providerId": 4283,
        "name": "Nissy",
        "email": "kusumayekula0191@gmail.com",
        "role": "provider",
        "typeOfCare": "Domiciliary Care",
        "location": "Hyderabad, Madhapur",
        "phone": "",
        "password": "$2b$10$1eg.nYHNrEpeayLEwuHVGuUUlrObA8sm4yl.ofnvwMj8c420s5TEK",
        "services": [
            "Home Nursing"
        ],
        "credentials": [
            "Nursing Certificate"
        ],
        "isVerified": true,
        "avatar": "",
        "pictures": [],
        "createdAt": "2026-02-11T07:37:32.714Z",
        "updatedAt": "2026-02-11T08:33:21.230Z",
        "availability": {
            "monday": "9AM-6PM",
            "thursday": "9AM-6PM",
            "wednesday": "9AM-6PM",
            "tuesday": "9AM-6PM",
            "friday": "9AM-6PM",
            "saturday": "9AM-6PM"
        },
        "serviceRadiusKm": 9
    }
}

```
### 11. `DELETE /api/auth/profile` — Delete Provider Profile

Deleting provider profile by UserId.

Request Body

```json
{
  "profileId": 4283
}

Success Response

{
    "success": true,
    "message": "Provider profile deleted successfully",
    "deletedUserId": 3897
}

```
