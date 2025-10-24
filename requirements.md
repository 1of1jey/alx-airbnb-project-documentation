# Backend Feature Requirements Specification

## Document Information
- **Project**: Airbnb Clone Backend
- **Version**: 1.0
- **Date**: October 2024
- **Author**: ALX Software Engineering Team

---

## Table of Contents
1. [User Authentication System](#1-user-authentication-system)
2. [Property Management System](#2-property-management-system)
3. [Booking System](#3-booking-system)

---

## 1. User Authentication System

### 1.1 Overview
The User Authentication System handles user registration, login, session management, and access control for the Airbnb Clone platform.

### 1.2 Functional Requirements

#### FR-1.1: User Registration
**Description**: Allow new users to create accounts as guests or hosts.

**User Story**: As a new user, I want to register an account so that I can access the platform.

**Business Rules**:
- Email must be unique across the system
- Password must meet security requirements
- Users must verify their email before full access
- Default role is 'guest' unless specified
- Users can upgrade from guest to host

---

#### FR-1.2: User Login
**Description**: Enable registered users to authenticate and access their accounts.

**User Story**: As a registered user, I want to log in securely so that I can use the platform.

**Business Rules**:
- Support email/password authentication
- Support OAuth 2.0 (Google, Facebook)
- Maximum 5 failed login attempts before account lockout
- Session expires after 24 hours of inactivity
- Support "Remember Me" functionality (30-day session)

---

#### FR-1.3: Password Management
**Description**: Allow users to reset forgotten passwords and change existing passwords.

**User Story**: As a user, I want to reset my password if I forget it.

**Business Rules**:
- Password reset link expires after 1 hour
- Old password cannot be reused
- Require email verification for password reset

---

### 1.3 API Endpoints

#### 1.3.1 Register User

**Endpoint**: `POST /api/v1/auth/register`

**Request Headers**:
```json
{
  "Content-Type": "application/json"
}
```

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "first_name": "Jeffrey",
  "last_name": "Eshun",
  "phone_number": "+1234567890",
  "role": "guest"
}
```

**Response (201 Created)**:
```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "user": {
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@example.com",
      "first_name": "Jeffrey",
      "last_name": "Eshun",
      "role": "guest",
      "created_at": "2024-10-22T10:30:00Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_at": "2024-10-23T10:30:00Z"
  }
}
```

**Error Response (400 Bad Request)**:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email already exists",
    "details": {
      "field": "email",
      "issue": "duplicate"
    }
  }
}
```

**Validation Rules**:
- `email`: Required, valid email format, max 255 characters, unique
- `password`: Required, min 8 characters, must contain uppercase, lowercase, number, special character
- `first_name`: Required, min 2 characters, max 100 characters, letters only
- `last_name`: Required, min 2 characters, max 100 characters, letters only
- `phone_number`: Optional, valid phone format
- `role`: Required, enum ['guest', 'host'], default 'guest'

---

#### 1.3.2 Login User

**Endpoint**: `POST /api/v1/auth/login`

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@example.com",
      "first_name": "Jeffrey",
      "last_name": "Eshun",
      "role": "guest"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_at": "2024-10-23T10:30:00Z"
  }
}
```

**Error Response (401 Unauthorized)**:
```json
{
  "success": false,
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "message": "Invalid email or password",
    "remaining_attempts": 4
  }
}
```

---

#### 1.3.3 OAuth Login

**Endpoint**: `POST /api/v1/auth/oauth/{provider}`

**Path Parameters**:
- `provider`: enum ['google', 'facebook']

**Request Body**:
```json
{
  "access_token": "oauth_provider_access_token",
  "id_token": "oauth_provider_id_token"
}
```

**Response**: Same as regular login

---

#### 1.3.4 Get Current User

**Endpoint**: `GET /api/v1/auth/me`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "user_id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "first_name": "Jeffrey",
    "last_name": "Eshun",
    "phone_number": "+1234567890",
    "role": "guest",
    "email_verified": true,
    "created_at": "2024-10-22T10:30:00Z"
  }
}
```

---

#### 1.3.5 Refresh Token

**Endpoint**: `POST /api/v1/auth/refresh`

**Request Body**:
```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "token": "new_jwt_token",
    "refresh_token": "new_refresh_token",
    "expires_at": "2024-10-23T10:30:00Z"
  }
}
```

