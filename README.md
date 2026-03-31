# Booking.comcare-api-documentation

---

## 🌐 Base URL

https://api.nursingcarebooking.com

---

## 📬 Endpoints Overview

## Authentication End Points

### 1. `POST /api/auth/register` — Register 

Registers a new User or Provider by sending OTP to email.
Account will be created only after OTP verification.

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

Resends OTP for registration.

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

Verifies OTP and creates account.

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

Login using email and password.
Returns JWT token.

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

Sends OTP for password reset.

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

Verifies OTP and resets password.

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

Deletes a user or provider account using ID.

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

## Provider Endpoints

---

### 1. `POST /api/provider/create` — Create Provider Profile

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

Success Response

```json
{
  "success": true,
  "message": "Provider profile created successfully",
  "provider": {}
}
```

---

### 2. `GET /api/provider/providers` — Get All Providers

Fetch providers with pagination and search.

#### Query Params

```
?page=1
&limit=10
&typeOfCare=Dementia Care
&search=john

Success Response

{
  "success": true,
  "totalProviders": 25,
  "currentPage": 1,
  "totalPages": 3,
  "providers": []
}
```

---

### 3. `POST /api/provider/profile/providerid` — Get Provider Profile

Authorization: Bearer Token

#### Request Body

```json
{
  "providerId": 4283
}

Success Response

{
  "success": true,
  "provider": {}
}
```


### 4. `PUT /api/provider/profile` — Update Provider Profile

Update provider profile (partial update supported).  
Accepts multipart/form-data.  
Authorization: Bearer Token

Request Body

```
providerId: 4283
name: Updated Name
serviceRadiusKm: 15
```

✔ Replace images automatically  
✔ Auto geocode if address updated  

Success Response

```json
{
  "success": true,
  "message": "Provider profile updated successfully",
  "provider": {}
}
```

---

### 5. `DELETE /api/provider/profile` — Delete Provider Profile

Deletes provider profile and all S3 images.  
Authorization: Bearer Token

Request Body

```json
{
  "providerId": 4283
}


Success Response

{
  "success": true,
  "message": "Provider profile along with images deleted successfully"
}
```

---

### 6. `POST /api/provider/search` — Geo Search Providers

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

Success Response

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

### 1. `POST /api/booking/create` — Create Booking

Allows a User to create a new booking request for a provider.
User profile must be completed.
Service type must be valid.
Booking status will be Pending initially.

Request Body

```json
{
    "providerId": 7751,
    "appointmentDate": "2026-02-20T10:30:00.000Z",
    "serviceType": "Domiciliary Care",
    "notes": "Patient needs daily support"
}
```

#### Success Response

```json

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


### 2. `PUT /api/booking/accept` — Accepting Booking

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

### 3. `PUT /api/booking/reject` — Rejecting booking

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

### 4. `PUT /api/booking/cancel` — User cancel booking

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

### 5. `PUT /api/booking/complete` — Provider Completed Booking

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
# Booking History APIs

## Get User Booking History

**Endpoint**

GET /api/bookings/history/user

**Headers**

Authorization: Bearer <USER_TOKEN>

**Description**

Returns booking history for the logged-in user including provider details and appointment information.

### Example Response

```json
{
 "success": true,
 "total": 1,
 "bookings": [
  {
   "bookingId": 101,
   "userId": 2001,
   "providerId": 5001,
   "userName": "Rahul",
   "providerName": "Dr Kumar",
   "serviceType": "Nursing Care",
   "status": "Completed"
  }
 ]
}
```
# Booking History APIs

## Get User Booking History

**Endpoint**

GET /api/bookings/history/provider

**Headers**

Authorization: Bearer <PROVIDER_TOKEN>

**Description**

Returns booking history for the logged-in provider including user details and appointment information.

### Example Response

```json
{
 "success": true,
 "total": 1,
 "bookings": [
  {
   "bookingId": 101,
   "userId": 2001,
   "providerId": 5001,
   "userName": "Rahul",
   "providerName": "Dr Kumar",
   "serviceType": "Nursing Care",
   "status": "Completed"
  }
 ]
}
```
### 6. `POST /api/booking/details` — getting details by bookingId

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

### 1. `POST /api/review/add` — Create Review by Booking Id

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

### 2. `POST /api/review/provider` — Provider can see theire reviews

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

### 3. `PUT /api/review/flag` — user can flag the review 

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

# User Endpoints

### 1. `POST /api/user/profile` — user can create their profile 

This is for user can create their profile. User Access token as a bearer token. 

Request Body

```json

