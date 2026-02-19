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

# Provider Endpoints

---

### 20. `POST /api/provider/create` — Create Provider Profile

Create or complete provider profile with details, images and geo location.  
This endpoint accepts **multipart/form-data**.  
Authorization: Provider Access Token (Bearer)

#### Request Body (form-data)

```
providerId: 4283
name: John Care
phone: 9876543210
typeOfCare: Domiciliary Care
location: Bangalore
serviceRadiusKm: 10
services: ["Bathing","Feeding"]
credentials: ["CPR","Nursing"]
availability: {"monday":"9AM-6PM","tuesday":"9AM-6PM"}
pricing: {"hourly":300,"daily":2000}
```

#### Images (form-data)

```
avatar → single image  
pictures → up to 5 images
```

⭐ If latitude & longitude are not provided → system auto-detects from address/postalCode/city.

#### Success Response

```json
{
  "success": true,
  "message": "Provider profile created successfully",
  "provider": {}
}
```

---

### 21. `GET /api/provider/providers` — Get All Providers

Fetch providers with pagination and search.

#### Query Params

```
?page=1
&limit=10
&typeOfCare=Dementia Care
&search=john
```

#### Success Response

```json
{
  "success": true,
  "totalProviders": 25,
  "currentPage": 1,
  "totalPages": 3,
  "providers": []
}
```

---

### 22. `POST /api/provider/profile/providerid` — Get Provider Profile

Authorization: Bearer Token

#### Request Body

```json
{
  "providerId": 4283
}
```

#### Success Response

```json
{
  "success": true,
  "provider": {}
}
```

---

### 23. `PUT /api/provider/profile` — Update Provider Profile

Update provider profile (partial update supported).  
Accepts multipart/form-data.  
Authorization: Bearer Token

#### Request Body

```
providerId: 4283
name: Updated Name
serviceRadiusKm: 15
```

✔ Replace images automatically  
✔ Auto geocode if address updated  

#### Success Response

```json
{
  "success": true,
  "message": "Provider profile updated successfully",
  "provider": {}
}
```

---

### 24. `DELETE /api/provider/profile` — Delete Provider Profile

Deletes provider profile and all S3 images.  
Authorization: Bearer Token

#### Request Body

```json
{
  "providerId": 4283
}
```

#### Success Response

```json
{
  "success": true,
  "message": "Provider profile along with images deleted successfully"
}
```

---

### 25. `POST /api/provider/search` — Geo Search Providers

Find nearby providers using GPS or Address.

#### Option 1 — Using GPS

```json
{
  "latitude": 12.9716,
  "longitude": 77.5946
}
```

#### Option 2 — Using Address

```json
{
  "postalCode": "560001",
  "city": "Bangalore"
}
```

#### Optional Filters

```json
{
  "radiusMiles": 10,
  "typeOfCare": "Dementia Care"
}
```

Default radius = **20 miles**

---

### Smart Ranking Logic

Providers sorted by score:

| Condition | Score |
|---|---|
| Matching care type | +5 |
| Distance ≤ 5 miles | +4 |
| Distance ≤ 10 miles | +3 |
| Distance ≤ 20 miles | +2 |

Final sorting:

```
rankScore DESC → distance ASC
```

---

#### Success Response

```json
{
  "success": true,
  "searchCenter": {
    "latitude": 12.97,
    "longitude": 77.59
  },
  "radiusMiles": 10,
  "totalFound": 5,
  "providers": [
    {
      "name": "John Care",
      "distanceMiles": 2.3,
      "rankScore": 9
    }
  ]
}
```

---

### MongoDB Setup (IMPORTANT)

Run once:

```js
db.providersTree.createIndex({ locationGeo: "2dsphere" })
```

---

### Error Response Format

```json
{
  "success": false,
  "message": "Error message",
  "error": "optional_debug_error"
}
```

# Booking Endpoints

### 12. `POST /api/booking/create` — Create Booking

create booking by providerId. To this we need to give user Access Token as a authorization Bearer.

Request Body

