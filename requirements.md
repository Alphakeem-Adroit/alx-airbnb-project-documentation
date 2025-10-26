# 🏠 Airbnb Clone Backend — Requirement Specifications

## 🎯 Objective
This document details the **technical and functional requirements** for three key backend features of the Airbnb Clone project:
1. **User Authentication**
2. **Property Management**
3. **Booking System**

The goal is to define clear specifications for implementation, including endpoints, request/response formats, validation rules, and performance requirements.

---

## 1️⃣ User Authentication

### 💡 Description
Handles user registration, login, and session management for both **Guests** and **Hosts** using secure authentication mechanisms such as JWT.

### 🧩 Functional Requirements
- Users can register as either Guest or Host.
- Users can log in with valid credentials.
- System validates credentials and issues a secure JWT.
- Passwords are stored securely using hashing (e.g., bcrypt).
- Users can update their profile information and upload profile images.

### 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|-----------|-------------|
| `POST` | `/api/v1/auth/register` | Register a new user |
| `POST` | `/api/v1/auth/login` | Authenticate user and return JWT token |
| `GET`  | `/api/v1/users/profile` | Retrieve user profile (authenticated users only) |
| `PUT`  | `/api/v1/users/profile` | Update user profile details |

### 📥 Input Specification

#### Registration
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123",
  "role": "host"
}
```

#### Login

```json
{
  "email": "john@example.com",
  "password": "SecurePass123"
}
```

### 📤 Output Specification

#### Successful Login Response

```json
{
  "status": "success",
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI...",
  "user": {
    "id": 12,
    "name": "John Doe",
    "email": "john@example.com",
    "role": "host"
  }
}
```

### ✅ Validation Rules

* Email must be valid and unique.
* Password must contain at least 8 characters, one uppercase, and one special character.
* Role must be either “guest” or “host”.
* Duplicate registrations must be rejected.

### ⚙️ Performance Criteria

* Authentication API response time ≤ 400ms.
* JWT tokens expire in 24 hours.
* System must handle at least 500 concurrent login requests per minute.

---

## 2️⃣ Property Management

### 💡 Description

Allows Hosts to create, update, and delete property listings and enables Guests to view and search for available properties.

### 🧩 Functional Requirements

* Hosts can create property listings with title, description, price, location, and amenities.
* Hosts can edit or delete their listings.
* Guests can search and filter properties.
* System should support pagination for large datasets.

### 🌐 API Endpoints

| Method   | Endpoint                 | Description                                                    |
| -------- | ------------------------ | -------------------------------------------------------------- |
| `POST`   | `/api/v1/properties`     | Create a new property listing                                  |
| `GET`    | `/api/v1/properties`     | Retrieve all property listings (supports filters & pagination) |
| `GET`    | `/api/v1/properties/:id` | Retrieve a specific property by ID                             |
| `PUT`    | `/api/v1/properties/:id` | Update property details                                        |
| `DELETE` | `/api/v1/properties/:id` | Delete a property listing                                      |

### 📥 Input Specification

#### Add Property

```json
{
  "title": "Luxury Apartment in Lagos",
  "description": "A 2-bedroom apartment with ocean view.",
  "price": 250,
  "location": "Lekki, Lagos",
  "amenities": ["Wi-Fi", "Pool", "Air Conditioning"],
  "availability": true
}
```

### 📤 Output Specification

```json
{
  "status": "success",
  "data": {
    "id": 101,
    "title": "Luxury Apartment in Lagos",
    "host_id": 5,
    "price": 250,
    "location": "Lekki, Lagos",
    "created_at": "2025-10-25T12:00:00Z"
  }
}
```

### ✅ Validation Rules

* `title` and `description` are required.
* `price` must be numeric and greater than zero.
* `location` is required.
* Only the property owner (host) can update or delete their property.

### ⚙️ Performance Criteria

* Search queries should return within 800ms.
* API must handle up to 1,000 concurrent property listing requests.
* Frequently queried data should be cached using Redis or similar.

---

## 3️⃣ Booking System

### 💡 Description

Enables Guests to book available properties, handles date validation, booking statuses, and payment tracking.

### 🧩 Functional Requirements

* Guests can book properties for available dates.
* System validates booking dates to prevent double bookings.
* Hosts can view bookings for their properties.
* Guests and Hosts can cancel bookings (based on policy).

### 🌐 API Endpoints

| Method | Endpoint                      | Description                                      |
| ------ | ----------------------------- | ------------------------------------------------ |
| `POST` | `/api/v1/bookings`            | Create a new booking                             |
| `GET`  | `/api/v1/bookings`            | Retrieve all bookings (filtered by user or host) |
| `GET`  | `/api/v1/bookings/:id`        | Retrieve booking details                         |
| `PUT`  | `/api/v1/bookings/:id/cancel` | Cancel a booking                                 |

### 📥 Input Specification

#### Create Booking

```json
{
  "property_id": 101,
  "guest_id": 15,
  "check_in": "2025-11-10",
  "check_out": "2025-11-15"
}
```

### 📤 Output Specification

```json
{
  "status": "success",
  "message": "Booking created successfully",
  "booking": {
    "id": 501,
    "property_id": 101,
    "guest_id": 15,
    "status": "confirmed",
    "total_amount": 1250,
    "created_at": "2025-10-26T14:30:00Z"
  }
}
```

### ✅ Validation Rules

* `check_in` must be earlier than `check_out`.
* Property must be available during the requested dates.
* Guests cannot book their own property.
* Cancellation allowed only before check-in date.

### ⚙️ Performance Criteria

* Booking creation should complete in ≤ 600ms.
* System supports at least 200 concurrent booking operations.
* Data consistency maintained with transactional database operations.

---

## 🧱 Repository Structure

```
alx-airbnb-project-documentation/
│
├── use-case-diagram/
│   └── use-case-diagram.png
│
├── user-stories/
│   └── README.md
│
├── project-requirements/
│   └── README.md
│
└── requirements.md   ← (this file)
```

---

## ✍️ Author

**Busari Abdulhakeem Tunde (Aalphakeem Adroit)**
Backend Developer | ALX Software Engineering Program
📧 [Contact via GitHub](https://github.com/Alphakeem-Adroit)

```

---