---

#### 1.3.6 Logout

**Endpoint**: `POST /api/v1/auth/logout`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

### 1.4 Technical Specifications

#### Authentication Method
- **Primary**: JWT (JSON Web Tokens)
- **Token Type**: Bearer
- **Algorithm**: HS256 (HMAC with SHA-256)
- **Access Token Lifetime**: 24 hours
- **Refresh Token Lifetime**: 30 days

#### Password Security
- **Hashing Algorithm**: bcrypt
- **Salt Rounds**: 10
- **Minimum Complexity**: 
  - 8 characters minimum
  - At least 1 uppercase letter
  - At least 1 lowercase letter
  - At least 1 number
  - At least 1 special character (!@#$%^&*)

#### Session Management
- **Storage**: Redis (in-memory cache)
- **Session Timeout**: 24 hours (configurable)
- **Concurrent Sessions**: Max 3 devices per user

---

### 1.5 Performance Criteria

| Metric | Target | Critical Threshold |
|--------|--------|-------------------|
| Registration Response Time | < 500ms | < 1000ms |
| Login Response Time | < 300ms | < 500ms |
| Token Validation Time | < 50ms | < 100ms |
| Concurrent Users | 10,000 | 5,000 |
| Failed Login Rate | < 1% | < 5% |

---

### 1.6 Security Requirements

#### SEC-1.1: Data Encryption
- All passwords must be hashed using bcrypt
- Sensitive data encrypted in transit (HTTPS/TLS 1.3)
- JWT secrets stored in environment variables

#### SEC-1.2: Rate Limiting
- Login endpoint: 5 attempts per 15 minutes per IP
- Registration endpoint: 3 attempts per hour per IP
- Password reset: 3 requests per hour per email

#### SEC-1.3: Input Validation
- Server-side validation for all inputs
- Sanitize inputs to prevent SQL injection
- XSS protection on all text fields

---

### 1.7 Database Schema

```sql
CREATE TABLE User (
    user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    phone_number VARCHAR(20),
    role VARCHAR(20) NOT NULL CHECK (role IN ('guest', 'host', 'admin')),
    email_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP,
    failed_login_attempts INTEGER DEFAULT 0,
    locked_until TIMESTAMP,
    INDEX idx_email (email),
    INDEX idx_role (role)
);
```

---

## 2. Property Management System

### 2.1 Overview
The Property Management System enables hosts to create, update, and manage property listings on the platform.

### 2.2 Functional Requirements

#### FR-2.1: Create Property Listing
**Description**: Hosts can create new property listings with details, photos, and pricing.

**User Story**: As a host, I want to list my property so that guests can book it.

**Business Rules**:
- Only users with 'host' or 'admin' role can create listings
- Minimum 1 photo required, maximum 20 photos
- Price must be greater than $10 per night
- Location must include city, state, and country
- Property must have at least 1 amenity selected

---

#### FR-2.2: Update Property Listing
**Description**: Hosts can modify their existing property listings.

**User Story**: As a host, I want to update my property details so that information remains accurate.

**Business Rules**:
- Only the property owner or admin can edit
- Cannot modify property if active bookings exist (price changes)
- Changes logged for audit trail

---

#### FR-2.3: Delete Property Listing
**Description**: Hosts can remove their property listings.

**User Story**: As a host, I want to delete my listing when I no longer want to rent it.

**Business Rules**:
- Cannot delete if future bookings exist
- Soft delete (mark as inactive) for historical data
- Notify guests with active bookings (if any)

---

### 2.3 API Endpoints

#### 2.3.1 Create Property

**Endpoint**: `POST /api/v1/properties`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}",
  "Content-Type": "application/json"
}
```

**Request Body**:
```json
{
  "name": "Cozy Downtown Apartment",
  "description": "Beautiful 2-bedroom apartment in the heart of the city",
  "location": {
    "street_address": "123 Main Street",
    "city": "New York",
    "state": "New York",
    "country": "USA",
    "postal_code": "10001",
    "latitude": 40.7589,
    "longitude": -73.9851
  },
  "pricepernight": 150.00,
  "property_type": "apartment",
  "max_guests": 4,
  "bedrooms": 2,
  "bathrooms": 1,
  "amenities": ["wifi", "kitchen", "parking", "air_conditioning"],
  "house_rules": "No smoking, No pets",
  "cancellation_policy": "flexible"
}
```

**Response (201 Created)**:
```json
{
  "success": true,
  "message": "Property created successfully",
  "data": {
    "property_id": "650e8400-e29b-41d4-a716-446655440001",
    "host_id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Cozy Downtown Apartment",
    "location": {
      "location_id": "750e8400-e29b-41d4-a716-446655440002",
      "city": "New York",
      "state": "New York",
      "country": "USA"
    },
    "pricepernight": 150.00,
    "status": "pending_review",
    "created_at": "2024-10-22T10:30:00Z"
  }
}
```

**Validation Rules**:
- `name`: Required, min 10 characters, max 255 characters
- `description`: Required, min 50 characters, max 5000 characters
- `pricepernight`: Required, numeric, min 10.00, max 10000.00
- `location.city`: Required, min 2 characters
- `location.country`: Required, must be valid country code
- `property_type`: Required, enum ['apartment', 'house', 'villa', 'condo', 'cabin']
- `max_guests`: Required, integer, min 1, max 50
- `amenities`: Required, array, min 1 item

---

#### 2.3.2 Get All Properties

**Endpoint**: `GET /api/v1/properties`

**Query Parameters**:
```
?page=1
&limit=20
&city=New York
&min_price=50
&max_price=200
&guests=2
&amenities=wifi,parking
&property_type=apartment
&sort_by=price
&order=asc
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "properties": [
      {
        "property_id": "650e8400-e29b-41d4-a716-446655440001",
        "name": "Cozy Downtown Apartment",
        "location": {
          "city": "New York",
          "state": "New York",
          "country": "USA"
        },
        "pricepernight": 150.00,
        "rating": 4.8,
        "review_count": 24,
        "main_image": "https://cdn.example.com/property1.jpg",
        "amenities": ["wifi", "kitchen", "parking"]
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 95,
      "items_per_page": 20
    }
  }
}
```

---

#### 2.3.3 Get Property by ID

**Endpoint**: `GET /api/v1/properties/{property_id}`

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "property_id": "650e8400-e29b-41d4-a716-446655440001",
    "host": {
      "host_id": "550e8400-e29b-41d4-a716-446655440000",
      "first_name": "Jeffrey",
      "last_name": "Eshun",
      "member_since": "2023-01-15",
      "response_rate": 98,
      "rating": 4.9
    },
    "name": "Cozy Downtown Apartment",
    "description": "Beautiful 2-bedroom apartment...",
    "location": {
      "street_address": "123 Main Street",
      "city": "New York",
      "state": "New York",
      "country": "USA",
      "postal_code": "10001",
      "latitude": 40.7589,
      "longitude": -73.9851
    },
    "pricepernight": 150.00,
    "property_type": "apartment",
    "max_guests": 4,
    "bedrooms": 2,
    "bathrooms": 1,
    "amenities": ["wifi", "kitchen", "parking", "air_conditioning"],
    "images": [
      {
        "image_id": "img1",
        "url": "https://cdn.example.com/property1_1.jpg",
        "caption": "Living room",
        "order": 1
      }
    ],
    "house_rules": "No smoking, No pets",
    "cancellation_policy": "flexible",
    "rating": 4.8,
    "review_count": 24,
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-10-20T14:20:00Z"
  }
}
```