```json
{
    "providerId": 7751,
    "appointmentDate": "2026-02-20T10:30:00.000Z",
    "serviceType": "Domiciliary Care",
    "notes": "Patient needs daily support"
}


Success Response

{
    "success": true,
    "message": "Booking created successfully",
    "booking": {
        "bookingId": 8,
        "userId": 329,
        "providerId": 7751,
        "serviceType": "Domiciliary Care",
        "appointmentDate": "2026-02-20T10:30:00.000Z",
        "status": "Pending",
        "patientSnapshot": {
            "recipientName": "Sujatha",
            "recipientAge": 70,
            "recipientGender": "",
            "mobilityStatus": "Walks with support",
            "medicalConditions": [],
            "equipmentDetails": "",
            "address": "221B Baker Street",
            "city": "London",
            "emergencyContactName": "",
            "emergencyContactPhone": "1234567890"
        },
        "notes": "Patient needs daily support",
        "rejectionReason": "",
        "_id": "6995700377ee79dc9b147e5b",
        "createdAt": "2026-02-18T07:53:39.852Z",
        "updatedAt": "2026-02-18T07:53:39.852Z",
        "__v": 0
    }
}
```


### 13. `PUT /api/booking/accept` — Accepting Booking

After user booking service user if care provider wants to accept. To accept booking also we need provider access token as a authorization bearer.

Request Body

```json
{
    "bookingId": 4
}

Success Response

{
    "success": true,
    "message": "Booking accepted successfully",
    "booking": {
        "_id": "698c4acae37c0391a322197e",
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "serviceType": "Dementia Care",
        "appointmentDate": "2026-02-15T10:00:00.000Z",
        "status": "Accepted",
        "notes": "Need care for 2 hours",
        "rejectionReason": "",
        "createdAt": "2026-02-11T09:24:26.816Z",
        "updatedAt": "2026-02-11T09:38:48.748Z",
        "__v": 0
    }
}

```

### 14. `PUT /api/booking/reject` — Rejecting booking

If anything wrong provider can reject booking along with reason. For this we need user Access Token as a authorization Brarer.

Request Body

```json
{
  "bookingId": 3,
  "reason": "Client cancelled the booking."
}


Success Response

{
    "success": true,
    "message": "Booking rejected successfully",
    "booking": {
        "_id": "698c4acae37c0391a322197e",
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "serviceType": "Dementia Care",
        "appointmentDate": "2026-02-15T10:00:00.000Z",
        "status": "Rejected",
        "notes": "Need care for 2 hours",
        "rejectionReason": "Client cancelled the booking.",
        "createdAt": "2026-02-11T09:24:26.816Z",
        "updatedAt": "2026-02-11T09:38:48.748Z",
        "__v": 0
    }
}

```

### 15. `PUT /api/booking/cancel` — User cancel booking

If user booked services if they want cancel again they can use this 

Request Body

```json

{
  "bookingId": 5
}


Success Response

{
    "success": true,
    "message": "Booking cancelled successfully",
    "booking": {
        "_id": "698ccf75e37c0391a32219f1",
        "bookingId": 5,
        "userId": 888,
        "providerId": 4283,
        "serviceType": "Dementia Care",
        "appointmentDate": "2026-02-15T10:00:00.000Z",
        "status": "Cancelled",
        "notes": "",
        "rejectionReason": "",
        "createdAt": "2026-02-11T18:50:29.314Z",
        "updatedAt": "2026-02-11T18:55:48.177Z",
        "__v": 0
    }
}

```

### 16. `PUT /api/booking/complete` — Provider Completed Booking

If user booked services if they want cancel again they can use this 

Request Body

```json

{
  "bookingId": 4
}


Success Response

{
    "success": true,
    "message": "Booking marked as completed",
    "booking": {
        "_id": "698c4acae37c0391a322197e",
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "serviceType": "Dementia Care",
        "appointmentDate": "2026-02-15T10:00:00.000Z",
        "status": "Completed",
        "notes": "Need care for 2 hours",
        "rejectionReason": "",
        "createdAt": "2026-02-11T09:24:26.816Z",
        "updatedAt": "2026-02-11T19:14:09.575Z",
        "__v": 0
    }
}

```

### 17. `POST /api/booking/details` — getting details by bookingId

If user booked services if they want cancel again they can use this 

Request Body

