# My Properties API Documentation

## Overview

The My Properties API endpoint provides functionality for managing a user's property portfolio. Users can retrieve, create, update, and delete properties associated with their account. Access is restricted to specific membership levels.

## Base URL

```
GET/POST/DELETE /api/member/my-properties
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

### GET - Retrieve Properties

Retrieves all properties associated with the authenticated user.

#### Success Response (200)

```json
[
  {
    "id": "unique-property-id",
    "propertyId": "external-property-identifier",
    "userId": "user-identifier"
    // Additional property details
  }
]
```

### POST - Create or Update Property

Supports two scenarios:

1. Create a new property
2. Update an existing property

#### Request Body for Creating a Property

```json
{
  "propertyId": "required-external-identifier"
  // Other property details
}
```

#### Request Body for Updating a Property

```json
{
  "id": "existing-property-id"
  // Fields to update
}
```

#### Success Responses

- Create: 201 Status Code

```json
{
  "success": true,
  "property": {
    // Created property details
  }
}
```

- Update: 200 Status Code

```json
{
  "success": true,
  "property": {
    // Updated property details
  }
}
```

### DELETE - Remove a Property

Removes a specific property from the user's portfolio.

#### Query Parameters

- `propertyId`: External identifier of the property to delete

#### Success Response

- 200 Status Code (No content)

## Error Responses

### 400 - Bad Request

```json
{
  "error": "Invalid input data",
  "message": "Specific validation error"
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
  "error": "Property not found"
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

### Retrieve Properties

```bash
curl -X GET "http://api.hotspotting.com.au/api/member/my-properties" \
  -H "Authorization: Bearer your_access_token"
```

### Create a Property

```bash
curl -X POST "http://api.hotspotting.com.au/api/member/my-properties" \
  -H "Authorization: Bearer your_access_token" \
  -H "Content-Type: application/json" \
  -d '{
    "propertyId": "PROP123456",
    "address": "123 Example Street",
    "type": "Residential"
  }'
```

### Update a Property

```bash
curl -X POST "http://api.hotspotting.com.au/api/member/my-properties" \
  -H "Authorization: Bearer your_access_token" \
  -H "Content-Type: application/json" \
  -d '{
    "id": "existing-property-id",
    "address": "Updated Address"
  }'
```

### Delete a Property

```bash
curl -X DELETE "http://api.hotspotting.com.au/api/member/my-properties?propertyId=PROP123456" \
  -H "Authorization: Bearer your_access_token"
```

## PropTrack API Endpoints

### AVM PDF Generation

Generate a PDF valuation report for a specific property.

```bash
curl --location 'http://api.hotspotting.com.au/api/member/proptrack/avm-pdf' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain' \
--data '{
  "valuationId": "0d7faeb0-3525-4a40-9bd3-40426c319a15",
  "propertyId": "6651421"
}'
```

### AVM (Automated Valuation Model)

Retrieve property valuation details.

```bash
curl --location 'http://api.hotspotting.com.au/api/member/proptrack/avm' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain' \
--data '{
  "fullAddress": "LOT 1,UNIT 1,80 ANN STREET,SOUTH GLADSTONE QLD 4680",
  "propertyId": "4982697"
}'
```

### Property Suggestions

Get property suggestions based on a search query.

```bash
curl --location 'http://api.hotspotting.com.au/api/member/proptrack/suggest?q=spr' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain'
```

### Valuation Details

Retrieve detailed valuation information.

```bash
curl --location 'http://api.hotspotting.com.au/api/member/proptrack/valuation' \
--header 'Authorization: Bearer your_access_token' \
--header 'Content-Type: text/plain' \
--data '{
  "valuationId": "0d7faeb0-3525-4a40-9bd3-40426c319a15",
  "propertyId": "6651421"
}'
```

## Notes

1. All operations require authentication via the `withAuth` middleware
2. User ID is automatically extracted from the authenticated request
3. Property operations are scoped to the authenticated user
4. Validation checks are performed for input data
5. Detailed error handling is implemented for various scenarios
