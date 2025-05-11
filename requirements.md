# Airbnb Clone – Technical and Functional Requirements

This document outlines the technical and functional requirements for three key backend features of the Airbnb Clone project: User Authentication, Property Management, and Booking System.

---

## 1. User Authentication

### Functional Requirements

- Users must be able to register as guests or hosts.
- Login should be handled using secure methods.
- Passwords must be hashed and stored securely.
- JWT should be used for user sessions.
- OAuth support (Google, Facebook) is optional but recommended.

### API Endpoints

- `POST /api/auth/register`: Register a new user.
- `POST /api/auth/login`: Log in with email and password.
- `GET /api/users/me`: Fetch authenticated user profile.
- `PUT /api/users/me`: Update profile.

### Input/Output Specifications

#### Registration Input

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "securePass123",
  "role": "host"
}
```

#### Registration Output

```json
{
  "message": "User registered successfully",
  "token": "JWT_TOKEN"
}
```

### Validation Rules

- Email must be unique and in a valid format.
- Password must be at least 8 characters.
- Role must be either `guest` or `host`.

### Performance Criteria

- Registration and login actions should complete in less than 1 second.
- Token verification should complete in less than 200 milliseconds.


## 2. Property Management

### Functional Requirements

- Hosts can add, update, and delete property listings.
- Properties should contain: title, description, location, price, amenities, availability.
- Only the listing host can update/delete their property.

### API Endpoints

- `POST /api/properties`: Add a property.
- `PUT /api/properties/:id`: Update property.
- `DELETE /api/properties/:id`: Delete property.
- `GET /api/properties`: View all properties.
- `GET /api/properties/:id`: View single property.

### Input/Output Specifications

#### Create Property Input

```json
{
  "title": "Modern Apartment",
  "description": "Great location, 2 beds, Wi-Fi",
  "location": "New York",
  "price": 150.00,
  "amenities": ["Wi-Fi", "Kitchen", "Washer"],
  "availability": {
    "start_date": "2025-06-01",
    "end_date": "2025-06-30"
  }
}
```

#### Create Property Output

```json
{
   "message": "Property created successfully",
  "property_id": "uuid-here"
}
```

### Validation Rules

- Title and description are required.
- Price must be greater than 0.
- Start date must be before end date.

### Performance Criteria

- Listings should load within 1 second using optimized database indexing.
- Support paginated responses for large datasets.

## 3. Booking System

### Functional Overview

This feature allows guests to book available properties for specific dates and hosts to manage these bookings.

### API Endpoints

#### POST `/api/bookings`

- **Description**: Create a new booking.
- **Input**:
  - `property_id`: UUID, required
  - `user_id`: UUID, required
  - `start_date`: date, required
  - `end_date`: date, required
- **Output**:
  ```json
  {
    "message": "Booking created",
    "booking_id": "UUID",
    "status": "pending"
  }

### GET `/api/bookings/:id`

- **Description**: Get booking details.

---

### Validation Rules

- Prevent overlapping bookings for the same property.
- `start_date` must be before `end_date`.
- Booking cannot be made for past dates.

---

### Performance Criteria

- Booking availability check must complete in < 500ms.
- Date ranges must be efficiently validated using indexed queries.

---

### Additional Notes

- All endpoints return JSON responses.
- JWT-based authentication is required for protected routes.
- Role-based access ensures that only hosts can manage properties and only guests can book.
