# Hotspotting API Documentation

## Base URL

```
https://dev-api.hotspotting.com.au/api
```

## Authentication Endpoints

### 1. User Login

**Endpoint:** `POST /auth/login`

**Description:** Authenticates a user with username and password, returns a JWT session token.

#### Request Body

```json
{
  "username": "string",
  "password": "string"
}
```

#### Response

**Success (200)**

```json
{
  "success": true,
  "sessionToken": "jwt_token_string",
  "user": {
    "userId": "number",
    "username": "string",
    "email": "string",
    "displayName": "string",
    "memberships": "array",
    "membershipPlans": "array"
  }
}
```

**Error (400)**

```json
{
  "error": "Username and password are required"
}
```

**Error (401)**

```json
{
  "error": "Authentication failed"
}
```

**Error (500)**

```json
{
  "error": "Authentication failed",
  "message": "error_details"
}
```

#### Example Request

```bash
curl -X POST https://dev-api.hotspotting.com.au/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your_username",
    "password": "your_password"
  }'
```

---

### 2. Session Validation

**Endpoint:** `GET /auth/session`

**Description:** Validates an existing JWT session token and returns user information.

#### Headers

```
Authorization: Bearer <jwt_token>
```

#### Response

**Success (200)**

```json
{
  "valid": true,
  "user": {
    "userId": "number",
    "username": "string",
    "email": "string",
    "displayName": "string",
    "memberships": "array",
    "membershipPlans": "array"
  },
  "username": "string"
}
```

**Error (401)**

```json
{
  "error": "Authorization header with Bearer token required",
  "valid": false
}
```

```json
{
  "error": "Session has expired",
  "valid": false
}
```

```json
{
  "error": "Invalid session",
  "valid": false
}
```

#### Example Request

```bash
curl -X GET https://dev-api.hotspotting.com.au/api/auth/session \
  -H "Authorization: Bearer your_jwt_token_here"
```

---

### 3. Token Verification

**Endpoint:** `POST /auth/verify` or `GET /auth/verify`

**Description:** Verifies a JWT token and returns the decoded payload information.

#### POST Method

**Request Body**

```json
{
  "token": "jwt_token_string"
}
```

#### GET Method

**Headers**

```
Authorization: Bearer <jwt_token>
```

#### Response

**Success (200)**

```json
{
  "valid": true,
  "user": {
    "username": "string",
    "user": {
      "userId": "number",
      "username": "string",
      "email": "string",
      "displayName": "string",
      "memberships": "array",
      "membershipPlans": "array"
    }
  }
}
```

**Error (400)** _(POST only)_

```json
{
  "error": "Token is required"
}
```

**Error (401)**

```json
{
  "error": "Authorization header with Bearer token required"
}
```

```json
{
  "error": "Token has expired",
  "valid": false
}
```

```json
{
  "error": "Invalid token signature",
  "valid": false
}
```

```json
{
  "error": "Invalid token",
  "valid": false
}
```

#### Example Requests

**POST Method:**

```bash
curl -X POST https://dev-api.hotspotting.com.au/api/auth/verify \
  -H "Content-Type: application/json" \
  -d '{
    "token": "your_jwt_token_here"
  }'
```

**GET Method:**

```bash
curl -X GET https://dev-api.hotspotting.com.au/api/auth/verify \
  -H "Authorization: Bearer your_jwt_token_here"
```

---

## Authentication Flow

1. **Login:** Use `POST /auth/login` with username and password
2. **Store Token:** Save the returned `sessionToken` for subsequent requests
3. **Validate Session:** Use `GET /auth/session` to check if token is still valid
4. **Verify Token:** Use `/auth/verify` for additional token validation if needed

## Error Handling

All endpoints return appropriate HTTP status codes:

- `200` - Success
- `400` - Bad Request (missing required fields)
- `401` - Unauthorized (invalid credentials or expired token)
- `500` - Internal Server Error

## Token Expiration

JWT tokens are valid for **1 week** from the time of issuance. When a token expires, you'll receive a `401` status with an appropriate error message, and you'll need to login again to get a new token.
