
# Airbnb Clone Backend – Technical and Functional Requirements

This document outlines the technical and functional requirements for three core backend features: User Authentication, Property Management, and Booking System.

---

## 1. User Authentication

### Functional Requirements
- Users should be able to register as either a host or a guest.
- Users should be able to log in with email/password and via OAuth (Google, Facebook).
- JWT should be used for session management.
- Passwords must be stored securely using hashing.

### API Endpoints
- `POST /api/v1/auth/register`
- `POST /api/v1/auth/login`
- `POST /api/v1/auth/oauth/google`
- `GET /api/v1/profile`
- `PUT /api/v1/profile`

### Input/Output Specifications
**Register:**
```json
Request:
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "strongpassword123",
  "role": "host"
}

Response:
{
  "message": "Registration successful",
  "token": "<jwt_token>"
}
```
Login:
```json
Request:
{
    "email": "john@example.com",
    "password": "strongpassword123"
}

Response:
{
    "message": "Login successful",
    "token": "<jwt_token>"
}
```

### Validation Rules
- Email must be valid and unique.
- Password must be at least 8 characters.
- Role must be either `host` or `guest`.

### Performance Criteria
- Response time: <= 200ms under normal load.
- Authentication endpoints must support rate limiting and brute force protection.

---
## 2. Property Management

### Functional Requirements
- Hosts can create, update, and delete property listings.
- Listings must include key fileds such as title, description, price, availability, and amenities.
- Images should be uploaded and stored using file storage or a cloud provider.

### API Endpoints
- `POST /api/v1/properties`
- `GET /api/v1/properties`
- `GET /api/v1/properties/:id`
- `PUT /api/v1/properties/:id`
- `DELETE /api/v1/properties/:id`

### Input/Output Specifications
#### Create Listing:
```json
Request:
{
  "title": "Cozy 2-bedroom apartment",
  "description": "Great place in the city center",
  "location": "Lagos",
  "price": 100,
  "amenities": ["WiFi", "Air Conditioning"],
  "availability": {
    "start_date": "2025-06-01",
    "end_date": "2025-12-01"
  }
}

Response:
{
  "message": "Listing created successfully",
  "property_id": "1234"
}
```
#### Validation Rules
- Title, description, location, and price must be provided.
- Price must be a positive number.
- Availability dates must be valid and chronological.

#### Performance Criteria
- Listing creation must complete within 300ms.
- Support image upload with max file size of 5MB/image.

---
## 3. Booking System
### Functional Requirements
- Guests can book available listings for specific dates.
- Prevent overlapping bookings for the same property.
- Booking status should reflect confirmation, cancellation, or completion.

### API Endpoints
- `POST /api/v1/bookings`
- `GET /api/v1/bookings`
- `GET /api/v1/bookings/:id`
- `PUT /api/v1/bookings/:id/cancel`

### Input/Output Specifications
#### Create Booking:
```json
Request:
{
  "property_id": "1234",
  "start_date": "2025-07-01",
  "end_date": "2025-07-10"
}

Response:
{
  "message": "Booking confirmed",
  "booking_id": "7890",
  "status": "confirmed"
}
```
### Validation Rules
- Dates must not overlap with existing bookings for the same property.
- Start date must be before end date.
- User must be authenticated as a guest.

### Performance Criteria
- Booking operations must be atomic to prevent race conditions.
- Booking creation must be completed within 250ms under average load.

---


Author: **Alexander Edim**