---

#### 2.3.4 Update Property

**Endpoint**: `PUT /api/v1/properties/{property_id}`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}",
  "Content-Type": "application/json"
}
```

**Request Body**: Same structure as Create Property (partial updates supported with PATCH)

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Property updated successfully",
  "data": {
    "property_id": "650e8400-e29b-41d4-a716-446655440001",
    "updated_fields": ["pricepernight", "amenities"],
    "updated_at": "2024-10-22T15:45:00Z"
  }
}
```

---

#### 2.3.5 Delete Property

**Endpoint**: `DELETE /api/v1/properties/{property_id}`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Property deleted successfully"
}
```

**Error Response (400 Bad Request)**:
```json
{
  "success": false,
  "error": {
    "code": "ACTIVE_BOOKINGS_EXIST",
    "message": "Cannot delete property with active bookings",
    "details": {
      "active_bookings": 3,
      "next_available_date": "2024-12-15"
    }
  }
}
```

---

#### 2.3.6 Upload Property Images

**Endpoint**: `POST /api/v1/properties/{property_id}/images`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}",
  "Content-Type": "multipart/form-data"
}
```

**Request Body (Form Data)**:
- `images`: File[] (max 10 files per request)
- `captions`: String[] (optional)

**Response (201 Created)**:
```json
{
  "success": true,
  "message": "Images uploaded successfully",
  "data": {
    "images": [
      {
        "image_id": "img123",
        "url": "https://cdn.example.com/property1_new.jpg",
        "thumbnail_url": "https://cdn.example.com/property1_new_thumb.jpg",
        "order": 5
      }
    ]
  }
}
```

