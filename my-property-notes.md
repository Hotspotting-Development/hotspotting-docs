# My Property Notes API Documentation

## Overview

The My Property Notes API endpoint provides functionality for managing notes associated with specific properties. Users can retrieve, create, update, and delete property-specific notes. Access is restricted to specific membership levels.

## Base URL

```
GET/POST/DELETE /api/member/my-properties/notes
```

## Authentication

**Required**: Bearer token in Authorization header. The endpoint is protected and requires specific membership levels.

```
Authorization: Bearer <your-access-token>
```

## Membership Levels

The API supports the following membership tiers:

### Enterprise Memberships

- `enterprise-membership`
- `property-pro-membership`

### Premium Memberships

- `premium-membership-gold`
- `premium-membership-gold-monthly`
- `premium-membership-gold-resubscription`
- `premium-membership-gold-quarterly`
- `premium-membership-new`
- `hotspotters-annual-membership`

## Endpoints and Methods

### GET - Retrieve Property Notes

Retrieves all notes for a specific property.

#### Query Parameters

- `propertyId`: **Required** External identifier of the property

#### Success Response (200)

```json
[
  {
    "id": "unique-note-id",
    "propertyId": "property-identifier",
    "userId": "user-identifier",
    "notes": "Note content",
    "name": "User Name",
    "email": "user@example.com"
  }
]
```

### POST - Create or Update Property Note

Supports two scenarios:

1. Create a new property note
2. Update an existing property note

#### Request Body for Creating a Note

```json
{
  "propertyId": "required-property-identifier",
  "notes": "Note content",
  "name": "User Name",
  "email": "user@example.com"
}
```

#### Request Body for Updating a Note

```json
{
  "id": "existing-note-id",
  "notes": "Updated note content",
  "name": "User Name",
  "email": "user@example.com"
}
```

#### Success Responses

- Create: 201 Status Code

```json
{
  "success": true,
  "note": {
    // Created note details
  }
}
```

- Update: 200 Status Code

```json
{
  "success": true,
  "note": {
    // Updated note details
  }
}
```

### DELETE - Remove a Property Note

Removes a specific property note.

#### Query Parameters

- `notesId`: External identifier of the note to delete

#### Success Response

- 200 Status Code (No content)

## Error Responses

### 400 - Bad Request

```json
{
  "error": "Invalid input data",
  "message": "Property ID is required" or "Invalid input data for update"
}
```

### 401 - Unauthorized

```json
{
  "success": false,
  "error": "Unauthorized",
  "message": "Unable to retrieve user ID"
}
```

### 404 - Not Found

```json
{
  "error": "Note not found"
}
```

### 405 - Method Not Allowed

```json
{
  "success": false,
  "error": "Method Not Allowed",
  "message": "Only GET, POST, and DELETE methods are supported"
}
```

### 500 - Server Error

```json
{
  "success": false,
  "error": "Server Error",
  "message": "Detailed error information"
}
```

## Example Requests

### Retrieve Property Notes

```bash
curl --location 'http://api.hotspotting.com.au/api/member/my-properties/notes?propertyId=13004165' \
--header 'Authorization: Bearer your_access_token'
```

### Create a New Property Note

```bash
curl --location 'http://api.hotspotting.com.au/api/member/my-properties/notes' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "propertyId":"13004165",
  "notes":"test api live",
  "name":"Joseph Charles Vargas",
  "email":"charles@hotspotting.com.au"
}'
```

### Update an Existing Property Note

```bash
curl --location 'http://api.hotspotting.com.au/api/member/my-properties/notes' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "id": "68ed9f1ae024cd0933d8eb6f",
  "notes": "test add edited API Request",
  "name": "Joseph Charles Vargas",
  "email": "charles@hotspotting.com.au",
  "userId": "2553",
  "propertyId": "13004165"
}'
```

## Notes

1. All operations require authentication via the `withAuth` middleware
2. User ID is automatically extracted from the authenticated request
3. Note operations are scoped to the authenticated user
4. Validation checks are performed for input data
5. Detailed error handling is implemented for various scenarios
6. Each note is associated with a specific property and user