{
  "name": "Kusuma Yekula", *
  "email": "kusuma@example.com", *
  "password": "123456", *
  "role": "user",*

  "profile": {
    "avatar": "https://example.com/avatar.jpg",
    "phone": "9876543210",
    "bio": "Looking for elderly care services"
  },

  "address": "12-3-45, MG Road", *
  "city": "Hyderabad",*
  "postalCode": "500001",*
  "houseType": "Apartment",*
  "floorNumber": "3",
  "hasLift": true,
  "petsAtHome": false,

  "locationGeo": {
    "type": "Point",
    "coordinates": [78.4867, 17.3850]
  },

  "bookingFor": "Father",*
  "recipientName": "Ramesh Yekula",*
  "recipientAge": 72,*
  "recipientGender": "Male",*
  "height": "5.6ft",
  "weight": "70kg",
  "medicalConditions": ["Diabetes", "BP"],*
  "allergies": "Penicillin",
  "currentMedications": "Insulin",
  "mobilityStatus": "Needs support",*
  "requiresMedicalEquipment": true,
  "equipmentDetails": "Wheelchair",
  "mentalCondition": "Normal",
  "specialInstructions": "Needs morning assistance",

  "preferredCareTypes": ["Elderly Care"],*
  "careShift": "Day",*
  "careFrequency": "Daily",*
  "preferredCaregiverGender": "Female",
  "languagesPreferred": ["Telugu", "English"],*

  "emergencyContactName": "Suresh",*
  "emergencyContactPhone": "9876543211",*
  "relationship": "Son",*

  "isProfileCompleted": true
}

Success Response

{
    "success": true,
    "message": "Profile created successfully",
    "profile": {
        "locationGeo": {
            "type": "Point",
            "coordinates": [
                78.4867,
                17.385
            ]
        },
        "_id": "699548fa8086ecea20c53b96",
        "userId": 329,
        "name": "Kusuma Yekula",
        "email": "kusuma@example.com",
        "password": "123456",
        "role": "user",
        "otp": null,
        "otpExpire": null,
        "isVerified": true,
        "address": "12-3-45, MG Road",
        "city": "Hyderabad",
        "postalCode": "500001",
        "houseType": "Apartment",
        "floorNumber": "3",
        "hasLift": true,
        "petsAtHome": false,
        "bookingFor": "Father",
        "recipientName": "Ramesh Yekula",
        "recipientAge": 72,
        "recipientGender": "Male",
        "height": "5.6ft",
        "weight": "70kg",
        "medicalConditions": [
            "Diabetes",
            "BP"
        ],
        "allergies": "Penicillin",
        "currentMedications": "Insulin",
        "mobilityStatus": "Needs support",
        "requiresMedicalEquipment": true,
        "equipmentDetails": "Wheelchair",
        "mentalCondition": "Normal",
        "specialInstructions": "Needs morning assistance",
        "preferredCareTypes": [
            "Elderly Care"
        ],
        "careShift": "Day",
        "careFrequency": "Daily",
        "preferredCaregiverGender": "Female",
        "languagesPreferred": [
            "Telugu",
            "English"
        ],
        "emergencyContactName": "Suresh",
        "emergencyContactPhone": "9876543211",
        "relationship": "Son",
        "isProfileCompleted": true,
        "createdAt": "2026-02-18T05:07:07.005Z",
        "updatedAt": "2026-02-27T08:04:29.230Z",
        "__v": 3,
        "profile": {
            "avatar": "https://example.com/avatar.jpg",
            "phone": "9876543210",
            "bio": "Looking for elderly care services"
        }
    }
}

```
### 2. `GET /api/user/profile` — Get user profile  

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

### 3. `GET /api/user/profile/check` — User can check their profile  

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
## Quickchat Endpoints

### 1. `GET /api/chat/templates/provider` — Getting provider tamplet while chatting

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

### 2. `GET /api/chat/templates/user` — Getting user tamplet while chatting

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
### 3. `POST /api/chat/provider/start` — Start messaging by care provider


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

### 4. `POST /api/chat/user/reply` — Sending reply by user

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

### 5. `POST /api/chat/provider/booknow` — Booking Care

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
### 6. `GET /api/chat/user` — Getting user chat

We no need any body format just user authorization bearer only we need after that we can get the login user chat.

Request Body

```json