```json

{
  "bookingId": 5
}


Success Response

{
    "success": true,
    "booking": {
        "_id": "698ccf75e37c0391a32219f1",
        "bookingId": 5,
        "userId": 888,
        "providerId": 4283,
        "serviceType": "Dementia Care",
        "appointmentDate": "2026-02-15T10:00:00.000Z",
        "status": "Cancelled",
        "notes": "",
        "rejectionReason": "",
        "createdAt": "2026-02-11T18:50:29.314Z",
        "updatedAt": "2026-02-11T18:55:48.177Z",
        "__v": 0
    }
}

```

# Review Endpoints

### 17. `POST /api/review/add` — Create Review by Booking Id

This is for giiving rating along with comment for the care provider. Service User access token as a Authorization Token. 

Request Body

```json

{
  "bookingId": 4,
  "rating": 5,
  "comment": "Excellent service and very professional."
}



Success Response

{
    "success": true,
    "message": "Review submitted successfully",
    "review": {
        "reviewId": 2,
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "rating": 5,
        "comment": "Excellent service and very professional.",
        "isFlagged": false,
        "flagReason": "",
        "isHidden": false,
        "_id": "698cdd8fe37c0391a3221a02",
        "createdAt": "2026-02-11T19:50:39.846Z",
        "updatedAt": "2026-02-11T19:50:39.846Z",
        "__v": 0
    }
}

```

### 18. `POST /api/review/provider` — Provider can see theire reviews

This is for getting review along with comment for the care provider.

Request Body

```json


Success Response

{
    "success": true,
    "count": 1,
    "reviews": [
        {
            "_id": "698cdd8fe37c0391a3221a02",
            "reviewId": 2,
            "bookingId": 4,
            "userId": 888,
            "providerId": 4283,
            "rating": 5,
            "comment": "Excellent service and very professional.",
            "isFlagged": false,
            "flagReason": "",
            "isHidden": false,
            "createdAt": "2026-02-11T19:50:39.846Z",
            "updatedAt": "2026-02-11T19:50:39.846Z",
            "__v": 0
        }
    ]
}

```

### 19. `PUT /api/review/flag` — user can flag the review 

If anything wrong with provider service user can flag the service. Provider Access token as a bearer token. 

Request Body

```json

{
  "reviewId": 2,
  "reason": "This review contains false information"
}

Success Response

{
    "success": true,
    "message": "Review flagged successfully",
    "review": {
        "_id": "698cdd8fe37c0391a3221a02",
        "reviewId": 2,
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "rating": 5,
        "comment": "Excellent service and very professional.",
        "isFlagged": true,
        "flagReason": "This review contains false information",
        "isHidden": false,
        "createdAt": "2026-02-11T19:50:39.846Z",
        "updatedAt": "2026-02-11T20:07:57.256Z",
        "__v": 0
    }
}

```

### 20. `POST /api/user/profile` — user can create their profile 

This is for user can create their profile. User Access token as a bearer token. 

Request Body

```json

KEY	TYPE	VALUE
photo	File	(choose image file)
name	Text	Kusuma
phone	Text	9876543210
bio	Text	Need care service
address	Text	Hitech City
city	Text	Hyderabad
postalCode	Text	500081
recipientName	Text	Ramesh
recipientAge	Text	65
recipientGender	Text	Male
mobilityStatus	Text	Wheelchair
equipmentDetails	Text	Oxygen Cylinder
emergencyContactName	Text	Naveen
emergencyContactPhone	Text	9988776655
medicalConditions	Text	["Diabetes","BP"]
preferredCareTypes	Text	["Domiciliary Care","Respite Care"]
languagesPreferred	Text	["English","Telugu"]
```
```
Success Response

{
  "success": true,
  "message": "Profile saved successfully",
  "profile": {
    "_id": "65d9f4c2b3a1f123456789ab",
    "userId": 408,
    "role": "user",
    "name": "Kusuma",
    "email": "kusuma@gmail.com",
    "address": "Hitech City",
    "city": "Hyderabad",
    "postalCode": "500081",
    "recipientName": "Ramesh",
    "recipientAge": 65,
    "recipientGender": "Male",
    "mobilityStatus": "Wheelchair",
    "equipmentDetails": "Oxygen Cylinder",
    "emergencyContactName": "Naveen",
    "emergencyContactPhone": "9988776655",
    "medicalConditions": ["Diabetes", "BP"],
    "preferredCareTypes": ["Domiciliary Care", "Respite Care"],
    "languagesPreferred": ["English", "Telugu"],
    "isProfileCompleted": true,
    "profile": {
      "phone": "9876543210",
      "bio": "Need care service",
      "avatar": "https://your-s3-bucket-url/profile-photo.jpg"
    },
    "locationGeo": {
      "type": "Point",
      "coordinates": [78.3856, 17.4474]
    },
    "createdAt": "2026-02-18T10:30:00.000Z",
    "updatedAt": "2026-02-18T10:40:00.000Z"
  }
}


```
### 21. `GET /api/user/profile` — Get user profile  

