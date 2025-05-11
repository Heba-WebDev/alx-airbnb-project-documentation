# Technical and Functional Requirements

## 1. User Authentication

### Description
Handles user registration and login to enable secure access to the system.

### Endpoints
- **POST** `/api/auth/register`  
  Registers a new user.

  **Input:**
  ```
  {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",
    "password": "securePassword123"
  }
  ```

  **Validation Rules:**

  - All fields are required.
  - Email must be unique and valid.
  - Password must be at least 8 characters.

  **Response:**
  ```
  {
    "message": "User registered successfully",
    "user_id": "uuid"
  }
  ```

- **POST** `/api/auth/login`  
  Authenticates user and returns JWT.

  **Input:**
  ```
  {
    "email": "john@example.com",
    "password": "securePassword123"
  }
  ```

  **Response:**
  ```
  {
    "token": "jwt_token_here",
    "user": {
      "user_id": "uuid",
      "role": "guest"
    }
  }
  ```

### Performance Criteria
- Response time must be under 300ms for 95% of requests.
- Rate limiting to prevent brute-force attacks.

---

## 2. Property Management

### Description

Allows hosts to create, update, view, and delete property listings.

### Endpoints
- **POST** `/api/properties`  
  Adds a new property.

  **Input:**
  ```
  {
    "host_id": "uuid",
    "name": "Modern Loft",
    "description": "Spacious and clean loft in city center.",
    "location": "Berlin",
    "pricepernight": 80
  }
  ```

  **Validation Rules:**

  - Name, description, location are required.
  - Price must be a positive number.
  - Host ID must exist and belong to a user with role = host.

  **Response:**
  ```
  {
    "message": "Property added successfully",
    "property_id": "uuid"
  }
  ```

- **GET** `/api/properties/{id}`
  Fetches property details by ID.

- **PUT** `/api/properties/{id}`
  Updates property details.

- **DELETE** `/api/properties/{id}`
  Deletes a property listing.

### Performance Criteria

- Optimized queries for properties list (use of indexes).
- Paginated results for listing all properties.

---

## 3. Booking System

### Description
Enables users to book properties for a specific date range and manage their bookings.

### Endpoints
- **POST** `/api/bookings`  
  Creates a new booking.

  **Input:**
  ```
  {
    "user_id": "uuid",
    "property_id": "uuid",
    "start_date": "2025-06-01",
    "end_date": "2025-06-05"
  }
  ```

  **Validation Rules:**

  - User and property IDs must exist.
  - Start date must be before end date.
  - No overlap with existing bookings for that property.

  **Response:**
  ```
  {
    "message": "Booking created successfully",
    "booking_id": "uuid",
    "total_price": 320
  }
  ```

- **GET** `/api/bookings/user/{user_id}`
  Lists all bookings for a specific user.

### Performance Criteria

- Prevent race conditions with transactions when booking.
- Ensure availability check completes in under 500ms.