Success Response

{
    "success": true,
    "chats": []
}

```

### 7. `PGET /api/chat/provider` — Getting provider chat

We no need any body format just provider authorization bearer only we need after that we can get the login provieder chat.

Request Body

```json

Success Response

{
    "success": true,
    "chats": [
        {
            "_id": "6994aa6687d82f7a9684caf7",
            "chatId": 1,
            "providerId": 7751,
            "userId": 888,
            "lastMessage": "Book Now",
            "messages": [
                {
                    "sender": "provider",
                    "type": "template",
                    "text": "👋 Hi, I am interested in providing care for you.",
                    "_id": "6994aa6687d82f7a9684caf8",
                    "createdAt": "2026-02-17T17:50:30.052Z"
                },
                {
                    "sender": "user",
                    "type": "reply",
                    "text": "Yes, I'm interested",
                    "_id": "6994ab4187d82f7a9684cafc",
                    "createdAt": "2026-02-17T17:54:09.066Z"
                },
                {
                    "sender": "provider",
                    "type": "action",
                    "text": "Book Now",
                    "action": "BOOK_NOW",
                    "_id": "6994abd687d82f7a9684cb01",
                    "createdAt": "2026-02-17T17:56:38.449Z"
                }
            ],
            "createdAt": "2026-02-17T17:50:30.066Z",
            "updatedAt": "2026-02-17T17:56:38.451Z",
            "__v": 2
        },
        {
            "_id": "69955d9c40d6d54d72719d83",
            "chatId": 2,
            "providerId": 7751,
            "userId": 329,
            "lastMessage": "Book Now",
            "messages": [
                {
                    "sender": "provider",
                    "type": "template",
                    "text": "👋 Hi, I am interested in providing care for you.",
                    "_id": "69955d9c40d6d54d72719d84",
                    "createdAt": "2026-02-18T06:35:08.400Z"
                },
                {
                    "sender": "user",
                    "type": "reply",
                    "text": "Yes, I'm interested",
                    "_id": "69955e1d40d6d54d72719d88",
                    "createdAt": "2026-02-18T06:37:17.567Z"
                },
                {
                    "sender": "provider",
                    "type": "action",
                    "text": "Book Now",
                    "action": "BOOK_NOW",
                    "_id": "69955e5b40d6d54d72719d8d",
                    "createdAt": "2026-02-18T06:38:19.261Z"
                }
            ],
            "createdAt": "2026-02-18T06:35:08.429Z",
            "updatedAt": "2026-02-18T06:38:19.278Z",
            "__v": 2
        },
        {
            "_id": "69956685c98a4f9ffd163b17",
            "chatId": 3,
            "providerId": 7751,
            "userId": 329,
            "lastMessage": "Book Now",
            "messages": [
                {
                    "sender": "provider",
                    "type": "template",
                    "text": "👋 Hi, I am interested in providing care for you.",
                    "_id": "69956685c98a4f9ffd163b18",
                    "createdAt": "2026-02-18T07:13:09.596Z"
                },
                {
                    "sender": "user",
                    "type": "reply",
                    "text": "Yes, I'm interested",
                    "_id": "699566eac98a4f9ffd163b1c",
                    "createdAt": "2026-02-18T07:14:50.406Z"
                },
                {
                    "sender": "provider",
                    "type": "action",
                    "text": "Book Now",
                    "action": "BOOK_NOW",
                    "_id": "69956712c98a4f9ffd163b21",
                    "createdAt": "2026-02-18T07:15:30.074Z"
                }
            ],
            "createdAt": "2026-02-18T07:13:09.663Z",
            "updatedAt": "2026-02-18T07:15:30.087Z",
            "__v": 2
        },
        {
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
    ]
}

```
# Posts Endpoints

### 1. `POST /api/post/create` — Create Care Request Post

Create a new care request post.  
Only **Service User** can create posts.  
Authorization: User Access Token (Bearer)  
Content-Type: multipart/form-data

#### Request Body (form-data)

| Key | Type | Required |
|---|---|---|
| title | Text | ✅ |
| description | Text | ✅ |
| careType | Text | ✅ |
| city | Text | ✅ |
| postalCode | Text | ✅ |
| appointmentDate | Date | ✅ |
| budget | Number | ✅ |
| photo | File | ❌ |

#### Success Response

```json
{
  "success": true,
  "message": "Post created successfully",
  "post": {
    "postId": 12,
    "userId": 329,
    "title": "Need dementia care",
    "description": "Looking for experienced caregiver",
    "careType": "Dementia Care",
    "city": "London",
    "postalCode": "NW16XE",
    "budget": 200,
    "appointmentDate": "2026-02-22T10:00:00.000Z",
    "photo": "https://s3-url/post-image.jpg",
    "status": "Active"
  }
}
```
### 2.`PUT /api/post/update ` — Update Post

Update an existing post.
Only Post Owner can update.
Authorization: User Access Token (Bearer)
Content-Type: multipart/form-data

### Request Body (form-data)
Key	Required
postId	✅
title	❌
description	❌
careType	❌
city	❌
postalCode	❌
budget	❌
appointmentDate	❌
status	❌
photo	❌

⭐ Old photo will be removed from S3 if new photo uploaded.

### Success Response
```json
{
  "success": true,
  "message": "Post updated successfully",
  "post": {}
}
```
### 3. `DELETE /api/post/delete `— Delete Post

Delete post and its S3 image.
Only Post Owner can delete.
Authorization: User Access Token (Bearer)

### Request Body
```json
{
  "postId": 12
}
```
```json
Success Response
{
  "success": true,
  "message": "Post deleted successfully"
}
```
### 4. GET /api/post/all — Get All Active Posts

Fetch all active care request posts.
Accessible by Users & Providers.
Authorization: Bearer Token

### Success Response
```json
{
  "success": true,
  "count": 10,
  "posts": []
}

Sorted by newest first.
```
### 5.` GET /api/post/my-posts` — Get My Posts

Fetch posts created by logged-in user.
Only Service User can access.
Authorization: User Access Token (Bearer)

### Success Response
```json
{
  "success": true,
  "count": 3,
  "posts": []
}
```
```json
📌 Common Error Response
{
  "success": false,
  "message": "Error message",
  "error": "optional_debug_error"
}
```

# Admin Endpoints

AUTHENTICATION


### 1 POST /api/admin/send-otp - SEND OTP
Access: Public

```json
Request Body:
{
    "email": "kusumayekula0191@gmail.com"
}
```
### Success Response:

```json
{
    "success": true,
    "message": "OTP sent to email successfully"
}
 
Error Response:


{
    "success": false,
    "message": "Email is required"
}
```

### 2 POST /api/admin/verify-otp - VERIFY OTP
Access: Public

Request Body:
```json
{
    "email": "kusumayekula0191@gmail.com",
    "otp": "123456"
}

Success Response:

{
    "success": true,
    "message": "Login successful",
    "adminId": 1,
    "accessToken": "JWT_TOKEN"
}

Error Response:

{
    "success": false,
    "message": "Invalid OTP"
}

{
    "success": false,
    "message": "OTP expired"
}
```

All routes below require header:

Authorization: Bearer YOUR_ADMIN_TOKEN
Content-Type: application/json


### 3 POST /api/admin/providers/list - GET PROVIDERS LIST

Request Body:
```json
{
    "status": "pending",
    "search": "john"
}

Status values:
pending
verified
rejected

Success Response:

{
    "success": true,
    "providers": 
    {
        "providerId": 101,
        "name": "John Care",
        "email": "john@example.com",
        "isVerified": false,
        "isRejected": false,
        "isBlocked": false,
        "createdAt": "2026-02-20T10:00:00.000Z"
        }
}
```
### 4 POST /api/admin/providers/details - GET PROVIDER DETAILS

Request Body:
```json
{
    "providerId": 101
}

Success Response:

{
    "success": true,
    "provider": {
        "providerId": 101,
        "name": "John Care",
        "email": "john@example.com",
        "typeOfCare": "Domiciliary Care",
        "isVerified": false,
        "isBlocked": false,
        "adminNote": ""
    }
}
```
### 5 POST /api/admin/providers/verify - VERIFY PROVIDER

Request Body:
```json
{
    "providerId": 101,
    "note": "Documents verified"
}

Success Response:

{
    "success": true,
    "message": "Provider verified",
    "provider": {
        "providerId": 101,
        "isVerified": true,
        "isRejected": false,
        "adminNote": "Documents verified"
        }
}
```
### 6 POST /api/admin/providers/reject - REJECT PROVIDER

Request Body:
```json
{
    "providerId": 101,
    "note": "Invalid documents"
}

Success Response:

{
    "success": true,
    "message": "Provider rejected",
    "provider": {
        "providerId": 101,
        "isVerified": false,
        "isRejected": true,
        "adminNote": "Invalid documents"
        }
}
```
### 7 POST /api/admin/providers/block - BLOCK / UNBLOCK PROVIDER

Request Body:
```json
{
    "providerId": 101,
    "blocked": true,
    "note": "Policy violation"
}

Success Response:

{
    "success": true,
    "message": "Provider blocked",
    "provider": {
        "providerId": 101,
        "isBlocked": true
    }
}
```

### 8 POST /api/admin/reviews/list - GET REVIEWS

Request Body:
```json
{
    "status": "pending",
    "providerId": 101,
    "userId": 201
}

Success Response:

{
    "success": true,
    "total": 1,
    "reviews": [
        {
        "reviewId": 301,
        "providerId": 101,
        "userId": 201,
        "rating": 5,
        "comment": "Excellent service",
        "status": "pending",
        "createdAt": "2026-02-20T09:00:00.000Z"
        }
    ]
}
````
### 9 POST /api/admin/reviews/approve - APPROVE REVIEW

Request Body:
```json
{
    "reviewId": 301
}

Success Response:

{
    "success": true,
    "message": "Review approved",
    "review": {
        "reviewId": 301,
        "status": "approved"
    }
}
```
### 10 POST /api/admin/reviews/reject - REJECT REVIEW

Request Body:
```json
{
    "reviewId": 301,
    "reason": "Inappropriate language"
}

Success Response:

{
    "success": true,
    "message": "Review rejected",
    "review": {
        "reviewId": 301,
        "status": "rejected",
        "rejectReason": "Inappropriate language"
    }
}
```
### 11 POST /api/admin/reviews/delete - DELETE REVIEW

Request Body:
```json

{
    "reviewId": 301
}

Success Response:

{
    "success": true,
    "message": "Review deleted"
}
```

### 12 POST /api/admin/bookings/list - GET BOOKINGS

Request Body:
```json
{
    "status": "pending",
    "providerId": 101,
    "userId": 201,
    "from": "2026-01-01",
    "to": "2026-12-31"
}

Success Response:

{
    "success": true,
    "bookings": [
    {
        "bookingId": 501,
        "providerId": 101,
        "userId": 201,
        "status": "pending",
        "appointmentDate": "2026-03-01",
        "createdAt": "2026-02-20T10:00:00.000Z"
       }
    ]
}
```
### 13 POST /api/admin/bookings/details - GET BOOKING DETAILS

Request Body:
```json
{
    "bookingId": 501
}

Success Response:

{
    "success": true,
    "booking": {
        "bookingId": 501,
        "providerId": 101,
        "userId": 201,
        "status": "pending",
        "appointmentDate": "2026-03-01"
    }
}
```
### 14 POST /api/admin/bookings/update-status - UPDATE BOOKING STATUS

Request Body:
```json

{
    "bookingId": 501,
    "status": "confirmed"
}

Status values:
pending
confirmed
completed
cancelled

Success Response:

{
    "success": true,
    "message": "Booking updated",
    "booking": {
        "bookingId": 501,
        "status": "confirmed"
    }
}
```

### 15 POST /api/admin/bookings/cancel - CANCEL BOOKING

Request Body:
```json
{
    "bookingId": 501,
    "reason": "Customer request"
}

Success Response:

{
"success": true,
    "message": "Booking cancelled",
    "booking": {
    "bookingId": 501,
    "status": "cancelled",
    "cancelReason": "Customer request"
    }
}
```

### 16 GET /api/admin/dashboard/stats - DASHBOARD STATS

```json
Success Response:
{
    "success": true,
    "stats": {
        "totalUsers": 150,
        "totalProviders": 40,
        "pendingProviders": 5,
        "totalBookings": 320,
        "pendingBookings": 20,
        "totalPayments": 280,
        "paidPayments": 250,
        "totalRevenue": 125000
    }
}
```

##     Notification Endpoints

### 1 GET /web-notification - Getting notification of login person

We no need any body just we need only access token of user or provider for getting their notification 

Request Body:
```json

Success Response:

{
    "success": true,
    "role": "user",
    "recipientId": 587,
    "notifications": [
        {
            "_id": "69aa6882a52d7ebec0bc9d6d",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_COMPLETED",
            "title": "Booking Completed",
            "message": "Your booking 21 has been completed",
            "link": "/bookings/21",
            "isRead": false,
            "createdAt": "2026-03-06T05:39:14.496Z",
            "updatedAt": "2026-03-06T05:39:14.496Z",
            "notificationId": 10,
            "__v": 0
        },
        {
            "_id": "69aa683de023d2a9ad17fef1",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_COMPLETED",
            "title": "Booking Completed",
            "message": "Your booking 20 has been completed",
            "link": "/bookings/20",
            "isRead": false,
            "createdAt": "2026-03-06T05:38:05.638Z",
            "updatedAt": "2026-03-06T05:38:05.638Z",
            "notificationId": 9,
            "__v": 0
        },
        {
            "_id": "69aa6511eb7f734d27a3634a",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_REJECTED",
            "title": "Booking Not Approved",
            "message": "Booking 25 not approved",
            "link": "/bookings/25",
            "isRead": false,
            "createdAt": "2026-03-06T05:24:33.835Z",
            "updatedAt": "2026-03-06T05:24:33.835Z",
            "notificationId": 6,
            "__v": 0
        },
        {
            "_id": "69aa64ac0bfdf945789ad23e",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_REJECTED",
            "title": "Booking Rejected",
            "message": "Booking 24 rejected",
            "link": "/bookings/24",
            "isRead": false,
            "createdAt": "2026-03-06T05:22:52.219Z",
            "updatedAt": "2026-03-06T05:22:52.219Z",
            "notificationId": 5,
            "__v": 0
        },
        {
            "_id": "69aa5f6b10342c734760bbb2",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_APPROVED",
            "title": "Booking Approved",
            "message": "Your booking 23 has been approved",
            "link": "/bookings/23",
            "isRead": false,
            "createdAt": "2026-03-06T05:00:27.732Z",
            "updatedAt": "2026-03-06T05:00:27.732Z",
            "notificationId": 4,
            "__v": 0
        },
        {
            "_id": "69aa5f3b501ce912fbcc9d5e",
            "recipient": 587,
            "sender": 7751,
            "type": "BOOKING_APPROVED",
            "title": "Booking Approved",
            "message": "Your booking 22 has been approved",
            "link": "/bookings/22",
            "isRead": false,
            "createdAt": "2026-03-06T04:59:39.766Z",
            "updatedAt": "2026-03-06T04:59:39.766Z",
            "notificationId": 3,
            "__v": 0
        }
    ],
    "unreadCount": 6
}
```

### 2 PUT /web-notification/read/69aa6882a52d7ebec0bc9d6d - Marking read a single notification by notification id 

We no need any body just we need  notification id by end points along with access token

Request Body:
```json

Success Response:

{
    "success": true,
    "message": "Notification marked as read",
    "notification": {
        "_id": "69aa6882a52d7ebec0bc9d6d",
        "recipient": 587,
        "sender": 7751,
        "type": "BOOKING_COMPLETED",
        "title": "Booking Completed",
        "message": "Your booking 21 has been completed",
        "link": "/bookings/21",
        "isRead": true,
        "createdAt": "2026-03-06T05:39:14.496Z",
        "updatedAt": "2026-03-09T05:53:09.495Z",
        "notificationId": 10,
        "__v": 0
    }
}

```

### 3 PUT /web-notification/read-all - Marking read all notification 

We no need any body just we need  access token

Request Body:
```json

Success Response:

{
    "success": true,
    "message": "All notifications marked as read"
}

```
### 4 Delete /web-notification/69aa6882a52d7ebec0bc9d6d - Deleting notification by notification id  

We no need any body just we need  notification id by end points

Request Body:
```json

Success Response:

{
    "success": true,
    "message": "Notification deleted successfully"
}

```
## 🔐 Authentication

All admin APIs require:

```
Authorization: Bearer <ADMIN_TOKEN>
Content-Type: application/json
```

---

# 📊 DASHBOARD APIs

---

## 1. Get Basic Dashboard Stats

### Endpoint

```
GET /dashboard/stats
```

### Query Params (Optional)

| Param     | Type   | Description |
| --------- | ------ | ----------- |
| startDate | String | YYYY-MM-DD  |
| endDate   | String | YYYY-MM-DD  |

### Example

```
GET /dashboard/stats?startDate=2026-03-01&endDate=2026-03-31
```

### Response

```json
{
  "success": true,
  "stats": {
    "totalUsers": 100,
    "totalProviders": 50,
    "pendingProviders": 10,
    "totalBookings": 200,
    "pendingBookings": 20,
    "totalPayments": 150,
    "paidPayments": 120,
    "totalRevenue": 50000,
    "revenueTrend": [],
    "bookingStatusStats": [],
    "topProviders": [],
    "failedPayments": 5,
    "cancelledBookings": 8
  }
}
```

---

## 2. Get Advanced Dashboard Stats

### Endpoint

```
GET /dashboard/advanced-stats
```

### Query Params (Optional)

| Param     | Type   | Description |
| --------- | ------ | ----------- |
| startDate | String | YYYY-MM-DD  |
| endDate   | String | YYYY-MM-DD  |

### Response Includes

* Location stats
* Avg completion time
* Ratings analytics
* Revenue trends

---

# 🗑 DELETE REQUEST APIs

---

## 3. Get All Delete Requests

### Endpoint

```
GET /delete-requests
```

### Query Params

| Param  | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| status | String | pending / approved / rejected |

### Example

```
GET /delete-requests?status=pending
```

---

## 4. Search Delete Requests

### Endpoint

```
GET /delete-requests/search
```

### Query Params

| Param      | Type            |
| ---------- | --------------- |
| userId     | Number          |
| providerId | Number          |
| name       | String          |
| status     | String          |
| type       | user / provider |
| page       | Number          |
| limit      | Number          |

### Example

```
GET /delete-requests/search?name=rahul&type=user&page=1
```

---

## 5. Delete Requests Dashboard

### Endpoint

```
GET /delete-requests/dashboard
```

### Query Params

| Param     | Description               |
| --------- | ------------------------- |
| startDate | Date filter               |
| endDate   | Date filter               |
| status    | pending/approved/rejected |
| type      | user/provider             |
| page      | pagination                |
| limit     | pagination                |

---

# ✅ ADMIN ACTION APIs

---

## 6. Approve Delete Request

### Endpoint

```
POST /delete/approve
```

### Body

```json
{
  "requestId": "65f1a2b3c4d5e6f789abcd12"
}
```

### Response

```json
{
  "success": true,
  "message": "Account soft deleted successfully"
}
```

---

## 7. Reject Delete Request

### Endpoint

```
POST /delete/reject
```

### Body

```json
{
  "requestId": "65f1a2b3c4d5e6f789abcd12",
  "reason": "Active bookings exist"
}
```

---

# ⚡ BULK ACTION APIs

---

## 8. Bulk Approve Delete Requests

### Endpoint

```
POST /delete/bulk-approve
```

### Body

```json
{
  "requestIds": [
    "65f1a2b3c4d5e6f789abcd12",
    "65f1a2b3c4d5e6f789abcd34"
  ]
}
```

---

## 9. Bulk Reject Delete Requests

### Endpoint

```
POST /delete/bulk-reject
```

### Body

```json
{
  "requestIds": [
    "65f1a2b3c4d5e6f789abcd12",
    "65f1a2b3c4d5e6f789abcd34"
  ],
  "reason": "Policy violation"
}
```

---

# ⚠️ Error Responses

### Unauthorized

```json
{
  "success": false,
  "message": "Unauthorized"
}
```

### Invalid Request

```json
{
  "success": false,
  "error": "Invalid request"
}
```

---

# 🚀 Notes

* All delete operations are **soft deletes**
* Audit logs are maintained for every action
* Bulk operations support up to recommended limits (e.g., 100 items)
* Use pagination for large datasets

---

# 📦 Related Models

* User
* Provider
* DeleteRequest
* AuditLog
* Booking
* Payment
* Review

---

# 🔥 Summary

This system supports:

* Admin-controlled account deletion
* Search & filtering
* Bulk operations
* Dashboard analytics
* Audit logging
* Production-grade scalability

---