This is for getting user profile. User Access token as a bearer token. For this we no need any body data.

Request Body

```json

Success Response

{
    "success": true,
    "message": "Profile saved successfully",
    "profile": {
        "locationGeo": {
            "type": "Point",
            "coordinates": [
                -122.1627345,
                38.0716295
            ]
        },
        "_id": "699548fa8086ecea20c53b96",
        "userId": 329,
        "name": "Nissy",
        "email": "kusumayekula872@gmail.com",
        "password": "$2b$10$nKBT/USTOpjNZkhj/wNWK.cS.ivPAf008Drik2gwwNbfPwvVW8Bh.",
        "role": "user",
        "otp": null,
        "otpExpire": null,
        "isVerified": true,
        "address": "221B Baker Street",
        "city": "London",
        "postalCode": "",
        "houseType": "",
        "floorNumber": "",
        "hasLift": false,
        "petsAtHome": false,
        "bookingFor": "",
        "recipientName": "Sujatha",
        "recipientAge": 70,
        "recipientGender": "",
        "height": "",
        "weight": "",
        "medicalConditions": [],
        "allergies": "",
        "currentMedications": "",
        "mobilityStatus": "Walks with support",
        "requiresMedicalEquipment": false,
        "equipmentDetails": "",
        "mentalCondition": "",
        "specialInstructions": "",
        "preferredCareTypes": [
            "Dementia Care",
            "Respite Care"
        ],
        "careShift": "",
        "careFrequency": "",
        "preferredCaregiverGender": "",
        "languagesPreferred": [
            "English"
        ],
        "emergencyContactName": "",
        "emergencyContactPhone": "1234567890",
        "relationship": "",
        "isProfileCompleted": true,
        "createdAt": "2026-02-18T05:07:07.005Z",
        "updatedAt": "2026-02-18T05:17:52.369Z",
        "__v": 1,
        "profile": {
            "avatar": "https://nursingbookingcare.s3.ap-south-1.amazonaws.com/providers/92aeaae7-563e-4b5b-9645-4834b5e945ca.png",
            "phone": "9542679596",
            "bio": ""
        }
    }
}

```

### 22. `GET /api/user/profile/check` — user can flag the review 

This is api is for checking user profile creation completed or not. User Access token as a bearer token. For this we no need any data as a body.

Request Body

```json

Success Response

{
    "success": true,
    "message": "Review flagged successfully",
    "review": {
        "_id": "698cdd8fe37c0391a3221a02",
        "reviewId": 2,
        "bookingId": 4,
        "userId": 888,
        "providerId": 4283,
        "rating": 5,
        "comment": "Excellent service and very professional.",
        "isFlagged": true,
        "flagReason": "This review contains false information",
        "isHidden": false,
        "createdAt": "2026-02-11T19:50:39.846Z",
        "updatedAt": "2026-02-11T20:07:57.256Z",
        "__v": 0
    }
}

```

### 23. `GET /api/chat/templates/provider` — Getting provider tamplet while chatting

Getting provider messages tamplets to send automatic messages. Provider access token will work on authorization bearer.

Request Body

```json

Success Response

{
    "success": true,
    "templates": [
        "👋 Hi, We are interested in providing care for you.",
        "We are available for your requested care service.",
        "We match your care requirements."
    ]
}

```

### 24. `GET /api/chat/templates/user` — Getting user tamplet while chatting

Getting user messages tamplets to send automatic messages. user access token will work on authorization bearer.

Request Body