**Validation Rules**:
- File types: JPEG, PNG, WebP only
- Max file size: 10MB per image
- Min dimensions: 800x600 pixels
- Max dimensions: 5000x5000 pixels
- Max 20 images per property

---

### 2.4 Technical Specifications

#### Image Storage
- **Service**: AWS S3 / Cloudinary
- **CDN**: CloudFront / Cloudinary CDN
- **Format**: Original + WebP conversion
- **Sizes**: Original, Large (1920px), Medium (1280px), Thumbnail (400px)
- **Optimization**: Automatic compression, lazy loading

#### Geolocation
- **Service**: Google Maps API / Mapbox
- **Geocoding**: Address to coordinates
- **Reverse Geocoding**: Coordinates to address
- **Search Radius**: Calculate distances for proximity search

---

### 2.5 Performance Criteria

| Metric | Target | Critical Threshold |
|--------|--------|-------------------|
| Property Creation Time | < 1000ms | < 2000ms |
| Property Listing Load | < 200ms | < 500ms |
| Search Response Time | < 300ms | < 800ms |
| Image Upload Time | < 3s per image | < 5s per image |
| Concurrent Property Views | 50,000 | 25,000 |

---

### 2.6 Database Schema

```sql
CREATE TABLE Location (
    location_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    street_address VARCHAR(255),
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100) NOT NULL,
    country VARCHAR(100) NOT NULL,
    postal_code VARCHAR(20),
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_city (city),
    INDEX idx_coordinates (latitude, longitude)
);

CREATE TABLE Property (
    property_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    host_id UUID NOT NULL,
    location_id UUID NOT NULL,
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    pricepernight DECIMAL(10, 2) NOT NULL,
    property_type VARCHAR(50) NOT NULL,
    max_guests INTEGER NOT NULL,
    bedrooms INTEGER NOT NULL,
    bathrooms INTEGER NOT NULL,
    amenities JSONB,
    house_rules TEXT,
    cancellation_policy VARCHAR(50),
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (host_id) REFERENCES User(user_id),
    FOREIGN KEY (location_id) REFERENCES Location(location_id),
    INDEX idx_host (host_id),
    INDEX idx_location (location_id),
    INDEX idx_price (pricepernight),
    INDEX idx_status (status)
);
```

---

## 3. Booking System

### 3.1 Overview
The Booking System manages property reservations, including creation, confirmation, cancellation, and tracking of bookings.

### 3.2 Functional Requirements

#### FR-3.1: Create Booking
**Description**: Guests can book available properties for specific dates.

**User Story**: As a guest, I want to book a property for my travel dates.

**Business Rules**:
- Property must be available for selected dates
- No overlapping bookings allowed
- Minimum 1-night stay required
- Maximum 365-night stay allowed
- Booking requires immediate payment
- Guest count must not exceed property capacity

---

#### FR-3.2: Confirm Booking
**Description**: Hosts can confirm or reject pending booking requests.

**User Story**: As a host, I want to review and approve booking requests.

**Business Rules**:
- Host has 24 hours to respond to requests
- Auto-confirm for instant book properties
- Payment captured only after confirmation

---

#### FR-3.3: Cancel Booking
**Description**: Guests and hosts can cancel bookings based on cancellation policy.

**User Story**: As a guest, I want to cancel my booking if plans change.

**Business Rules**:
- Refund amount based on cancellation policy
- Flexible: Full refund if cancelled 24 hours before check-in
- Moderate: 50% refund if cancelled 5 days before check-in
- Strict: No refund if cancelled within 7 days of check-in

---

### 3.3 API Endpoints

#### 3.3.1 Create Booking