```json

Success Response

{
    "success": true,
    "templates": [
        "Yes, I'm interested",
        "Tell me more",
        "Not interested"
    ]
}

```
### 25. `POST /api/chat/provider/start` — Start messaging by care provider


Sending first automatic message by provider. Provider access token will work on authorization bearer.

Request Body

```json

{
    "userId": 329,
    "text": "👋 Hi, I am interested in providing care for you."
}

Success Response

{
    "success": true,
    "chat": {
        "chatId": 4,
        "providerId": 7751,
        "userId": 329,
        "lastMessage": "👋 Hi, I am interested in providing care for you.",
        "messages": [
            {
                "sender": "provider",
                "type": "template",
                "text": "👋 Hi, I am interested in providing care for you.",
                "_id": "699684844b052945cbd56867",
                "createdAt": "2026-02-19T03:33:24.716Z"
            }
        ],
        "_id": "699684844b052945cbd56866",
        "createdAt": "2026-02-19T03:33:24.730Z",
        "updatedAt": "2026-02-19T03:33:24.730Z",
        "__v": 0
    }
}

```

### 26. `POST /api/chat/user/reply` — Sending reply by user

Sending reply by user to automatic message by provider. user access token will work on authorization bearer.

Request Body

```json

{
    "chatId": 4,
    "text": "Yes, I'm interested"
}

Success Response

{
    "success": true,
    "chat": {
        "_id": "699684844b052945cbd56866",
        "chatId": 4,
        "providerId": 7751,
        "userId": 329,
        "lastMessage": "Yes, I'm interested",
        "messages": [
            {
                "sender": "provider",
                "type": "template",
                "text": "👋 Hi, I am interested in providing care for you.",
                "_id": "699684844b052945cbd56867",
                "createdAt": "2026-02-19T03:33:24.716Z"
            },
            {
                "sender": "user",
                "type": "reply",
                "text": "Yes, I'm interested",
                "_id": "699687314b052945cbd5686b",
                "createdAt": "2026-02-19T03:44:49.146Z"
            }
        ],
        "createdAt": "2026-02-19T03:33:24.730Z",
        "updatedAt": "2026-02-19T03:44:49.150Z",
        "__v": 1
    }
}

```

### 27. `POST /api/chat/provider/booknow` — Booking Care

This is for booking slot os CQC care. 

Request Body

```json
{
    "chatId": 4
}
```
```
Success Response

{
    "success": true,
    "chat": {
        "_id": "699684844b052945cbd56866",
        "chatId": 4,
        "providerId": 7751,
        "userId": 329,
        "lastMessage": "Book Now",
        "messages": [
            {
                "sender": "provider",
                "type": "template",
                "text": "👋 Hi, I am interested in providing care for you.",
                "_id": "699684844b052945cbd56867",
                "createdAt": "2026-02-19T03:33:24.716Z"
            },
            {
                "sender": "user",
                "type": "reply",
                "text": "Yes, I'm interested",
                "_id": "699687314b052945cbd5686b",
                "createdAt": "2026-02-19T03:44:49.146Z"
            },
            {
                "sender": "provider",
                "type": "action",
                "text": "Book Now",
                "action": "BOOK_NOW",
                "_id": "699687cb4b052945cbd56870",
                "createdAt": "2026-02-19T03:47:23.410Z"
            }
        ],
        "createdAt": "2026-02-19T03:33:24.730Z",
        "updatedAt": "2026-02-19T03:47:23.414Z",
        "__v": 2
    }
}

```
### 23. `POST /api/chat/templates/provider` — Getting provider tamplet while chatting

Getting provider messages tamplets to send automatic messages. Provider access token will work on authorization bearer.

Request Body

```json

Success Response

{
    "success": true,
    "templates": [
        "👋 Hi, We are interested in providing care for you.",
        "We are available for your requested care service.",
        "We match your care requirements."
    ]
}

```### 23. `POST /api/chat/templates/provider` — Getting provider tamplet while chatting

Getting provider messages tamplets to send automatic messages. Provider access token will work on authorization bearer.

Request Body

```json

Success Response

{
    "success": true,
    "templates": [
        "👋 Hi, We are interested in providing care for you.",
        "We are available for your requested care service.",
        "We match your care requirements."
    ]
}

```