**Endpoint**: `POST /api/v1/bookings`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}",
  "Content-Type": "application/json"
}
```

**Request Body**:
```json
{
  "property_id": "650e8400-e29b-41d4-a716-446655440001",
  "start_date": "2024-12-20",
  "end_date": "2024-12-25",
  "guests": 2,
  "special_requests": "Early check-in if possible"
}
```

**Response (201 Created)**:
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "booking_id": "850e8400-e29b-41d4-a716-446655440003",
    "property_id": "650e8400-e29b-41d4-a716-446655440001",
    "property_name": "Cozy Downtown Apartment",
    "host_name": "John Doe",
    "start_date": "2024-12-20",
    "end_date": "2024-12-25",
    "nights": 5,
    "guests": 2,
    "pricing": {
      "price_per_night": 150.00,
      "subtotal": 750.00,
      "service_fee": 75.00,
      "taxes": 82.50,
      "total_price": 907.50
    },
    "status": "pending",
    "payment_required": true,
    "payment_deadline": "2024-10-22T11:00:00Z",
    "created_at": "2024-10-22T10:30:00Z"
  }
}
```

**Validation Rules**:
- `property_id`: Required, must exist, must be active
- `start_date`: Required, date format, must be future date, cannot be more than 365 days ahead
- `end_date`: Required, date format, must be after start_date
- `guests`: Required, integer, min 1, max property.max_guests
- `special_requests`: Optional, max 500 characters

---

#### 3.3.2 Get Booking by ID

**Endpoint**: `GET /api/v1/bookings/{booking_id}`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "booking_id": "850e8400-e29b-41d4-a716-446655440003",
    "property": {
      "property_id": "650e8400-e29b-41d4-a716-446655440001",
      "name": "Cozy Downtown Apartment",
      "address": "123 Main Street, New York, NY",
      "main_image": "https://cdn.example.com/property1.jpg"
    },
    "guest": {
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "first_name": "Jeffrey",
      "last_name": "Eshun",
      "email": "user@example.com",
      "phone": "+1234567890"
    },
    "host": {
      "user_id": "550e8400-e29b-41d4-a716-446655440099",
      "first_name": "Jeffrey",
      "last_name": "Eshun",
      "email": "jeffrey@example.com"
    },
    "start_date": "2024-12-20",
    "end_date": "2024-12-25",
    "nights": 5,
    "guests": 2,
    "pricing": {
      "price_per_night": 150.00,
      "subtotal": 750.00,
      "service_fee": 75.00,
      "taxes": 82.50,
      "total_price": 907.50
    },
    "special_requests": "Early check-in if possible",
    "status": "confirmed",
    "cancellation_policy": "flexible",
    "created_at": "2024-10-22T10:30:00Z",
    "confirmed_at": "2024-10-22T11:15:00Z"
  }
}
```

---

#### 3.3.3 Get User Bookings

**Endpoint**: `GET /api/v1/bookings`

**Query Parameters**:
```
?status=confirmed
&start_date_from=2024-10-01
&start_date_to=2024-12-31
&page=1
&limit=20
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "bookings": [
      {
        "booking_id": "850e8400-e29b-41d4-a716-446655440003",
        "property_name": "Cozy Downtown Apartment",
        "start_date": "2024-12-20",
        "end_date": "2024-12-25",
        "total_price": 907.50,
        "status": "confirmed",
        "property_image": "https://cdn.example.com/property1.jpg"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 3,
      "total_items": 45,
      "items_per_page": 20
    }
  }
}
```

---

#### 3.3.4 Confirm Booking (Host)

**Endpoint**: `POST /api/v1/bookings/{booking_id}/confirm`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Booking confirmed successfully",
  "data": {
    "booking_id": "850e8400-e29b-41d4-a716-446655440003",
    "status": "confirmed",
    "confirmed_at": "2024-10-22T11:15:00Z"
  }
}
```

---

#### 3.3.5 Cancel Booking

**Endpoint**: `POST /api/v1/bookings/{booking_id}/cancel`

**Request Headers**:
```json
{
  "Authorization": "Bearer {jwt_token}",
  "Content-Type": "application/json"
}
```

**Request Body**:
```json
{
  "reason": "Change of plans",
  "detailed_reason": "Unexpected work commitment"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "message": "Booking cancelled successfully",
  "data": {
    "booking_id": "850e8400-e29b-41d4-a716-446655440003",
    "status": "canceled",
    "cancellation": {
      "canceled_at
